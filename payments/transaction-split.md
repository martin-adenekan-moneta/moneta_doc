# Transaction Split

Transaction splitting enables incoming funds to be separated into sub accounts you have created using any share logic you decide. You have the liberty to share to as many sub accounts as you wish but be sure your splitting rule is well defined.

## Create Transaction split

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/split`

\<Create splitting rules for how incoming payments should be splitted >

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |

**Body**

| Name            | Type             | Description                                                                                                                             |
| --------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| alias           | string           | An identifier for this split                                                                                                            |
| `subaccounts`   | array(number)    | An array of account sub accounts that you want to split funds into                                                                      |
| `perc_flat`     | string           | Sharing type: percentage or flat                                                                                                        |
| `shares`        | array (number)   | For percentage sharing the sum of this shares must be equal to 100                                                                      |
| `base_amount`   | number \| string | for flat sharing, the sum of your shares must be equal to this base amount                                                              |
| `businessnames` | array (string)   | This is an array of the **Account Name** for each account number provided in the order it is arranged in the `subaccounts` field above. |
| bearer          | 0 or 1           | <p>0 - if you want to bear the VAT rate of these transactions or <br>1 - if you want to charge the accounts</p>                         |

Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
/**
 * Creates a new split payment configuration.
 *
 * @param {string} baseUrl The base URL (e.g., 'https://api.example.com').
 * @param {string} serviceToken The value for the 'X-Service-Token' header.
 * @returns {Promise<object | null>} The parsed JSON response object, or null on failure.
 */
async function createSplitConfig(baseUrl, serviceToken) {
  const url = `${baseUrl}/split`;
  
  const payload = {
    "subaccounts": ["0429425494", "0792382349"],
    "shares": [50, 50],
    "businessNames": ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
    "bearer": 0,
    "perc_flat": "percentage",
    "alias": "New Split",
    "base_amount": 100
  };

  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'X-Service-Token': serviceToken, 
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(payload)
    });

    if (!response.ok) {
      const errorBody = await response.text();
      console.error(`HTTP error! Status: ${response.status}`, `Body: ${errorBody}`);
      return null;
    }

    return await response.json();

  } catch (error) {
    console.error('Network or Fetch Error:', error);
    return null;
  }
}

// Example Call:
// const result = await createSplitConfig('https://api.paymentgate.com', 'YOUR_SERVICE_TOKEN');
// console.log(result);
```
{% endtab %}

{% tab title="Php" %}
```php
<?php

/**
 * Sends a POST request to create a new split configuration.
 *
 * @param string $baseUrl The base URL (e.g., 'https://api.paymentgate.com').
 * @param string $serviceToken The value for the 'X-Service-Token' header.
 * @return array|null The decoded JSON response array, or null on failure.
 */
function createSplitConfigCurl(string $baseUrl, string $serviceToken): ?array
{
    $url = $baseUrl . '/split';
    
    $payload = [
        "subaccounts" => ["0429425494", "0792382349"],
        "shares" => [50, 50],
        "businessNames" => ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
        "bearer" => 0,
        "perc_flat" => "percentage",
        "alias" => "New Split",
        "base_amount" => 100
    ];

    $jsonPayload = json_encode($payload);

    $ch = curl_init($url);
    
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'POST');
    curl_setopt($ch, CURLOPT_POSTFIELDS, $jsonPayload);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        'X-Service-Token: ' . $serviceToken,
        'Content-Type: application/json',
    ]);

    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($httpCode >= 400 || $response === false) {
        error_log("API Error (HTTP $httpCode): " . ($response === false ? curl_error($ch) : $response));
        return null;
    }

    return json_decode($response, true);
}
// Example Call:
// $result = createSplitConfigCurl('https://api.paymentgate.com', 'YOUR_SERVICE_TOKEN');
// print_r($result);
```
{% endtab %}

{% tab title="Laravel" %}
```php
<?php
namespace App\Services;

use Illuminate\Support\Facades\Http;

class SplitService
{
    /**
     * Creates a new split payment configuration using the Laravel HTTP facade.
     *
     * @param string $baseUrl The base API endpoint URL.
     * @param string $serviceToken The value for the 'X-Service-Token' header.
     * @return array|null The decoded JSON response array, or null on failure.
     */
    public function createSplitConfig(string $baseUrl, string $serviceToken): ?array
    {
        $url = $baseUrl . '/split';
        
        $payload = [
            "subaccounts" => ["0429425494", "0792382349"],
            "shares" => [50, 50],
            "businessNames" => ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
            "bearer" => 0,
            "perc_flat" => "percentage",
            "alias" => "New Split",
            "base_amount" => 100
        ];

        try {
            $response = Http::withHeaders([
                'X-Service-Token' => $serviceToken,
            ])->post($url, $payload);
            
            // Checks for error status codes and throws an exception if one is found
            $response->throw();

            return $response->json();

        } catch (\Exception $e) {
            logger()->error('Split API Call Failed: ' . $e->getMessage(), ['url' => $url]);
            return null;
        }
    }
}
// Example Call (inside a Laravel class):
// $result = (new SplitService())->createSplitConfig(env('BASE_URL'), env('SERVICE_TOKEN'));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
from typing import Dict, Any, Optional

def create_split_config(base_url: str, service_token: str) -> Optional[Dict[str, Any]]:
    """
    Sends a POST request to create a split configuration.

    :param base_url: The base URL of the API.
    :param service_token: The X-Service-Token value.
    :return: The decoded JSON response dictionary, or None on failure.
    """
    url = f"{base_url}/split"
    
    payload = {
        "subaccounts": ["0429425494", "0792382349"],
        "shares": [50, 50],
        "businessNames": ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
        "bearer": 0,
        "perc_flat": "percentage",
        "alias": "New Split",
        "base_amount": 100
    }

    headers = {
        'X-Service-Token': service_token,
        'Content-Type': 'application/json'
    }

    try:
        response = requests.post(url, headers=headers, json=payload, timeout=10)
        response.raise_for_status() # Raises an HTTPError for bad responses (4xx or 5xx)
        return response.json()

    except requests.exceptions.RequestException as e:
        print(f"Error creating split config at {url}: {e}")
        return None

# Example Call:
# result = create_split_config('https://api.paymentgate.com', 'YOUR_SERVICE_TOKEN')
# if result:
#     print(result)
```
{% endtab %}

{% tab title="Django" %}
```py
# In a Django app's services.py or utils.py file

from typing import Dict, Any, Optional
import requests
from django.conf import settings # Use settings for base URL/Token

def create_split_config_django(base_url: str, service_token: str) -> Optional[Dict[str, Any]]:
    # ... (same implementation as Python's create_split_config function above)
    url = f"{base_url}/split"
    
    payload = {
        "subaccounts": ["0429425494", "0792382349"],
        "shares": [50, 50],
        "businessNames": ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
        "bearer": 0,
        "perc_flat": "percentage",
        "alias": "New Split",
        "base_amount": 100
    }

    headers = {'X-Service-Token': service_token, 'Content-Type': 'application/json'}

    try:
        response = requests.post(url, headers=headers, json=payload, timeout=10)
        response.raise_for_status() 
        return response.json()

    except requests.exceptions.RequestException as e:
        # In Django, you would use logging.error instead of print
        print(f"Error creating split config: {e}") 
        return None

# Example Call (e.g., in a View or another Service):
# BASE_URL = settings.API_BASE_URL 
# SERVICE_TOKEN = settings.SERVICE_TOKEN
# result = create_split_config_django(BASE_URL, SERVICE_TOKEN)
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
message = "hello world"
puts message
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.IOException;

public class SplitPaymentClient {

    private static final HttpClient client = HttpClient.newHttpClient();

    /**
     * Sends a synchronous POST request to create a split payment configuration.
     *
     * @param baseUrl The base API endpoint URL.
     * @param serviceToken The value for the 'X-Service-Token' header.
     * @return The raw JSON response body as a String.
     * @throws IOException If an I/O error occurs.
     * @throws InterruptedException If the operation is interrupted.
     * @throws RuntimeException If the HTTP status code is not successful (non-2xx).
     */
    public static String createSplitConfig(String baseUrl, String serviceToken) 
            throws IOException, InterruptedException {
        
        String url = baseUrl + "/split";
        
        // Manual construction of the JSON payload string
        String jsonPayload = """
            {
                "subaccounts": ["0429425494", "0792382349"],
                "shares": [50, 50],
                "businessNames": ["SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"],
                "bearer": 0,
                "perc_flat": "percentage",
                "alias": "New Split",
                "base_amount": 100
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("X-Service-Token", serviceToken)
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(jsonPayload))
                .build();

        HttpResponse<String> response = client.send(
            request, 
            HttpResponse.BodyHandlers.ofString()
        );

        int statusCode = response.statusCode();
        if (statusCode < 200 || statusCode >= 300) {
            throw new RuntimeException("Split Config Creation Failed. Status Code: " + statusCode + 
                                       ". Body: " + response.body());
        }

        return response.body();
    }
}
// Example Call:
// String resultJson = SplitPaymentClient.createSplitConfig("https://api.paymentgate.com", "YOUR_SERVICE_TOKEN");
```
{% endtab %}

{% tab title="Spring Boot" %}
```java
// Spring Boot Service Class using WebClient

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;
import java.util.List;
import java.util.Map;

@Service
public class SplitService {

    private final WebClient webClient;
    private final String serviceToken;

    // Constructor injection for WebClient and reading token from application.properties/yaml
    public SplitService(WebClient.Builder webClientBuilder, 
                        @Value("${api.base-url}") String baseUrl,
                        @Value("${api.service-token}") String serviceToken) {
        this.webClient = webClientBuilder.baseUrl(baseUrl).build();
        this.serviceToken = serviceToken;
    }

    /**
     * Creates a new split configuration using Spring's WebClient.
     * * @return Mono<Map> A reactive stream containing the response body as a Map.
     */
    public Mono<Map> createSplitConfig() {
        
        Map<String, Object> payload = Map.of(
            "subaccounts", List.of("0429425494", "0792382349"),
            "shares", List.of(50, 50),
            "businessNames", List.of("SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE"),
            "bearer", 0,
            "perc_flat", "percentage",
            "alias", "New Split",
            "base_amount", 100
        );

        return webClient.post()
                .uri("/split") // Combines with the base URL set in the constructor
                .header("X-Service-Token", serviceToken)
                .bodyValue(payload) // Spring handles JSON serialization automatically
                .retrieve()
                .bodyToMono(Map.class) // Expecting a JSON response mapped to a Map
                .doOnError(e -> System.err.println("WebClient Error: " + e.getMessage()));
    }
}
// Example Call (e.g., in a Controller):
// splitService.createSplitConfig().block(); // block() to wait for result in non-reactive contexts
```
{% endtab %}

{% tab title="C#" %}
```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using System.Text.Json;
using System.Collections.Generic;

public class SplitApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _serviceToken;

    public SplitApiClient(string baseUrl, string serviceToken)
    {
        _httpClient = new HttpClient { BaseAddress = new Uri(baseUrl) };
        _httpClient.DefaultRequestHeaders.Accept.Clear();
        _httpClient.DefaultRequestHeaders.Accept.Add(
            new MediaTypeWithQualityHeaderValue("application/json"));
        
        _serviceToken = serviceToken;
    }

    public async Task<string> CreateSplitConfigAsync()
    {
        var payload = new
        {
            subaccounts = new List<string> { "0429425494", "0792382349" },
            shares = new List<int> { 50, 50 },
            businessNames = new List<string> { "SANUSI OLUWOLE BLESSING", "SANUSI BLESSING OLUWOLE" },
            bearer = 0,
            perc_flat = "percentage",
            alias = "New Split",
            base_amount = 100
        };

        // Serialize the anonymous object to a JSON string
        string jsonPayload = JsonSerializer.Serialize(payload);
        
        // Create the request content
        var content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");

        // Add the custom header
        _httpClient.DefaultRequestHeaders.Remove("X-Service-Token");
        _httpClient.DefaultRequestHeaders.Add("X-Service-Token", _serviceToken);

        try
        {
            HttpResponseMessage response = await _httpClient.PostAsync("/split", content);

            // Throw an exception if the status code is not successful
            response.EnsureSuccessStatusCode(); 

            // Read the response body as a string (JSON)
            return await response.Content.ReadAsStringAsync();
        }
        catch (HttpRequestException e)
        {
            Console.WriteLine($"Request exception: {e.Message}");
            return null;
        }
    }
}
// Example Call:
// var client = new SplitApiClient("https://api.paymentgate.com", "YOUR_SERVICE_TOKEN");
// var result = await client.CreateSplitConfigAsync();
// Console.WriteLine(result);
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
        "user_id": 572,
        "alias": "New Split",
        "split_code": "06440986",
        "subaccounts": "0429425494,0792382349",
        "banks": "090146,090146",
        "shares": "50,50",
        "bearer": "!Main",
        "perc_flat": "percentage",
        "base_amount": null,
        "gateway": "bank",
        "split_group": "69382589750864.06440986",
        "aggregator": 1,
        "updated_at": "2025-12-09T13:35:05.000000Z",
        "created_at": "2025-12-09T13:35:05.000000Z",
        "id": 1023
    },
    "message": "success"
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

