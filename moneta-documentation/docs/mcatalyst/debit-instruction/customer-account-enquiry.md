# Customer Account Enquiry

This Endpoint Allows our merchants & clients to validate a Customer Account Number or Bank Verification Number and checks if the Account Number is tied to the submitted BVN.

## Account Name Enquiry

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v2/debit-instruction/account/name-enquiry`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name               | Type   | Description                                                      |
| ------------------ | ------ | ---------------------------------------------------------------- |
| `bvn`              | string | The Bank Verification Number (11 digits). Example: `22345678901` |
| `account_number`   | string | The Account Number (10 digits). Example: `1234567890`            |
| `institution_code` | string | The code of the financial institution. Example: `00190`          |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "baseUrl/v2/debit-instruction/account/name-enquiry" \
    --header "X-Service-Token: ......................................." \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"bvn\": \"...........\",
    \"account_number\": \"...........\",
    \"institution_code\": \".....\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    baseUrl+"/v2/debit-instruction/account/name-enquiry"
);

const headers = {
    "X-Service-Token": ".....................................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "bvn": "..............",
    "account_number": ".............",
    "institution_code": "....."
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
$url = $baseUrl.'/v2/debit-instruction/account/name-enquiry';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '......................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'bvn' => '...............',
            'account_number' => '....................',
            'institution_code' => '....',
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

url = baseUrl+'/v2/debit-instruction/account/name-enquiry'
payload = {
    "bvn": "...........",
    "account_number": "...............",
    "institution_code": "...."
}
headers = {
  'X-Service-Token': '....................',
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
     "status": true,
     "data": {
         "account_number": "..........",
         "account_name": "John Doe",
         "kyc_level": 2,
     },
     "message": "00",
     "statusCode": 200
 }
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "data": [........],
    "statusCode": 400,
    "message": "string"
}
```
{% endtab %}
{% endtabs %}
