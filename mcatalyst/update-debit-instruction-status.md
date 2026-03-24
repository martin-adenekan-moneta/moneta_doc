# Update Debit Instruction Status

This endpoint is used to suspend or activate a Debit Instruction.



## Debit Instruction Status

<mark style="color:green;">`POST`</mark> `/debit-instruction/disable-enable`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| Content-Type      | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th width="128">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>mandate_ref</code></td><td>string</td><td>The reference ID of the mandate e.g MT-JJEe3CJ</td></tr><tr><td><code>mandate_code</code></td><td>string</td><td>The code of the mandate. e.g: <code>RC/1234/567890</code></td></tr><tr><td><code>mandate_status</code></td><td>number</td><td>The new status of the mandate. 1 = Active, 2 = Suspended, 3 = Deleted</td></tr><tr><td><code>workflow_status</code></td><td>number</td><td>The new workflow status of the mandate. 1 = New Debit Instruction Initiated By Client, 2 = Debit Instruction Authorized By Client, 3 = Debit Instruction Rejected By Client, 4 = Debit Instruction Approved By Client, 5 =Debit Instruction Disapproved By Client, 8 = Debit Instruction Approved By Bank</td></tr></tbody></table>

Body



{% tabs %}
{% tab title="JavaScript" %}
```javascript
const url = new URL(baseUrl);

const headers = {
    "X-Service-Token": "............",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "mandate_ref": "...........",
    "mandate_code": "...........",
    "mandate_status": "...........",
    "workflow_status": "............"
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
$url = baseUrl;
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '............',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'mandate_ref' => '......',
            'mandate_code' => '..........',
            'mandate_status' => '.........',
            'workflow_status' => '...........',
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

url = 'https://api.moneta.ng/api/v2/debit-instruction/disable-enable'
payload = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890",
    "mandate_status": 11613.31890586,
    "workflow_status": 11613.31890586
}
headers = {
  'X-Service-Token': '6337|0IjdE3othWKaECr2GL0ynUWGlmsdFsYI90bqyn7Md2cc154d',
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
import com.google.gson.Gson; // External Library: com.google.code.gson

public class MonetaMandateManager {
    public static void main(String[] args) {
        String url = "https://api.moneta.ng/api/v2/debit-instruction/disable-enable";
        String token = "........"; // Replace with your actual token

        try {
            // 1. Construct the Payload
            Map<String, Object> payload = new HashMap<>();
            payload.put("mandate_ref", "........");
            payload.put("mandate_code", ".........");
            payload.put("mandate_status", ..........);
            payload.put("workflow_status", ..........);

            // Convert Map to JSON string
            String jsonPayload = new Gson().toJson(payload);

            // 2. Build the HttpClient and Request
            HttpClient client = HttpClient.newHttpClient();
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(URI.create(url))
                    .header("X-Service-Token", token)
                    .header("Content-Type", "application/json")
                    .header("Accept", "application/json")
                    .POST(HttpRequest.BodyPublishers.ofString(jsonPayload))
                    .build();

            // 3. Send the Request and handle the response
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

            // 4. Output the result
            if (response.statusCode() == 200) {
                System.out.println("Status updated successfully:");
                System.out.println(response.body());
            } else {
                System.err.println("❌ Request failed. HTTP Status: " + response.statusCode());
                System.err.println("Response: " + response.body());
            }

        } catch (Exception e) {
            System.err.println("An error occurred during the API call.");
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
        "mandate_ref": "MT-JJF4342JF",
        "mandate_code": "RC/1234/567890",
        "response_message": "Mandate Status Successfully Changed",
        "mandate_status": 2,
        "workflow_status": 2,
        "workflow_description": "Approved",
        "rejection_reason": "",
        "rejection_comment": ""
    },
    "message": "Mandate Status Successfully Changed",
    "statusCode": 201
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "data": [],
    "statusCode": 400,
    "message": "Invalid request..Check Your Request Body"
}
```
{% endtab %}

{% tab title="401" %}
```bash
{
    "status": false,
    "data": [],
    "statusCode": 410,
    "message": "Unable to process request"
}
```
{% endtab %}

{% tab title="420" %}
```bash
{
    "status": false,
    "data": [],
    "statusCode": 420,
    "message": "Unable to process request"
}
```
{% endtab %}

{% tab title="500" %}
```bash
{
    "status": false,
    "data": [],
    "statusCode": 500,
    "message": "Unable to process request"
}
```
{% endtab %}
{% endtabs %}

