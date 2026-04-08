# List of Banks

This endpoint retrieves the list of avialable banks&#x20;

##

<mark style="color:green;">`POST`</mark> [`{{baseUrl}}`](./#base-url-for-mcatalyst)`/v2/get-banks`

&#x20;Remember to include your X-Service-Token header as [shown here](./)

**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| Content-Type      | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

Body

{% tabs %}
{% tab title="Curl" %}
```bash

curl --request POST \
    "{{baseUrl}}/v2/bvn/account_identity_check" \
    --header "X-Service-Token: ................................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"bvn\": \"............\",
    \"account_number\": \"....................\",
    \"institution_code\": \"..............\",
    \"first_name\": \"................\",
    \"last_name\": \"................\"
}"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = new URL(baseUrl+"/v2/get-banks");

const headers = {
    "X-Service-Token": "..................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

fetch(url, {
    method: "GET",
    headers,
}).then(response => response.json());
```
{% endtab %}

{% tab title="PHP" %}
```php
$client = new \GuzzleHttp\Client();
$url = baseUrl+'/v2/get-banks';
$response = $client->get(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '..............',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
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

url = baseUrl+'/v2/get-banks'
headers = {
  'X-Service-Token': '.................................',
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request('GET', url, headers=headers)
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
"message": "Banks fetched successfully",
"data": [
 {
  "id": 1,
  "bank_name": "Bank A",
  "mandate_bank_code": "000"
  "institutionCode": "123456",
  "moneta_code": "040",
  "category": ""
 },
],
"statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "message": "Error",
    "data": [],
    "statusCode": 400
}
```
{% endtab %}
{% endtabs %}
