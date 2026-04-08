# Get Customer OTP

Submit Customer OTP received for BVN validation. Kindly Note that this doesn't verify the otp but collects it.

## Query User BVN

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/bvn/query`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name                 | Type   | Description                                                                                     |
| -------------------- | ------ | ----------------------------------------------------------------------------------------------- |
| `customer_reference` | string | The unique reference for the BVN Details Retrieval Instance. Example: `MNT-BVN-98-8443-3949493` |
| `otp`                | string | The OTP received by the customer. Example: `123456`                                             |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/bvn/verify/otp" \
    --header "X-Service-Token: ...................................." \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"customer_reference\": \"......................\",
    \"otp\": \"123456\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```js
const url = new URL(
    "https://api.moneta.ng/api/v2/bvn/verify/otp"
);

const headers = {
    "X-Service-Token": "...............................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "customer_reference": "......................",
    "otp": "123456"
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
$url = 'https://api.moneta.ng/api/v2/bvn/verify/otp';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '..................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'customer_reference' => '................',
            'otp' => '123456',
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

url = 'https://api.moneta.ng/api/v2/bvn/verify/otp'
payload = {
    "customer_reference": ".....................",
    "otp": "123456"
}
headers = {
  'X-Service-Token': '.............................',
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request('POST', url, headers=headers, json=payload)
response.json()
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "data": [],
    "message": "OTP Received",
    "status": true,
    "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "data": [],
    "message": "Validation Error Message",
    "status": false,
    "statusCode": 400
}
```
{% endtab %}
{% endtabs %}
