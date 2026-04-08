# Retrieve Balance Info

This endpoint checks the balance of a Debit Instruction using the mandate\_ ref and code.

## Debit Instruction Balance Information

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/debit-instruction/account/balance-enquiry`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name             | Type   | Description                                              |
| ---------------- | ------ | -------------------------------------------------------- |
| `mandate_ref`    | string | The reference ID of the mandate. Example: `MT-JJF4342JF` |
| `mandate_code`   | string | The code of the mandate. Example: `RC/1234/567890`       |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/debit-instruction/account/balance-enquiry" \
    --header "X-Service-Token: ......................................." \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"mandate_ref\": \"...........\",
    \"mandate_code\": \"...........\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    "https://api.moneta.ng/api/v2/debit-instruction/account/balance-enquiry"
);

const headers = {
    "X-Service-Token": ".....................................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "mandate_ref": "..............",
    "mandate_code": "............."
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
$url = 'https://api.moneta.ng/api/v2/debit-instruction/account/balance-enquiry';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '......................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'mandate_ref' => '...............',
            'mandate_code' => '....................',
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

url = 'https://api.moneta.ng/api/v2/debit-instruction/account/balance-enquiry'
payload = {
    "mandate_ref": "...........",
    "mandate_code": "..............."
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
        "available_balance": 1000,
        "ledger_balance": 1000,
        "customer_account_no": "1234567890"
    },
    "message": "Balance Enquiry Successful",
    "statusCode": 200
 }
```
{% endtab %}

{% tab title="400" %}
```json
{
  "status": false,
    "data": [],
    "message": "Validation Errors",
    "statusCode": 400
}
```
{% endtab %}

{% tab title="500" %}
```json
   "status": false,
    "data": [],
    "message": "Unable to Process Request",
    "statusCode": 500
```
{% endtab %}
{% endtabs %}
