# Webhooks

The Moneta Webhook System allows you to receive real-time notifications about events in your Moneta account. When specific events occur (such as successful payments, failed transactions, or account updates), Moneta will send `HTTP POST` requests to your configured webhook endpoints.

#### Key Features

* **Secure Delivery**: All webhooks are signed with HMAC signatures
* **Reliable Retry Logic**: Failed webhooks are automatically retried with exponential backoff
* **Dual Environment Support**: Separate URLs for production and test environments
* **Comprehensive Logging**: Detailed logs of all webhook attempts
* **Flexible Configuration**: Customizable timeout, retry attempts, and headers

***

### Dashboard Configuration

#### Accessing Webhook Settings

1. **Login to your Moneta Dashboard**
2. **Navigate to Settings → Webhooks** (or use the API endpoints)
3. **Configure your webhook endpoints**

#### Configuration Options

**1. Webhook URLs**

* **URL:** Your webhook url is dependent on the environment you are using.&#x20;
* **Requirements**:
  * Must be a valid HTTPS URL (HTTP allowed for development only, localhost is not allowed for webhook)
  * Must be publicly accessible
  * Should respond within the configured timeout period

**2. Security Settings**

* **Client Secret**: Used for generating webhook signatures (auto-generated)
* **Signature Algorithm**: All webhook responses uses `sha256` encryption system for encoding X-Moneta-Signature header together with your payload

**3. Delivery Settings**

* **Timeout**: How long to wait for your server to respond (default: 30)
* **Max Retry Attempts**: Number of retry attempts for failed webhooks ( default: 3)
* **Retry Backoff Multiplier**: Exponential backoff multiplier ( default: 2.0)



### Webhook Message Format

#### HTTP Request Details

All webhook requests are sent as HTTP POST requests with the following characteristics:

* **Method**: POST
* **Content-Type**: application/json
* **User-Agent**: Moneta-Webhook/1.0
* **Headers**: Include signature and any custom headers you've configured

#### Standard Payload Structure

```json
{
  "event": "transaction.completed",
  "event_id": "evt_1234567890abcdef",
  "timestamp": "2024-01-22T10:30:00Z",
  "data": {
    "object": "transaction",
    "id": "txn_1234567890abcdef",
    "reference": "TXN-REF-123456",
    "amount": 50000,
    "currency": "NGN",
    "status": "completed",
    "customer": {
      "id": "cust_1234567890abcdef",
      "email": "customer@example.com",
      "name": "John Doe"
    },
    "payment_method": {
      "type": "card",
      "last4": "1234",
      "brand": "visa"
    },
    "metadata": {
      "order_id": "ORD-123456",
      "custom_field": "custom_value"
    },
    "created_at": "2024-01-22T10:25:00Z",
    "updated_at": "2024-01-22T10:30:00Z"
  },
  "meta": {
    "webhook_id": "wh_1234567890abcdef",
    "delivery_attempt": 1,
    "user_id": 12345
  }
}
```

#### Event Types

Common webhook events include:

**Transaction Events**

* `transaction.created` - New transaction initiated
* `transaction.completed` - Transaction successfully completed
* `transaction.failed` - Transaction failed
* `transaction.cancelled` - Transaction was cancelled
* `transaction.refunded` - Transaction was refunded



#### Test Payload Example

When testing your webhook endpoint from the settings console page, Moneta sends a test payload in the format below to your webhook:

```json
{
  "test": true,
  "message": "This is a test webhook from Moneta",
  "timestamp": "2024-01-22T10:30:00Z",
  "user_id": 12345,
  "event": "webhook.test",
  "data": {
    "test_parameter": "test_value"
  }
}
```

***

### Signature Verification

For every transaction response you receive from your webhook url, it is expidient that you validate the origin of this request.

#### Why Verify Signatures?

Webhook signatures ensure that:

1. The request actually came from Moneta
2. The payload hasn't been tampered with
3. The request isn't a replay attack

#### Signature Header Format

Moneta includes a signature in the `X-Moneta-Signature` header:

```
X-Moneta-Signature: sha256=a1b2c3d4e5f6...
```

The format is: `{algorithm}={signature}`

#### How Signatures Are Generated

1. **Payload Preparation**: The JSON payload is serialized consistently
2. **HMAC Generation**: An HMAC is generated using your client mac and payload
3. **Header Addition**: The signature is added to the `X-Moneta-Signature` header

#### Verification Process

1. **Extract the signature** from the `X-Moneta-Signature` header
2. **Parse the algorithm and signature** (format: `algorithm=signature`)
3. **Generate your own signature** using the same algorithm and your client secret
4. **Compare signatures** using a timing-safe comparison function

***

### Server Implementation Examples

{% tabs %}
{% tab title="Express (JavaScript)" %}
```javascript
const express = require('express');
const crypto = require('crypto');
const app = express();

// Middleware to capture raw body for signature verification
app.use('/webhooks', express.raw({ type: 'application/json' }));

// Your client secret from Moneta dashboard
const CLIENT_MAC = 'your_client_mac_here';

function verifyWebhookSignature(payload, signature, secret) {
  // Parse the signature header (format: algorithm=signature)
  const [algorithm, receivedSignature] = signature.split('=');
  
  if (!algorithm || !receivedSignature) {
    return false;
  }
  
  // Generate expected signature
  const expectedSignature = crypto
    .createHmac(algorithm, secret)
    .update(payload, 'utf8')
    .digest('hex');
  
  // Use timing-safe comparison
  return crypto.timingSafeEqual(
    Buffer.from(receivedSignature, 'hex'),
    Buffer.from(expectedSignature, 'hex')
  );
}

app.post('/webhooks/moneta', (req, res) => {
  const signature = req.headers['x-moneta-signature'];
  const payload = req.body;
  
  // Verify the signature
  if (!signature || !verifyWebhookSignature(payload, signature, CLIENT_MAC)) {
    console.log('Invalid webhook signature');
    return res.status(401).send('Unauthorized');
  }
  
  // Parse the JSON payload
  const event = JSON.parse(payload.toString());
  
  // Process the webhook event
  console.log('Received webhook:', event.event);
  console.log('Event data:', event.data);
  
  // Handle different event types
  switch (event.event) {
    case 'transaction.completed':
      handleTransactionCompleted(event.data);
      break;
    case 'transaction.failed':
      handleTransactionFailed(event.data);
      break;
    case 'webhook.test':
      console.log('Test webhook received successfully');
      break;
    default:
      console.log('Unhandled event type:', event.event);
  }
  
  // Always respond with 200 OK to acknowledge receipt
  res.status(200).send('OK');
});

function handleTransactionCompleted(data) {
  console.log(`Transaction ${data.reference} completed for ${data.amount} ${data.currency}`);
  // Update your database, send confirmation emails, etc.
}

function handleTransactionFailed(data) {
  console.log(`Transaction ${data.reference} failed`);
  // Handle failed transaction logic
}

app.listen(3000, () => {
  console.log('Webhook server listening on port 3000');
});
```
{% endtab %}

{% tab title="Laravel (Php)" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Http\Response;
use Illuminate\Support\Facades\Log;

class MonetaWebhookController extends Controller
{
    private const CLIENT_MAC = 'your_client_mac_here';
    
    public function handle(Request $request): Response
    {
        $signature = $request->header('X-Moneta-Signature');
        $payload = $request->getContent();
        
        // Verify the signature
        if (!$signature || !$this->verifySignature($payload, $signature)) {
            Log::warning('Invalid Moneta webhook signature');
            return response('Unauthorized', 401);
        }
        
        // Parse the JSON payload
        $event = json_decode($payload, true);
        
        if (!$event) {
            Log::error('Invalid JSON in Moneta webhook');
            return response('Bad Request', 400);
        }
        
        // Log the event
        Log::info('Moneta webhook received', [
            'event' => $event['event'] ?? 'unknown',
            'event_id' => $event['event_id'] ?? null
        ]);
        
        // Handle different event types
        switch ($event['event'] ?? '') {
            case 'transaction.completed':
                $this->handleTransactionCompleted($event['data']);
                break;
            case 'transaction.failed':
                $this->handleTransactionFailed($event['data']);
                break;
            case 'webhook.test':
                Log::info('Test webhook received successfully');
                break;
            default:
                Log::warning('Unhandled Moneta webhook event', ['event' => $event['event'] ?? 'unknown']);
        }
        
        return response('OK', 200);
    }
    
    private function verifySignature(string $payload, string $signature): bool
    {
        // Parse the signature header (format: algorithm=signature)
        if (!str_contains($signature, '=')) {
            return false;
        }
        
        [$algorithm, $receivedSignature] = explode('=', $signature, 2);
        
        if (!in_array($algorithm, ['sha256', 'sha1', 'sha512', 'md5'])) {
            return false;
        }
        
        // Generate expected signature
        $expectedSignature = hash_hmac($algorithm, $payload, self::CLIENT_MAC);
        
        // Use timing-safe comparison
        return hash_equals($expectedSignature, $receivedSignature);
    }
    
    private function handleTransactionCompleted(array $data): void
    {
        Log::info('Transaction completed', [
            'reference' => $data['reference'] ?? null,
            'amount' => $data['amount'] ?? null,
            'currency' => $data['currency'] ?? null
        ]);
        
        // Update your database, send confirmation emails, etc.
        // Example:
        // Transaction::where('reference', $data['reference'])->update(['status' => 'completed']);
    }
    
    private function handleTransactionFailed(array $data): void
    {
        Log::info('Transaction failed', [
            'reference' => $data['reference'] ?? null
        ]);
        
        // Handle failed transaction logic
    }
}
```
{% endtab %}

{% tab title="Django (python)" %}
```python
import json
import hmac
import hashlib
import logging
from django.http import HttpResponse, HttpResponseBadRequest
from django.views.decorators.csrf import csrf_exempt
from django.views.decorators.http import require_http_methods
from django.utils.decorators import method_decorator
from django.views import View

logger = logging.getLogger(__name__)

CLIENT_MAC = 'your_client_mac_here'

class MonetaWebhookView(View):
    
    @method_decorator(csrf_exempt)
    @method_decorator(require_http_methods(["POST"]))
    def dispatch(self, *args, **kwargs):
        return super().dispatch(*args, **kwargs)
    
    def post(self, request):
        signature = request.META.get('HTTP_X_MONETA_SIGNATURE')
        payload = request.body
        
        # Verify the signature
        if not signature or not self.verify_signature(payload, signature):
            logger.warning('Invalid Moneta webhook signature')
            return HttpResponse('Unauthorized', status=401)
        
        # Parse the JSON payload
        try:
            event = json.loads(payload.decode('utf-8'))
        except json.JSONDecodeError:
            logger.error('Invalid JSON in Moneta webhook')
            return HttpResponseBadRequest('Invalid JSON')
        
        # Log the event
        logger.info(f"Moneta webhook received: {event.get('event', 'unknown')}")
        
        # Handle different event types
        event_type = event.get('event', '')
        if event_type == 'transaction.completed':
            self.handle_transaction_completed(event.get('data', {}))
        elif event_type == 'transaction.failed':
            self.handle_transaction_failed(event.get('data', {}))
        elif event_type == 'webhook.test':
            logger.info('Test webhook received successfully')
        else:
            logger.warning(f'Unhandled Moneta webhook event: {event_type}')
        
        return HttpResponse('OK', status=200)
    
    def verify_signature(self, payload, signature):
        """Verify the webhook signature"""
        if '=' not in signature:
            return False
        
        algorithm, received_signature = signature.split('=', 1)
        
        if algorithm not in ['sha256', 'sha1', 'sha512', 'md5']:
            return False
        
        # Generate expected signature
        hash_func = getattr(hashlib, algorithm)
        expected_signature = hmac.new(
            CLIENT_MAC.encode('utf-8'),
            payload,
            hash_func
        ).hexdigest()
        
        # Use timing-safe comparison
        return hmac.compare_digest(expected_signature, received_signature)
    
    def handle_transaction_completed(self, data):
        """Handle completed transaction"""
        logger.info(f"Transaction completed: {data.get('reference')} for {data.get('amount')} {data.get('currency')}")
        # Update your database, send confirmation emails, etc.
    
    def handle_transaction_failed(self, data):
        """Handle failed transaction"""
        logger.info(f"Transaction failed: {data.get('reference')}")
        # Handle failed transaction logic
```
{% endtab %}

{% tab title="Spring Boot (Java)" %}
```java
package com.example.webhooks;

import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Arrays;

@RestController
@RequestMapping("/webhooks")
public class MonetaWebhookController {
    
    private static final Logger logger = LoggerFactory.getLogger(MonetaWebhookController.class);
    private static final String CLIENT_MAC = "your_client_mac_here";
    private final ObjectMapper objectMapper = new ObjectMapper();
    
    @PostMapping("/moneta")
    public ResponseEntity<String> handleWebhook(
            @RequestBody String payload,
            @RequestHeader(value = "X-Moneta-Signature", required = false) String signature) {
        
        // Verify the signature
        if (signature == null || !verifySignature(payload, signature)) {
            logger.warn("Invalid Moneta webhook signature");
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Unauthorized");
        }
        
        try {
            // Parse the JSON payload
            JsonNode event = objectMapper.readTree(payload);
            String eventType = event.path("event").asText();
            
            logger.info("Moneta webhook received: {}", eventType);
            
            // Handle different event types
            switch (eventType) {
                case "transaction.completed":
                    handleTransactionCompleted(event.path("data"));
                    break;
                case "transaction.failed":
                    handleTransactionFailed(event.path("data"));
                    break;
                case "webhook.test":
                    logger.info("Test webhook received successfully");
                    break;
                default:
                    logger.warn("Unhandled Moneta webhook event: {}", eventType);
            }
            
            return ResponseEntity.ok("OK");
            
        } catch (Exception e) {
            logger.error("Error processing Moneta webhook", e);
            return ResponseEntity.badRequest().body("Bad Request");
        }
    }
    
    private boolean verifySignature(String payload, String signature) {
        if (!signature.contains("=")) {
            return false;
        }
        
        String[] parts = signature.split("=", 2);
        String algorithm = parts[0];
        String receivedSignature = parts[1];
        
        try {
            String expectedSignature = generateSignature(payload, algorithm);
            return MessageDigest.isEqual(
                expectedSignature.getBytes(StandardCharsets.UTF_8),
                receivedSignature.getBytes(StandardCharsets.UTF_8)
            );
        } catch (Exception e) {
            logger.error("Error verifying signature", e);
            return false;
        }
    }
    
    private String generateSignature(String payload, String algorithm) 
            throws NoSuchAlgorithmException, InvalidKeyException {
        
        String hmacAlgorithm = "Hmac" + algorithm.toUpperCase();
        Mac mac = Mac.getInstance(hmacAlgorithm);
        SecretKeySpec secretKeySpec = new SecretKeySpec(
            CLIENT_MAC.getBytes(StandardCharsets.UTF_8), 
            hmacAlgorithm
        );
        mac.init(secretKeySpec);
        
        byte[] hash = mac.doFinal(payload.getBytes(StandardCharsets.UTF_8));
        return bytesToHex(hash);
    }
    
    private String bytesToHex(byte[] bytes) {
        StringBuilder result = new StringBuilder();
        for (byte b : bytes) {
            result.append(String.format("%02x", b));
        }
        return result.toString();
    }
    
    private void handleTransactionCompleted(JsonNode data) {
        String reference = data.path("reference").asText();
        int amount = data.path("amount").asInt();
        String currency = data.path("currency").asText();
        
        logger.info("Transaction completed: {} for {} {}", reference, amount, currency);
        // Update your database, send confirmation emails, etc.
    }
    
    private void handleTransactionFailed(JsonNode data) {
        String reference = data.path("reference").asText();
        logger.info("Transaction failed: {}", reference);
        // Handle failed transaction logic
    }
}
```
{% endtab %}
{% endtabs %}



***



#### Debugging Common Issues

**1. Signature Verification Failures**

* Ensure you're using the correct client secret
* Verify you're using the raw request body (not parsed JSON)
* Check that the algorithm matches what's configured
* Use timing-safe comparison functions

**2. Timeout Issues**

* Increase the webhook timeout in your settings
* Optimize your webhook handler for faster response times
* Consider processing webhooks asynchronously

**3. Missing Webhooks**

* Check your webhook logs for delivery attempts
* Verify your server is publicly accessible
* Ensure your endpoint returns 200 OK status

***

### Best Practices

#### Reliability

1. **Respond quickly** - Return 200 OK as soon as possible
2. **Process asynchronously** - Queue webhook processing for complex operations
3. **Handle duplicates** - Use idempotency keys to handle duplicate events
4. **Monitor webhook health** - Set up alerts for failed webhooks
5. **Implement circuit breakers** - Prevent cascading failures

{% hint style="info" %}


#### Security

1. **Always verify signatures** - Never process webhooks without signature verification
2. **Use HTTPS** - Always use HTTPS URLs for production webhooks
3. **Validate payload structure** - Check that required fields are present
4. **Rate limiting** - Implement rate limiting on your webhook endpoints
5. **Log security events** - Log failed signature verifications for monitoring
{% endhint %}



***

### Troubleshooting

#### Common Error Scenarios

{% tabs %}
{% tab title="Webhook Not Received" %}
**Possible Causes:**

* Webhook URL is incorrect or inaccessible
* Server is down or not responding
* Firewall blocking incoming requests
* SSL certificate issues

**Solutions:**

1. Test your endpoint with curl or Postman
2. Check server logs for incoming requests
3. Verify SSL certificate is valid
4. Use webhook testing tools
{% endtab %}

{% tab title="Signature Verification Failing" %}
**Possible Causes:**

* Using wrong client secret
* Incorrect signature algorithm
* Modifying request body before verification
* Character encoding issues

**Solutions:**

1. Double-check your client secret
2. Verify the signature algorithm matches your settings
3. Use raw request body for signature verification
4. Check for character encoding issues
{% endtab %}

{% tab title="Webhooks Timing Out" %}
**Possible Causes:**

* Webhook handler taking too long to respond
* Database queries are slow
* External API calls in webhook handler
* Server overloaded

**Solutions:**

1. Increase webhook timeout in settings
2. Optimize database queries
3. Process webhooks asynchronously
4. Scale your infrastructure
{% endtab %}

{% tab title="Duplicate Webhooks" %}
**Possible Causes:**

* Network issues causing retries
* Server returning non-200 status codes
* Processing taking longer than timeout

**Solutions:**

1. Implement idempotency using event IDs
2. Return 200 OK immediately after receiving
3. Use database constraints to prevent duplicates
4. Process webhooks in background jobs
{% endtab %}
{% endtabs %}



#### Getting Help

If you're still experiencing issues:

1. **Check the webhook logs** in your dashboard or via API
2. **Review the error messages** in the webhook log details
3. **Test with the webhook test endpoint** to isolate issues
4. **Contact Moneta support** with specific error details and webhook log UUIDs





***
