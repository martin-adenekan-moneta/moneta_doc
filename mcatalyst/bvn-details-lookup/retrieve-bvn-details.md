# Retrieve BVN Details

After Customer OTP returns a success message you are advised to pragmatically wait for 5-7seconds before calling this endpoint to fetch the BVN details

## BVN Details Query

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/bvn/query`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name                 | Type   | Description                                                                                      |
| -------------------- | ------ | ------------------------------------------------------------------------------------------------ |
| `customer_reference` | string | The unique reference for the BVN Details Retrieval Instance. Example: `MNT-BVN-98-8443-3949493`  |
| `scope`              | string | The scope of the BVN validation. This can be either "accounts" or "profile". example "`profile`" |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/bvn/details" \
    --header "X-Service-Token: .................................." \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"scope\": \"profile\",
    \"customer_reference\": \"...................\"
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
{% tab title="Profile Scope (200)" %}
```json
{
    "data": {
        "DateOfBirth": "string",
        "gender": "string",
        "address": "string",
        "city": "string",
        "first_name": "string",
        "surname": "string",
        "middle_name": "string",
        "marital_status": "string",
        "nationality": "string",
        "state_of_origin": "string",
        "lga_of_origin": "string",
        "nin": "string",
        "title": "string",
        "watchlisted": "integer",
    },
    "message": "string",
    "status": true,
    "statusCode": 200
}
```
{% endtab %}

{% tab title="Account Scope (200)" %}
```json
{
    "data": {
        {
          "accountnumber": "string",
          "accountDesignation": "integer",
          "accountname": "string",
          "accountstatus": "integer",
          "accounttype": "integer",
          "institution": "integer",
          "BankCode": "string",
          "currency": "string",
          "branch": "string",
          "BankName": "string",
          "AccountDesignationName": "string",
          "AccountTypeName": "string",
}
     {
          "accountnumber": "string",
          "accountDesignation": "integer",
          "accountname": "string",
          "accountstatus": "integer",
          "accounttype": "integer",
          "institution": "integer",
          "BankCode": "string",
          "currency": "string",
          "branch": "string",
          "BankName": "string",
          "AccountDesignationName": "string",
          "AccountTypeName": "string",
}
    },
    "message": "string",
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

{% tab title="500" %}
```json
{
    "data": [],
    "message": "string",
    "status": false,
    "statusCode": 500
}
```
{% endtab %}
{% endtabs %}
