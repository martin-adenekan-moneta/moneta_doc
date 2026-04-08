# BVN Name Enquiry

This endpoint allows you to perform an account details enquiry against the provided BVN including name match verification.

### Perform an account name enquiry and BVN verification. <a href="#kyc-automation-postapi-v2-bvn-account_identity_check" id="kyc-automation-postapi-v2-bvn-account_identity_check"></a>

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/bvn/account_identity_check`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| Content-Type      | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th>Name</th><th width="167">Type</th><th width="111">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>bvn</code></td><td>string</td><td>required</td><td>The Bank Verification Number (BVN) of the account holder. Must be exactly 11 digits.</td></tr><tr><td><code>account_number</code></td><td>string</td><td>required</td><td>The account number to be verified. Must be exactly 10 digits. Example: <code>consequatur</code></td></tr><tr><td><code>institution_code</code></td><td>string</td><td>required</td><td>The institution code of the bank where the account is held(6 Digits). Example: <code>consequatur</code></td></tr><tr><td><code>first_name</code></td><td>string</td><td>required</td><td>The first name of the account holder.</td></tr><tr><td><code>last_name</code></td><td>string</td><td>required </td><td>The last name of the account holder.</td></tr></tbody></table>

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/bvn/account_identity_check" \
    --header "X-Service-Token: ......................." \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"bvn\": \".............\",
    \"account_number\": \"..............\",
    \"institution_code\": \"..................\",
    \"first_name\": \".............\",
    \"last_name\": \"...............\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```js
const url = new URL(
    "https://api.moneta.ng/api/v2/bvn/account_identity_check"
);

const headers = {
    "X-Service-Token": "..................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "bvn": "............",
    "account_number": "...............",
    "institution_code": "....................",
    "first_name": "..............",
    "last_name": "............."
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
$url = 'https://api.moneta.ng/api/v2/bvn/account_identity_check';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '...............................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'bvn' => '............',
            'account_number' => '.................',
            'institution_code' => '...................',
            'first_name' => '.............',
            'last_name' => '..................',
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

url = 'https://api.moneta.ng/api/v2/bvn/account_identity_check'
payload = {
    "bvn": "..........",
    "account_number": "................",
    "institution_code": ".............",
    "first_name": "............",
    "last_name": "..........."
}
headers = {
  'X-Service-Token': '.............................................',
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
         "bank_verification_number": "22448484848",
         "account_number": "1234567890",
         "account_name": "John Doe",
         "first_name": "John",
         "last_name": "Doe",
         "institution_code": "120443",
         "bvn_match": true,
         "name_match_percentage": 95
     }
    "message": "Success",
    "statusCode": 200
 }
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "data": {},
    "message": "The selected institution code is invalid. Kindly Refer to Our Bank Codes Endpoint to Get the required Bank Code.",
    "statusCode": 400
}
```
{% endtab %}
{% endtabs %}

##
