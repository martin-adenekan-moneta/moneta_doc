---
description: Recieve Payment using Card
---

# Accept Payment

Receive payments from clients & customers easily by following the steps and sample codes provided below as means to complete your payment link generation.

#### Hash Key Generation

Once you have successfully generated your [service token](./), generate a hash for your transaction by providing the following data:

<table><thead><tr><th width="180">Fields</th><th width="113">Required</th><th>Description</th></tr></thead><tbody><tr><td>Email</td><td>True</td><td>Your customer's email</td></tr><tr><td>Amount</td><td>True</td><td>Amount to be paid <strong>(in kobo)</strong></td></tr><tr><td>Payment Type</td><td>True</td><td>The type of payment to be initialized (card or bank)</td></tr><tr><td>channel</td><td>string</td><td>card, bank_transfer (when you select bank payment type) and ussd</td></tr><tr><td>Callback URL</td><td>True</td><td>The URL that your customer will be redirected to after payment</td></tr><tr><td>Mac</td><td>True</td><td>Your Mac</td></tr></tbody></table>

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const crypto = require('crypto');

// Variables
const email = "test@example.com";
const amount = 10000;
const payment_type = "card";
const callback_url = "http://localhost:8000/callback";
const mac =  "";

function generateHash(email, amount, payment_type, callback_url, mac ) {
    // Concatenate the values into a single string
    // Ensure amount is converted to a string before concatenation
    const mystring = email + String(amount) + payment_type + callback_url;

    // Create HMAC-SHA512
    const hmac = crypto.createHmac('sha512', mac );
    
    // Update the HMAC with the data and get the hexadecimal digest
    // The 'hex' encoding is crucial for matching the PHP output
    const hashedValue = hmac.update(mystring).digest('hex');
    return hashedValue;
}

// Generate and print the hash
const hashResult = generateHash(email, amount, payment_type, callback_url, mac);
console.log(hashResult);
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
$email = "test@example.com";
$amount =10000 ;
$payment_type = "card";
$callback_url = "http://localhost:8000/callback";
$mac = "";

function generate_hash($email,$amount,$payment_type,$callback_url,$mac){
     // Concatenate the values into a single string
    $mystring = $email . $amount. $payment_type . $callback_url;
    
    // Create HMAC-SHA512
    return hash_hmac('sha512', $mystring, $mac, false);
}


// Generate and print the hash
$hash = generate_hash($email,$amount,$payment_type,$callback_url,$mac);
echo $hash;
 
```
{% endtab %}

{% tab title="Python" %}
```python
import hmac
import hashlib

# Variables
email = "test@example.com"
amount = 10000
payment_type = "card"
callback_url = "http://localhost:8000/callback"
mac= ""

def generate_hash(email, amount, payment_type, callback_url, mac):
    # Concatenate the values into a single string
    # Ensure amount is converted to a string before concatenation
    mystring = email + str(amount) + payment_type + callback_url
    
    # Convert string data and key to bytes (using UTF-8 encoding)
    key_bytes = mac.encode('utf-8')
    data_bytes = mystring.encode('utf-8')
    
    # Generate HMAC-SHA512 hash
    # .hexdigest() converts the hash bytes to a hexadecimal string
    hashed_value = hmac.new(key_bytes, data_bytes, hashlib.sha512).hexdigest()
    return hashed_value

# Generate and print the hash
hash_result = generate_hash(email, amount, payment_type, callback_url, mac)
print(hash_result)
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.nio.charset.StandardCharsets;
import java.util.Formatter;

public class HashGenerator {

    // Variables
    private static final String EMAIL = "test@example.com";
    private static final int AMOUNT = 10000;
    private static final String PAYMENT_TYPE = "card";
    private static final String CALLBACK_URL = "http://localhost:8000/callback";
    private static final String MAC= "";

    public static String generateHash(String email, int amount, String payment_type, String callback_url, String mac) {
        // Concatenate the values into a single string
        String mystring = email + amount + payment_type + callback_url;
        
        try {
            // Algorithm specification
            String algorithm = "HmacSHA512";
            
            // Convert MAC key string to a SecretKeySpec
            SecretKeySpec secretKeySpec = new SecretKeySpec(mac.getBytes(StandardCharsets.UTF_8), algorithm);
            
            // Initialize the MAC object
            Mac hmacSha512 = Mac.getInstance(algorithm);
            hmacSha512.init(secretKeySpec);
            
            // Compute the HMAC digest
            byte[] hashBytes = hmacSha512.doFinal(mystring.getBytes(StandardCharsets.UTF_8));
            
            // Convert byte array to a hexadecimal string (matching PHP's output)
            return bytesToHex(hashBytes);

        } catch (NoSuchAlgorithmException | InvalidKeyException e) {
            e.printStackTrace();
            return null; // Handle error appropriately
        }
    }

    // Helper method to convert byte array to a hexadecimal string
    private static String bytesToHex(byte[] bytes) {
        Formatter formatter = new Formatter();
        for (byte b : bytes) {
            formatter.format("%02x", b);
        }
        return formatter.toString();
    }

    public static void main(String[] args) {
        String hashResult = generateHash(EMAIL, AMOUNT, PAYMENT_TYPE, CALLBACK_URL, MAC);
        System.out.println(hashResult);
    }
}
```
{% endtab %}

{% tab title="C# (.Net)" %}
```csharp
using System;
using System.Security.Cryptography;
using System.Text;

public class HashGenerator
{
    // Variables
    private const string Email = "test@example.com";
    private const int Amount = 10000;
    private const string PaymentType = "card";
    private const string CallbackUrl = "http://localhost:8000/callback";
    private const string Mac= "";

    public static string GenerateHash(string email, int amount, string paymentType, string callbackUrl, string mac)
    {
        // Concatenate the values into a single string
        // Ensure amount is converted to a string before concatenation
        string myString = email + amount.ToString() + paymentType + callbackUrl;
        
        // Convert the MAC key and the string data to byte arrays using UTF-8 encoding
        byte[] keyBytes = Encoding.UTF8.GetBytes(mac);
        byte[] dataBytes = Encoding.UTF8.GetBytes(myString);

        // Create an HMACSHA512 object with the key
        using (var hmac = new HMACSHA512(keyBytes))
        {
            // Compute the hash
            byte[] hashBytes = hmac.ComputeHash(dataBytes);
            
            // Convert the byte array to a hexadecimal string
            return BitConverter.ToString(hashBytes).Replace("-", "").ToLowerInvariant();
        }
    }

    public static void Main()
    {
        string hashResult = GenerateHash(Email, Amount, PaymentType, CallbackUrl, Mac);
        Console.WriteLine(hashResult);
    }
}
```
{% endtab %}
{% endtabs %}

Once you have successfully created an hash for your transaction, proceed to creating a POST request to the payment endpoint.

## Initialize Payment

Endpoint URL

<mark style="color:green;">`POST`</mark>  `{{`[`baseUrl`](./)`}}/transaction/initialize`

\<Generate payment link for receiving payment from client using card, bank transfer or ussd>

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |

**Body**

<table><thead><tr><th width="140">Name</th><th width="134">Type</th><th width="110">Required</th><th width="180">Description</th></tr></thead><tbody><tr><td>txnref</td><td>string</td><td>no</td><td>Transaction Reference  Generated from your system or from previous request.</td></tr><tr><td>amount</td><td>string|number</td><td>Yes</td><td>Amount to be paid (in Kobo)</td></tr><tr><td>email</td><td>string</td><td>Yes</td><td>Customer Email</td></tr><tr><td>payment_type</td><td>string</td><td>Yes</td><td>Payment Type (card,ussd,bank-transfer)</td></tr><tr><td>channel</td><td>string</td><td>No</td><td>Payment Channel (default: card)</td></tr><tr><td>hash</td><td>string</td><td>Yes</td><td>Your <a href="accept-payment.md#hash-key-generation">Generated Hash</a> key </td></tr><tr><td>callback_url</td><td>string</td><td>Yes</td><td>Which page on your app should users be redirected to on successful payment</td></tr><tr><td>use_split</td><td>string</td><td>No</td><td>For split transactions, provide your split key from your <a href="https://merchant.moneta.ng/account/profile-settings/bank-accounts">split settings page</a></td></tr><tr><td>json</td><td>string|bool</td><td>No</td><td>Return Json response</td></tr><tr><td>metadata</td><td>array|object</td><td>No</td><td>Meta data that you want to be sent back with the response</td></tr><tr><td>customerinfo</td><td>json|object</td><td>No</td><td>customer information is required when using bank payment type  e.g { "first_name": "Test", "last_name": "Example" },  </td></tr><tr><td>transaction_fee_bearer</td><td>string</td><td>No</td><td>Who will bear the charges? (customer/merchant)</td></tr></tbody></table>

Sample Codes

{% tabs %}
{% tab title="Curl" %}
```bash
curl --location 'ENDPOINT_URL' \
--header 'Content-Type: application/json' \
--header 'X-Service-Token: YOUR_SERVICE_TOKEN' \
--data '{
    "amount": 10000,
    "email": "holuwoleh@gmail.com",
    "payment_type": "card",
    "channel": "card",
    "hash": "GENERATED_HASH",
    "callback_url": "https://mail.google.com/mail/u/1/#inbox",
    "use_split": "691e80a66f639",
    "json": "true"
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// --- Configuration ---
const ENDPOINT_URL = "[ENDPOINT_URL]";

// Replace these placeholders with your actual values
const X_SERVICE_TOKEN = "[YOUR_X_SERVICE_TOKEN]"; 
const HMAC_HASH = "[YOUR_HMAC_SHA512_HASH]";

// --- Request Body ---
const transactionData = {
    "amount": 10000,
    "email": "test@example.com",
    "payment_type": "card",
    "channel": "card",
    // Insert the calculated hash here
    "hash": HMAC_HASH,
    "callback_url": "https://example.com/callback",
    "use_split": "",
    "json": "true"
};

// --- Function to Send Request ---
async function initializeTransaction() {
    try {
        const response = await fetch(ENDPOINT_URL, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-Service-Token': X_SERVICE_TOKEN // Custom Header
            },
            body: JSON.stringify(transactionData)
        });

        // Check for HTTP errors (4xx or 5xx)
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        console.log("Transaction Initialization Success:");
        console.log(data);
        return data;
        
    } catch (error) {
        console.error("Error initializing transaction:", error);
    }
}

// Execute the function
initializeTransaction();
```
{% endtab %}

{% tab title="Php (Curl)" %}
```php
<?php
// --- Configuration ---
$endpoint_url = "[ENDPOINT_URL]";

// Replace these placeholders with your actual values
$x_service_token = "[YOUR_X_SERVICE_TOKEN]"; 
$hmac_hash = "[YOUR_HMAC_SHA512_HASH]";

// --- Request Body ---
$transaction_data = [
    "amount" => 10000,
    "email" => "test@example.com",
    "payment_type" => "card",
    "channel" => "card",
    // Insert the calculated hash here
    "hash" => $hmac_hash,
    "callback_url" => "https://example.com/callback",
    "use_split" => "", 
    "json" => "true"
];

$json_payload = json_encode($transaction_data);

// --- cURL Setup ---
$ch = curl_init($endpoint_url);

// Set cURL options
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); // Return the response as a string
curl_setopt($ch, CURLOPT_POST, true);           // Use POST method
curl_setopt($ch, CURLOPT_POSTFIELDS, $json_payload); // Set JSON payload

// Set headers
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
    'Content-Length: ' . strlen($json_payload),
    "X-Service-Token: $x_service_token" // Custom Header
]);

// Execute the request
$response = curl_exec($ch);

// Check for errors
if (curl_errno($ch)) {
    $error_msg = curl_error($ch);
    echo "cURL Error: " . $error_msg;
} else {
    // Decode and display the API response
    $http_status = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    echo "HTTP Status: " . $http_status . "\n";
    echo "API Response:\n";
    print_r(json_decode($response, true));
}

// Close cURL handle
curl_close($ch);

?> 


```
{% endtab %}

{% tab title="Php (Guzzle)" %}
```php
<?php
// This line is essential if you are using Composer to manage dependencies
// You need to include the Composer autoloader to use GuzzleHttp
require 'vendor/autoload.php'; 

use GuzzleHttp\Client;
use GuzzleHttp\Exception\RequestException;

// --- Configuration ---
$endpoint_url = "[ENDPOINT_URL]";

// !! IMPORTANT: Replace these placeholders with your actual values !!
$x_service_token = "[YOUR_X_SERVICE_TOKEN]"; 
$hmac_hash = "[YOUR_HMAC_SHA512_HASH]";

// --- Request Body (PHP Array) ---
// Guzzle will automatically convert this array to JSON and set the Content-Type header
$transaction_data = [
    "amount" => 10000,
    "email" => "test@example.com",
    "payment_type" => "card",
    "channel" => "card",
    // Insert the calculated hash here
    "hash" => $hmac_hash,
    "callback_url" => "https://example.com/callback",
    "use_split" => "",
    "gateway_scheme_used" => 6,
    "json" => "true"
];

// --- Guzzle Client Setup and Execution ---
try {
    // 1. Initialize the Guzzle Client
    $client = new Client();
    
    // 2. Send the POST request
    $response = $client->post($endpoint_url, [
        // Headers option for custom headers
        'headers' => [
            'X-Service-Token' => $x_service_token, // Custom Header
        ],
        // 'json' option automatically sets Content-Type: application/json 
        // and converts the PHP array to a JSON string payload.
        'json' => $transaction_data,
        'timeout' => 10, // Set a timeout
    ]);

    // 3. Process the successful response (HTTP 2xx)
    $status_code = $response->getStatusCode();
    $response_body = $response->getBody()->getContents();

    echo "Transaction Initialization Success!\n";
    echo "HTTP Status Code: " . $status_code . "\n";
    echo "API Response:\n";
    print_r(json_decode($response_body, true));

} catch (RequestException $e) {
    // 4. Handle exceptions (network errors, timeouts, and non-200 status codes)
    
    echo "Guzzle Request Error:\n";
    
    if ($e->hasResponse()) {
        $response = $e->getResponse();
        echo "Status Code: " . $response->getStatusCode() . "\n";
        echo "Error Body: " . $response->getBody()->getContents() . "\n";
    } else {
        echo "Network or connection error: " . $e->getMessage() . "\n";
    }
} catch (\Exception $e) {
    echo "An unexpected error occurred: " . $e->getMessage() . "\n";
}

?>
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

# --- Configuration ---
ENDPOINT_URL = "[ENDPOINT_URL]"

# Replace these placeholders with your actual values
X_SERVICE_TOKEN = "[YOUR_X_SERVICE_TOKEN]"
HMAC_HASH = "[YOUR_HMAC_SHA512_HASH]"

# --- Request Body ---
transaction_data = {
    "amount": 10000,
    "email": "test@example.com",
    "payment_type": "card",
    "channel": "card",
    # Insert the calculated hash here
    "hash": HMAC_HASH,
    "callback_url": "https://example.com/callback",
    "use_split": "", 
    "json": "true"
}

# --- Headers ---
headers = {
    'Content-Type': 'application/json',
    'X-Service-Token': X_SERVICE_TOKEN  # Custom Header
}

# --- Function to Send Request ---
try:
    print(f"Sending request to: {ENDPOINT_URL}")
    response = requests.post(ENDPOINT_URL, headers=headers, json=transaction_data)
    
    # Raise an exception for bad status codes (4xx or 5xx)
    response.raise_for_status() 
    
    # Parse the JSON response
    response_data = response.json()
    
    print("Transaction Initialization Success:")
    print(json.dumps(response_data, indent=4))
    
except requests.exceptions.HTTPError as errh:
    print(f"HTTP Error: {errh}")
    if response.content:
        print("Response content:", response.text)
except requests.exceptions.ConnectionError as errc:
    print(f"Connection Error: {errc}")
except requests.exceptions.Timeout as errt:
    print(f"Timeout Error: {errt}")
except requests.exceptions.RequestException as err:
    print(f"An unexpected error occurred: {err}")
```
{% endtab %}

{% tab title="C# (.Net)" %}
```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

public class TransactionInitializer
{
    // --- Configuration ---
    private const string EndpointUrl = "[ENDPOINT_URL]";

    // Replace these placeholders with your actual values
    private const string XServiceToken = "[YOUR_X_SERVICE_TOKEN]";
    private const string HmacHash = "[YOUR_HMAC_SHA512_HASH]";

    public static async Task Main()
    {
        // 1. Prepare the JSON payload
        string jsonBody = $@"{{
            ""amount"": 10000,
            ""email"": ""test@example.com"",
            ""payment_type"": ""card"",
            ""channel"": ""card"",
            ""hash"": ""{HmacHash}"",
            ""callback_url"": ""https://example.com/callback"",
            ""use_split"": """",
            ""json"": ""true""
        }}";

        // 2. Create the HttpClient
        using (var client = new HttpClient())
        {
            // 3. Set the Custom Header (X-Service-Token)
            client.DefaultRequestHeaders.Add("X-Service-Token", XServiceToken);

            // 4. Create the request content
            var content = new StringContent(jsonBody, Encoding.UTF8, "application/json");

            try
            {
                Console.WriteLine($"Sending request to: {EndpointUrl}");
                
                // 5. Send the POST request
                HttpResponseMessage response = await client.PostAsync(EndpointUrl, content);

                // 6. Check for success status code
                if (response.IsSuccessStatusCode)
                {
                    string responseBody = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Transaction Initialization Success (HTTP 2xx)");
                    Console.WriteLine("Response Body:\n" + responseBody);
                }
                else
                {
                    string errorBody = await response.Content.ReadAsStringAsync();
                    Console.WriteLine($"Request failed with status code: {response.StatusCode}");
                    Console.WriteLine("Error Body:\n" + errorBody);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "status": "success",
    "responseCode": "00",
    "url": "http://localhost:8000/callback",
    "ref_no": "MNTAD9SL35DG7GEFK0",
    "type": "card",
    "authorization_url": "/api/v2/txn/isw/MNTAD9SL385GVC7FK0"
}
```
{% endtab %}

{% tab title="400" %}
```json
```
{% endtab %}
{% endtabs %}

#### Testing Cards

The following test cards have been provided for testing your checkout process in the sandbox environment. Use the provided pin and otp when making payment for any of the cards provided.

PIN: 1234

OTP: 123456

<table data-column-title-hidden data-view="cards"><thead><tr><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><em>Master</em></td><td><h4><strong>5300 0000 0000 0001</strong></h4></td><td>Expiry:  01/99</td><td>CVV:  111</td></tr><tr><td><em>Visa</em></td><td><h4><strong>4000 0000 0000 0002</strong></h4></td><td>Expiry:  02/99</td><td>CVV:  222</td></tr><tr><td> <em>Verve</em></td><td><h4><strong>5061 0200 0000 0000 0004</strong></h4></td><td>Expiry:  03/99</td><td>CVV:  333</td></tr></tbody></table>
