# Step 1: BVN  Querying

This endpoint is used to request a BVN validation for a user.

## Query User BVN

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/bvn/query`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th width="156">Parameter</th><th width="84">Type</th><th width="117">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>scope</code></td><td>string</td><td>required</td><td>The scope of the bvn validation. This can be either "accounts" TO RETRIEVE ACCOUNTS or "profile" TO RETRIEVE PROFILE</td></tr><tr><td>bvn</td><td>int</td><td>required</td><td>The BVN of the user. Example: <code>22448484848</code></td></tr><tr><td>channel_code</td><td>string</td><td>required</td><td>The channel/medium used in Processing the BVN. This can be any of the following: mobile_app, customer_web_portal, internal_web_portal, atm, pos, other. Example: <code>internal_web_portal</code></td></tr><tr><td>otp_method</td><td>string</td><td>required</td><td>The OTP method to be used. This is to Specify Phone Number that must be tied to an Account Under the BVN that the Customer Can Use to Receive the OTP. Example: <code>08012345678</code></td></tr></tbody></table>

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST "{{baseUrl}}/v2/bvn/query" \
  -H "Content-Type: application/json" \
  -H "X-Service-Token: YOUR_SERVICE_TOKEN" \
  -d '{
    "scope": "profile",
    "channel_code": "internal_web_portal",
    "bvn": "22302148176",
    "otp_method": "08133667142"
  }'
```
{% endtab %}

{% tab title="Javascript" %}
```js
const baseUrl = "https://your-api-base-url.com"; // Replace with your base URL

async function queryBvn() {
  const response = await fetch(`${baseUrl}/v2/bvn/query`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "X-Service-Token": "" // Add your token here
    },
    body: JSON.stringify({
      scope: "profile",
      channel_code: "internal_web_portal",
      bvn: "22302148176",
      otp_method: "08133667142"
    })
  });

  const data = await response.text();
  console.log(data);
}

queryBvn();
```
{% endtab %}

{% tab title="PHP" %}
```php
$client = new Client();
$headers = [
  'X-Service-Token' => ''
];
$body = '{
  "scope": "profile",
  "channel_code": "internal_web_portal",
  "bvn": "22302148176",
  "otp_method": "08133667142"
}';
$request = new Request('POST', '{{baseUrl}}/v2/bvn/query', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();
```
{% endtab %}

{% tab title="Laravel" %}
```php
use Illuminate\Support\Facades\Http;

$baseUrl = config('services.api.base_url'); // Or env('BASE_URL')

$response = Http::withHeaders([
    'X-Service-Token' => '', // Add your token here
])->post("{$baseUrl}/v2/bvn/query", [
    'scope'        => 'profile',
    'channel_code' => 'internal_web_portal',
    'bvn'          => '22302148176',
    'otp_method'   => '08133667142',
]);

echo $response->body();
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

base_url = "{{baseUrl}}"  

headers = {
    "X-Service-Token": "",  # Add your token here
    "Content-Type": "application/json"
}

payload = {
    "scope": "profile",
    "channel_code": "internal_web_portal",
    "bvn": "22302148176",
    "otp_method": "08133667142"
}

response = requests.post(f"{base_url}/v2/bvn/query", headers=headers, json=payload)

print(response.text)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class BvnQuery {
    public static void main(String[] args) throws IOException, InterruptedException {
        String baseUrl = "{{baseUrl}}";

        String jsonPayload = """
            {
              "scope": "profile",
              "channel_code": "internal_web_portal",
              "bvn": "22302148176",
              "otp_method": "08133667142"
            }
            """;

        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(baseUrl + "/v2/bvn/query"))
                .header("Content-Type", "application/json")
                .header("X-Service-Token", "") // Add your token here
                .POST(HttpRequest.BodyPublishers.ofString(jsonPayload))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        System.out.println(response.body());
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
    "data": [customer_reference: "MNT-BVN-98-8443-39******"],
    "message": "Bvn Details Retrieval Initiated",
    "status": true,
    "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "data": [],
    "message": "Something Went Wrong",
    "status": false,
    "statusCode": 400
}
```
{% endtab %}
{% endtabs %}
