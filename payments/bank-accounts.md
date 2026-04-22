---
description: Sub accounts
---

# Bank Accounts

Bank accounts serves as payment collection points for which you can split incoming payments into. With bank accounts, payments made by your customers can be sent into the accounts you specify when you define transaction splits for the sub account.

## Create Sub Account

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](./#overview)`}}/subaccount`

\<Create a new sub account for funds payment splitting>

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |

**Body**

| Name             | Type   | Description         |
| ---------------- | ------ | ------------------- |
| `account_number` | string | Bank Account Number |
| `bank_id`        | number | Bank ID             |

Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
/**
 * Sends a POST request to create a subaccount.
 *
 * @param {string} baseUrl The base URL (e.g., 'https://api.mybank.com').
 * @param {string} serviceToken The value for the 'X-Service-Token' header.
 * @param {string} accountNumber The account number string.
 * @param {number} bankId The bank ID integer.
 * @returns {Promise<object | null>} The parsed JSON response object, or null on failure.
 */
async function createSubaccount(baseUrl, serviceToken, accountNumber, bankId) {
  const url = `${baseUrl}/subaccount`;
  
  const payload = {
    account_number: accountNumber,
    bank_id: bankId,
  };

  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        // Use SERVICE_TOKEN directly here
        'X-Service-Token': serviceToken, 
        'Content-Type': 'application/json'
      },
      // Serialize the JavaScript object (payload) into a JSON string for the body
      body: JSON.stringify(payload) 
    });

    // Check for HTTP errors (e.g., 400, 500 status codes)
    if (!response.ok) {
      const errorBody = await response.text(); // Get response body for detailed error logging
      console.error(`HTTP error! Status: ${response.status}`, `Body: ${errorBody}`);
      return null;
    }

    // Parse and return the JSON response body
    return await response.json();

  } catch (error) {
    // Handle network errors (e.g., connection issues, DNS failure)
    console.error('Network or Fetch Error:', error);
    return null;
  }
}

// --- Example Usage (How to call the async function) ---
(async () => {
    const BASE_URL = 'https://api.mybank.com';
    const SERVICE_TOKEN = 'your_secure_sub_account_token';
    const ACCOUNT_NUM = '0123456789';
    const BANK_ID = 44;
    
    // const result = await createSubaccount(BASE_URL, SERVICE_TOKEN, ACCOUNT_NUM, BANK_ID);

    // if (result) {
    //   console.log('Subaccount Created Successfully:', result);
    // } else {
    //   console.log('Subaccount creation failed.');
    // }
})();
```
{% endtab %}

{% tab title="Php" %}
```php
<?php

/**
 * Sends a POST request to create a subaccount.
 *
 * @param string $baseUrl The base URL (e.g., 'https://api.example.com').
 * @param string $serviceToken The value for the 'X-Service-Token' header.
 * @param string $accountNumber The account number string.
 * @param int $bankId The bank ID integer.
 * @return array|null The decoded JSON response array, or null on failure.
 */
function createSubaccountCurl(string $baseUrl, string $serviceToken, string $accountNumber, int $bankId): ?array
{
    $url = $baseUrl . '/subaccount';
    
    $payload = [
        "account_number" => $accountNumber,
        "bank_id" => $bankId,
    ];

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

    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    
    if (curl_errno($ch)) {
        error_log("cURL Error: " . curl_error($ch));
        curl_close($ch);
        return null;
    }
    
    curl_close($ch);

    if ($httpCode >= 400) {
        error_log("API Error (HTTP $httpCode): " . $response);
        return null;
    }

    return json_decode($response, true);
}

// --- Example Usage ---
// $BASE_URL = 'https://api.mybank.com';
// $SERVICE_TOKEN = 'your_secure_sub_account_token';
// $ACCOUNT_NUM = '0123456789';
// $BANK_ID = 44; 

// $result = createSubaccountCurl($BASE_URL, $SERVICE_TOKEN, $ACCOUNT_NUM, $BANK_ID);
// print_r($result);
?>
```
{% endtab %}

{% tab title="Laravel" %}
```php
<?php
namespace App\Services;

use Illuminate\Support\Facades\Http;

class SubaccountService
{
    /**
     * Creates a new subaccount by calling an external API.
     *
     * @param string $baseUrl The base API endpoint URL.
     * @param string $serviceToken The value for the 'X-Service-Token' header.
     * @param string $accountNumber The account number string.
     * @param int $bankId The bank ID integer.
     * @return array|null The decoded JSON response array, or null on failure.
     */
    public function createSubaccount(string $baseUrl, string $serviceToken, string $accountNumber, int $bankId): ?array
    {
        $url = $baseUrl . '/subaccount';
        
        $payload = [
            'account_number' => $accountNumber,
            'bank_id' => $bankId,
        ];

        try {
            $response = Http::withHeaders([
                'X-Service-Token' => $serviceToken,
            ])
            // Laravel automatically sets Content-Type: application/json and serializes the array
            ->post($url, $payload);
            
            // Check for client or server errors (4xx or 5xx)
            $response->throw();

            // Return the decoded JSON response
            return $response->json();

        } catch (\Exception $e) {
            // Log the error
            logger()->error('Subaccount API Call Failed: ' . $e->getMessage(), ['url' => $url]);
            return null;
        }
    }
}
// --- Example Usage (in a Controller or another service) ---
/*
// Assuming dependency injection is used for SubaccountService
// $result = $subaccountService->createSubaccount($BASE_URL, $SERVICE_TOKEN, '0123456789', 44);
*/
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
from typing import Dict, Any, Optional

def create_subaccount(base_url: str, service_token: str, account_number: str, bank_id: int) -> Optional[Dict[str, Any]]:
    """
    Sends a POST request to create a subaccount.

    :param base_url: The base URL of the API.
    :param service_token: The X-Service-Token value.
    :param account_number: The account number string.
    :param bank_id: The bank ID integer.
    :return: The decoded JSON response dictionary, or None on failure.
    """
    url = f"{base_url}/subaccount"
    
    payload = {
        "account_number": account_number,
        "bank_id": bank_id,
    }

    headers = {
        'X-Service-Token': service_token,
        'Content-Type': 'application/json'
    }

    try:
        # requests uses the 'json' argument to handle serialization and Content-Type header
        response = requests.post(url, headers=headers, json=payload, timeout=10)
        
        # Raise an exception for HTTP error status codes (4xx or 5xx)
        response.raise_for_status()

        # Return the decoded JSON response
        return response.json()

    except requests.exceptions.RequestException as e:
        print(f"Error creating subaccount at {url}: {e}")
        return None

# --- Example Usage ---
# BASE_URL = 'https://api.mybank.com'
# SERVICE_TOKEN = 'your_secure_sub_account_token'
# ACCOUNT_NUM = '0123456789'
# BANK_ID = 44

# result = create_subaccount(BASE_URL, SERVICE_TOKEN, ACCOUNT_NUM, BANK_ID)
# if result:
#     print(result)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.IOException;

public class SubaccountClient {

    private static final HttpClient client = HttpClient.newHttpClient();

    /**
     * Sends a synchronous POST request to create a subaccount.
     *
     * @param baseUrl The base API endpoint URL.
     * @param serviceToken The value for the 'X-Service-Token' header.
     * @param accountNumber The account number string.
     * @param bankId The bank ID integer.
     * @return The raw JSON response body as a String.
     * @throws IOException If an I/O error occurs during the request.
     * @throws InterruptedException If the operation is interrupted.
     * @throws RuntimeException If the HTTP status code is not successful (non-2xx).
     */
    public static String createSubaccount(String baseUrl, String serviceToken, String accountNumber, int bankId) 
            throws IOException, InterruptedException {
        
        String url = baseUrl + "/subaccount";
        
        // Constructing the JSON payload string
        String jsonPayload = String.format("""
            {
                "account_number": "%s",
                "bank_id": %d
            }
            """, accountNumber, bankId);

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
            throw new RuntimeException("Subaccount Creation Failed. Status Code: " + statusCode + 
                                       ". Body: " + response.body());
        }

        return response.body();
    }
}

// --- Example Usage ---
/*
public class Main {
    public static void main(String[] args) {
        String BASE_URL = "https://api.mybank.com";
        String SERVICE_TOKEN = "your_secure_sub_account_token";
        String ACCOUNT_NUM = "0123456789";
        int BANK_ID = 44;

        try {
            String resultJson = SubaccountClient.createSubaccount(BASE_URL, SERVICE_TOKEN, ACCOUNT_NUM, BANK_ID);
            System.out.println("Successful Subaccount Response:\n" + resultJson);
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
    "status": "success",
    "data": {
        "user_id": 1,
        "business_name": "Test Account(0792382349)",
        "account_code": "0792382349",
        "settlement_bank": "090146",
        "account_number": "0792382349",
        "gateway": "bank",
        "aggregator": 1,
        "updated_at": "2025-12-12T10:38:22.000000Z",
        "created_at": "2025-12-12T10:38:22.000000Z",
        "id": 1
    },
    "message": "Subaccount added successfully"
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

***

## Get Sub Accounts

<mark style="color:green;">`GET`</mark> `{{baseURL}}/subaccounts`

\<Get  Sub account Information>

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |



**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "status": "success",
    "data": [
        {
            "id": 1,
            "user_id": 1,
            "aggregator": 1,
            "account_code": "0792382349",
            "business_name": "Test Account(0792382349)",
            "settlement_bank": "090146",
            "account_number": "0792382349",
            "bank_code": null,
            "gateway": "bank",
            "created_at": "2025-12-12T10:38:22.000000Z",
            "updated_at": "2025-12-12T10:38:22.000000Z"
        },
        {
            "id": 2,
            "user_id": 1,
            "aggregator": 1,
            "account_code": "0592367834",
            "business_name": "Test Account(0592367834)",
            "settlement_bank": "090146",
            "account_number": "0592367834",
            "bank_code": null,
            "gateway": "bank",
            "created_at": "2025-12-12T10:51:08.000000Z",
            "updated_at": "2025-12-12T10:51:08.000000Z"
        }
    ],
    "message": "Subaccounts fetched successfully"
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
