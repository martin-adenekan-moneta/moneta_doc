# Re-query Debit Instruction Transaction

Allows Clients to Re-Query a Debit Instruction Transaction status.

## All Transaction

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v2/debit-instruction/transaction_requery`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th width="179">Name</th><th width="94">Type</th><th width="120">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>mandate_ref</code></td><td>string</td><td>required</td><td>The reference ID of the mandate. Example: <code>MT-JJF4342JF</code></td></tr><tr><td><code>mandate_code</code></td><td>string</td><td>required</td><td>The code of the mandate. Example: <code>RC/1234/567890</code></td></tr><tr><td><code>session_id</code></td><td>string</td><td>optional</td><td>The session ID of the transaction. Example: <code>0000000000</code></td></tr><tr><td><code>moneta_reference</code></td><td>string</td><td>optional</td><td>The moneta reference of the transaction. Example: <code>0000000000</code></td></tr></tbody></table>

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "baseUrl/v2/debit-instruction/transaction_requery" \
    --header "X-Service-Token: ......................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"mandate_ref\": \"..............\",
    \"mandate_code\": \"RC\\/1234\\/567890\",
    \"session_id\": \"0000000000\",
    \"moneta_reference\": \"0000000000\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    baseUrl+"/v2/debit-instruction/transaction_requery"
);

const headers = {
    "X-Service-Token": ".........................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890",
    "session_id": "0000000000",
    "moneta_reference": "0000000000"
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
$url = $baseUrl.'/v2/debit-instruction/transaction_requery';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '..................................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'mandate_ref' => 'MT-JJF4342JF',
            'mandate_code' => 'RC/1234/567890',
            'session_id' => '0000000000',
            'moneta_reference' => '0000000000',
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

url = baseUrl+'/v2/debit-instruction/transaction_requery'
payload = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890",
    "session_id": "0000000000",
    "moneta_reference": "0000000000"
}
headers = {
  'X-Service-Token': '...........................',
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
      "data" : [
      "id": 1005,
      "mandate_ref": "MT/8a7aeb5bbcc3cf93ebfc",
      "status": "2",
      "status_description": "Suspended Mandate",
      "nibss_workflow_status": "4",
      "nibss_workflow_description": "Biller Initiated",
      "customer_phone": "8024305894",
      "customer_name": "Daniel Egbeleke",
      "customer_address": "1 dawko region, Abuja",
      "customer_email": "www.daniko15@gmail.com",
      "customer_account_name": "EGBELEKE DANIEL FIYINFOLUWA",
      "customer_account_no": "0343634941",
      "message": "Welcome to NIBSS e-mandate authentication service, a seamless and convenient authentication experience. Kindly proceed with a token payment of N50:00 into account number \"9880218357\" with Titan-Paystack Bank. This payment will trigger the authentication of your mandate. Thank You",
      "start_date": "2025-04-18",
      "end_date": "2026-11-29",
      "amount": "5000",
      "customer_bank_code": "058",
      "customer_reference": "danthebadguy22",
      "suscription_id": null
         ],
      "message": "Mandates fetched successfully",
      "status_code": 200
 }
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "data": null,
    "message": "Mandates not found",
    "status_code": 400
}
```
{% endtab %}
{% endtabs %}
