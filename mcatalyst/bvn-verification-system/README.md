# BVN Verification System

This endpoints allows you verify user Bank Verificaiton Number (BVN) information for identify confirmation. Before going forward, create an onboard request to the onboard endpoint to verify user account before going forward.

{% hint style="warning" %}
Use this endpoint before proceeding with a full BVN verification to save on API costs.
{% endhint %}

## BVN setup

<mark style="color:green;">`POST`</mark> \{{baseUrl\}}`/bvn/bvn_onboard`

\<onboard a customer's BVN>

**Headers**

| Name            | Value                                                            |
| --------------- | ---------------------------------------------------------------- |
| Content-Type    | `application/json`                                               |
| X-Service-Token |  `<`[`service_token`](../../payments/#payment-initialization)`>` |

**Body**

Please note: every field is required&#x20;

| Name                | Type   | Description                                                     |
| ------------------- | ------ | --------------------------------------------------------------- |
| `scope`             | string | The scope of this request e.g accounts                          |
| `channel_code`      | string | The code for the channel through which the request is generated |
| customer\_reference | string | The reference for the customer                                  |
| bvn                 | string | The BVN of the customer                                         |

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Configuration Variables 
const BVN_URL=""; // Replace {{BVN_URL}}
const SERVICE_TOKEN=""; // Replace ${SERVICE_TOKEN}



const validateBVN = async (bvn) => {
  // Request Payload
  const url = `{{BVN_URL}}`;
  const payload = {
  scope: "accounts",
  channel_code: "customer_web_portal",
  customer_reference: "daniska222",
  bvn: "22222222280",
};

// Initialize Fetch Action
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'X-Service-Token': `${SERVICE_TOKEN}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(payload)
  });
// return response
  return await response.json();
};

```
{% endtab %}

{% tab title="Php" %}
```php
function fetchBvnDataCurl(string $url, string $serviceToken, array $payload): ?array
{
    $jsonPayload = json_encode($payload);

    $ch = curl_init($url);
    
    // Set cURL options
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'POST');
    curl_setopt($ch, CURLOPT_POSTFIELDS, $jsonPayload);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); // Return response as a string
    curl_setopt($ch, CURLOPT_TIMEOUT, 10); // 10 second timeout

    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        'X-Service-Token: ' . $serviceToken,
        'Content-Type: application/json',
        'Content-Length: ' . strlen($jsonPayload)
    ]);

    // Execute the cURL request
    $response = curl_exec($ch);
    
    if (curl_errno($ch)) {
        // Log or handle the error
        error_log("cURL Error: " . curl_error($ch));
        curl_close($ch);
        return null;
    }
    
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($httpCode >= 400) {
        error_log("API Error (HTTP $httpCode): " . $response);
        return null;
    }

    // Decode the JSON response and return it as an array
    return json_decode($response, true);
}

// --- Example Usage ---
$BVN_URL = 'https://api.example.com/bvn';
$SERVICE_TOKEN = 'your_secure_token_123';
$PAYLOAD = [
    "scope" => "accounts",
    "channel_code" => "customer_web_portal",
    "customer_reference" => "daniska222",
    "bvn" => "22222222280",
];

// $result = fetchBvnDataCurl($BVN_URL, $SERVICE_TOKEN, $PAYLOAD);
// print_r($result);
?> 
```
{% endtab %}

{% tab title="Laravel" %}
```php
<?php
namespace App\Services;

use Illuminate\Support\Facades\Http;
use Illuminate\Http\Client\Response;

class BvnService
{
    /**
     * Verifies BVN data by calling an external API.
     *
     * @param string $bvnUrl The API endpoint URL.
     * @param string $serviceToken The value for the 'X-Service-Token' header.
     * @param array $payload The data to be sent in the request body.
     * @return array|null The decoded JSON response array, or null on failure.
     */
    public function verifyBvn(string $bvnUrl, string $serviceToken, array $payload): ?array
    {
        try {
            // Http::withHeaders is used for non-standard headers
            $response = Http::withHeaders([
                'X-Service-Token' => $serviceToken,
                // 'Content-Type: application/json' is set automatically by Http::post($url, $payload)
            ])->post($bvnUrl, $payload);
            
            // Throw an exception for 4xx or 5xx responses
            $response->throw();

            // Return the decoded JSON response as an associative array
            return $response->json();

        } catch (\Exception $e) {
            // Log the error for debugging
            logger()->error('BVN API Call Failed: ' . $e->getMessage(), [
                'url' => $bvnUrl,
                'payload' => $payload
            ]);
            return null;
        }
    }
}

// --- Example Usage (e.g., in a Controller) ---
/*
class BvnController extends Controller 
{
    public function __construct(protected BvnService $bvnService) {}

    public function processBvn() 
    {
        $bvnUrl = config('services.bvn.url');
        $serviceToken = config('services.bvn.token');
        $payload = [
            "scope" => "accounts",
            "channel_code" => "customer_web_portal",
            "customer_reference" => "daniska222",
            "bvn" => "22222222280",
        ];

        $result = $this->bvnService->verifyBvn($bvnUrl, $serviceToken, $payload);

        if ($result) {
            return response()->json(['status' => 'success', 'data' => $result]);
        }
        
        return response()->json(['status' => 'error', 'message' => 'BVN verification failed'], 500);
    }
}
*/
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json
from typing import Dict, Any, Optional

def fetch_bvn_data(url: str, service_token: str, payload: Dict[str, Any]) -> Optional[Dict[str, Any]]:
    """
    Sends a POST request to an API endpoint with a service token header.

    :param url: The API endpoint URL.
    :param service_token: The X-Service-Token value.
    :param payload: The data to be sent as JSON in the request body.
    :return: The decoded JSON response dictionary, or None on failure.
    """
    headers = {
        'X-Service-Token': service_token,
        'Content-Type': 'application/json'
    }

    try:
        # requests.post uses the 'json' argument to automatically handle 
        # JSON serialization and setting the Content-Type header.
        response = requests.post(url, headers=headers, json=payload, timeout=10)
        
        # Raise an exception for HTTP error status codes (4xx or 5xx)
        response.raise_for_status()

        # Return the decoded JSON response
        return response.json()

    except requests.exceptions.RequestException as e:
        # Log or handle the exception
        print(f"An error occurred during API call to {url}: {e}")
        if response is not None:
             print(f"Response status: {response.status_code}")
             
        return None

# --- Example Usage ---
BVN_URL = 'https://api.example.com/bvn'
SERVICE_TOKEN = 'your_secure_token_123'
PAYLOAD = {
    "scope": "accounts",
    "channel_code": "customer_web_portal",
    "customer_reference": "daniska222",
    "bvn": "22222222280",
}

# result = fetch_bvn_data(BVN_URL, SERVICE_TOKEN, PAYLOAD)
# if result:
#     print(json.dumps(result, indent=4))
```
{% endtab %}

{% tab title="Java" %}
```ruby
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.IOException;


public class ApiClient {

    private static final HttpClient client = HttpClient.newHttpClient();

    /**
     * Sends a synchronous POST request to an external API.
     * * @param url The API endpoint URL.
     * @param serviceToken The value for the 'X-Service-Token' header.
     * @param jsonPayload A String containing the request body in JSON format.
     * @return The raw JSON response body as a String.
     * @throws IOException If an I/O error occurs during the request.
     * @throws InterruptedException If the operation is interrupted.
     * @throws RuntimeException If the HTTP status code is not successful (2xx).
     */
    public static String postJson(String url, String serviceToken, String jsonPayload) 
            throws IOException, InterruptedException {

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("X-Service-Token", serviceToken)
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(jsonPayload))
                .build();

        // Send the request synchronously
        HttpResponse<String> response = client.send(
            request, 
            HttpResponse.BodyHandlers.ofString()
        );

        int statusCode = response.statusCode();
        if (statusCode < 200 || statusCode >= 300) {
            // Throw an exception for non-successful status codes
            throw new RuntimeException("HTTP Request Failed. Status Code: " + statusCode + 
                                       ". Body: " + response.body());
        }

        return response.body();
    }
}

// --- Example Usage ---
/*
public class Main {
    public static void main(String[] args) {
        String BVN_URL = "https://api.example.com/bvn";
        String SERVICE_TOKEN = "your_secure_token_123";
        // In a real application, you'd serialize a Java object to this string
        String PAYLOAD_JSON = """
            {
                "scope": "accounts",
                "channel_code": "customer_web_portal",
                "customer_reference": "daniska222",
                "bvn": "22222222280"
            }
            """;

        try {
            String resultJson = ApiClient.postJson(BVN_URL, SERVICE_TOKEN, PAYLOAD_JSON);
            System.out.println("Successful Response:\n" + resultJson);
        } catch (Exception e) {
            System.err.println("API Call Error: " + e.getMessage());
        }
    }
}
*/
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "status": true,
  "message": "Request successful",
  "data": "Response data goes here",
  "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}



