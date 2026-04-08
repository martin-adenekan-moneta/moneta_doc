# BVN Validation Request

This endpoint is used to request a BVN validation for a user.

## Query User BVN

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/bvn/query`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name   | Type   | Description      |
| ------ | ------ | ---------------- |
| `name` | string | Name of the user |
| `age`  | number | Age of the user  |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/bvn/verify/otp" \
    --header "X-Service-Token: ................................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"customer_reference\": \".....................\",
    \"otp\": \".................\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```js
const url = new URL(
    "https://api.moneta.ng/api/v2/bvn/query"
);

const headers = {
    "X-Service-Token": "...................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "scope": "...............",
    "bvn": "...................",
    "channel_code": "..............",
    "otp_method": "................"
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
$url = 'https://api.moneta.ng/api/v2/bvn/query';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '.........................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'scope' => '.................',
            'bvn' => '............',
            'channel_code' => '...........',
            'otp_method' => '...........',
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

url = 'https://api.moneta.ng/api/v2/bvn/query'
payload = {
    "scope": "........",
    "bvn": "...........",
    "channel_code": "...........",
    "otp_method": "..........."
}
headers = {
  'X-Service-Token': '................',
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
    "data": [customer_reference: "MNT-BVN-98-8443-3949493"],
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
