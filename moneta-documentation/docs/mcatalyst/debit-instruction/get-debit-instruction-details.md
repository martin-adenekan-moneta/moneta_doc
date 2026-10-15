# Get Debit Instruction Details

This endpoint retrieves the status from debit instruction

## Get Details

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v2/debit-instruction/details`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name           | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| `mandate_ref`  | string | The reference ID of the mandate e.g MT-JJEe3CJ |
| `mandate_code` | string | The code of the mandate. e.g: `RC/1234/567890` |

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const url = new URL(baseUrl);

const headers = {
    "X-Service-Token": "......................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890"
};

fetch(url, {
    method: "POST",
    headers,
    body: JSON.stringify(body),
}).then(response => response.json());
```
{% endtab %}

{% tab title="PHP" %}
```php
$client = new \GuzzleHttp\Client();
$url = baseUrl.'/v2/debit-instruction/details';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '.......................................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'mandate_ref' => 'MT-JJF4342JF',
            'mandate_code' => 'RC/1234/567890',
        ],
    ]
);
$body = $response->getBody();
print_r(json_decode((string) $body));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = baseUrl+'/v2/debit-instruction/details'
payload = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890"
}
headers = {
  'X-Service-Token': '........................................',
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request('POST', url, headers=headers, json=payload)
response.json()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson; // Assuming you use Gson for JSON

public class MonetaClient {
    public static void main(String[] args) {
        try {
            // 1. Prepare the Data
            Map<String, String> body = new HashMap<>();
            body.put("mandate_ref", "........");
            body.put("mandate_code", "............");

            // Convert Map to JSON string
            String jsonBody = new Gson().toJson(body);

            // 2. Build the Request
            HttpClient client = HttpClient.newHttpClient();
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(URI.create(baseUrl))
                    .header("X-Service-Token", ".................................")
                    .header("Content-Type", "application/json")
                    .header("Accept", "application/json")
                    .POST(HttpRequest.BodyPublishers.ofString(jsonBody))
                    .build();

            // 3. Send the Request
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

            // 4. Handle the Response
            if (response.statusCode() == 200) {
                System.out.println("Success: " + response.body());
            } else {
                System.err.println("Error Status Code: " + response.statusCode());
                System.err.println("Response: " + response.body());
            }

        } catch (Exception e) {
            e.printStackTrace();
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
    "status": true,
    "data": {
        "id": 1,
        "amount": 1000,
        "narration": "Testing",
        "mandate_ref": "MT-JJF4342JF",
        "subscription_id": 1,
        "frequency": 0,
        "customer_reference": "1234567890",
        "response_message": "Welcome to ...... Kindly Pay 50 from the Account for Authorization to a PAYSTACK-TITAN BANK ACCOUNT 00*****",
        "workflow_status": "8",
        "mandate_status": "1",
        "workflow_description": "Bank Approved",
        "rejection_reason": "",
        "rejection_comment": ""
    },
    "message": "Successful request",
    "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "message": "The given data was invalid.",
    "data": {}
}
```
{% endtab %}
{% endtabs %}
