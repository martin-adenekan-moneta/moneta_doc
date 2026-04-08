# Get All Debit Instructions

This endpoint fetches all debit instructions

## Account Name Enquiry

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v2/debit-instruction/all`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th width="128">Name</th><th width="139">Type</th><th width="120">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>date_from</code></td><td>string</td><td>optional</td><td>optional The start date for filtering mandates. Format: YYYY-MM-DD. Default: 30 days ago. </td></tr><tr><td><code>date_to</code></td><td>string</td><td>optional</td><td>optional The end date for filtering mandates. Format: YYYY-MM-DD. Default: current date. </td></tr></tbody></table>

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request GET \
    --get "baseUrl/v2/debit-instruction/all?date_from=consequatur&date_to=consequatur" \
    --header "X-Service-Token: ............................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json"
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    baseUrl+"/v2/debit-instruction/all"
);

const params = {
    "date_from": "........",
    "date_to": ".........",
};
Object.keys(params)
    .forEach(key => url.searchParams.append(key, params[key]));

const headers = {
    "X-Service-Token": "......................",
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
$url = $baseUrl.'/v2/debit-instruction/all';
$response = $client->get(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '...............................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'query' => [
            'date_from' => '...........',
            'date_to' => '...........',
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

url = baseUrl+'/v2/debit-instruction/all'
params = {
  'date_from': '..........',
  'date_to': '..............',
}
headers = {
  'X-Service-Token': '....................................',
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request('GET', url, headers=headers, params=params)
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
