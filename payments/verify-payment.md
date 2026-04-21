# Verify Payment

## Payment / Transaction Verification

<mark style="color:green;">`POST`</mark>  `{{`[`baseUrl`](./#overview)`}}/`transaction/charge/verify/reference

\<Verify Payment status>

**Headers**

| Name            | Value                                              |
| --------------- | -------------------------------------------------- |
| Content-Type    | `application/json`                                 |
| X-Service-Token | `<`[`service_token`](./#payment-initialization)`>` |

**Body**

| Name        | Type   | Description                  |
| ----------- | ------ | ---------------------------- |
| `reference` | string | Transaction Reference Number |

Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
async function verifyTransaction(baseUrl, serviceToken, reference) {
  const url = `${baseUrl}/transaction/charge/verify/reference`;
  
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'X-Service-Token': serviceToken,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ reference })
    });
    return await response.json();
  } catch (error) {
    console.error('Verification Error:', error);
    return null;
  }
}
```
{% endtab %}

{% tab title="Php" %}
```php
<?php
function verifyTransactionCurl($baseUrl, $token, $reference) {
    $url = $baseUrl . '/transaction/charge/verify/reference';
    $payload = json_encode(['reference' => $reference]);

    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        "X-Service-Token: $token",
        "Content-Type: application/json"
    ]);

    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}
```
{% endtab %}

{% tab title="Laravel" %}
```php
<?php

use Illuminate\Support\Facades\Http;

public function verify(string $baseUrl, string $token, string $reference) {
    return Http::withHeaders(['X-Service-Token' => $token])
        ->post("$baseUrl/transaction/charge/verify/reference", [
            'reference' => $reference
        ])->json();
}
```
{% endtab %}

{% tab title="Python" %}
```python


import requests

def verify_transaction(base_url, token, reference):
    url = f"{base_url}/transaction/charge/verify/reference"
    headers = {"X-Service-Token": token, "Content-Type": "application/json"}
    payload = {"reference": reference}
    
    response = requests.post(url, json=payload, headers=headers)
    return response.json()
```
{% endtab %}

{% tab title="Django" %}
```
# Typically placed in a services.py file
import requests
from django.conf import settings

def django_verify_payment(reference):
    url = f"{settings.API_BASE_URL}/transaction/charge/verify/reference"
    headers = {"X-Service-Token": settings.SERVICE_TOKEN}
    
    response = requests.post(url, json={"reference": reference}, headers=headers)
    return response.json() if response.ok else None
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.*;

public String verifyTransaction(String baseUrl, String token, String reference) throws Exception {
    HttpClient client = HttpClient.newHttpClient();
    String json = "{\"reference\":\"" + reference + "\"}";

    HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create(baseUrl + "/transaction/charge/verify/reference"))
        .header("X-Service-Token", token)
        .header("Content-Type", "application/json")
        .POST(HttpRequest.BodyPublishers.ofString(json))
        .build();

    return client.send(request, HttpResponse.BodyHandlers.ofString()).body();
}
```
{% endtab %}

{% tab title="Spring Boot" %}
```java
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;
import java.util.Map;

public Mono<Map> verifyTransaction(String reference) {
    return webClient.post()
        .uri("/transaction/charge/verify/reference")
        .header("X-Service-Token", serviceToken)
        .bodyValue(Map.of("reference", reference))
        .retrieve()
        .bodyToMono(Map.class);
}
```
{% endtab %}

{% tab title="C#" %}
```csharp
using System.Net.Http.Json;

public async Task<string> VerifyTransactionAsync(string baseUrl, string token, string reference) {
    using var client = new HttpClient();
    client.DefaultRequestHeaders.Add("X-Service-Token", token);
    
    var response = await client.PostAsJsonAsync(
        $"{baseUrl}/transaction/charge/verify/reference", 
        new { reference = reference }
    );
    
    return await response.Content.ReadAsStringAsync();
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
    "data": {
        "status": "pending", 
        "amount": 101.7,
        "gateway_scheme": {
            "id": 6,
            "gateway": "4",
            "scheme_name": "Flutterwave Scheme 3",
            "scheme_category": null,
            "actual_scheme_category": "percentage",
            "merchant_scheme_value": "1.7",
            "merchant_scheme_category": "percentage",
            "actual_scheme_value": "1.4",
            "status": "1",
            "created_at": null,
            "updated_at": null,
            "stamp_duty_fee": 50,
            "capped_fee": "2000",
            "merchant_capped_fee": "0",
            "commission_type": "percentage",
            "merchant_commission": "0",
            "agent_commission": "0",
            "reseller_commission": "0"
        },
        "payer_details": {
            "first_name": null,
            "last_name": null
        },
        "merchantName": "Martin Adenekan",
        "merchantThemeSettings": {
            "theme_color": null,
            "logo_url": null
        },
        "callback_url": "http://localhost:8000/callback?reference=MNTAD9SL35DGVC7FK0&paydate=2025-11-28&bank="
    },
    "message": ""
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": "success",
    "data": {
        "status": "error",
        "message": "Transaction not found"
    },
    "message": ""
}
```
{% endtab %}
{% endtabs %}

