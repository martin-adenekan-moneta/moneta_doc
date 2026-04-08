# Payment Using Debit Instruction

Initiates a Debit Instruction Using the generated code and Reference on a User Account. The Client has the flexibility to pass just the name\_enquiry\_id if they don't want to specify the beneficiary\_account\_number and destination\_bank\_code, and vice versa.

For Disbursement/Payout, the beneficiary\_account\_number and destination\_bank\_code are required.

##

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v2/debit-instruction/debit`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

<table><thead><tr><th width="189">Name</th><th width="106">Type</th><th width="113">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>mandate_ref</code></td><td>string</td><td>required</td><td>The reference ID of the mandate. This is Required together with the mandate_code field. This is not required when Disbursement is set to 'true'. Example: <code>MT-JJF4342JF</code></td></tr><tr><td><code>mandate_code</code></td><td>string</td><td>required</td><td>The code of the mandate. This is Required for scenarios like collections or money transfer from an Account. This is not required when Disbursement is set to 'true'. Example: <code>RC/1234/567890</code></td></tr><tr><td><code>amount</code></td><td>string</td><td>optional</td><td>The amount to be debited. Example: <code>1000</code></td></tr><tr><td><code>beneficiary_account_number</code>   </td><td>string</td><td>optional</td><td>optional string The account number of the beneficiary. Example: <code>0000000000</code></td></tr><tr><td><code>destination_bank_code</code>   </td><td>string</td><td>optional</td><td> The bank code of the beneficiary bank. Example: <code>000000</code></td></tr><tr><td><code>name_enquiry_id</code>   </td><td>integer</td><td>optional</td><td>optional The ID of the name enquiry associated with the mandate. Example: <code>1</code></td></tr><tr><td><code>disbursement</code></td><td>boolean</td><td>optional</td><td>A boolean indicating whether the mandate is for disbursement or not. This is required for scenarios like salary payment of Transfers from a Particular Pool Account to any desired Destination Account Example: <code>true</code></td></tr></tbody></table>

Example

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "baseUrl/v2/debit-instruction/debit" \
    --header "X-Service-Token: ......................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"mandate_ref\": \"MT-JJF4342JF\",
    \"mandate_code\": \"RC\\/1234\\/567890\",
    \"amount\": \"1000\",
    \"beneficiary_account_number\": \"0000000000\",
    \"destination_bank_code\": \"000000\",
    \"name_enquiry_id\": 1,
    \"disbursement\": true
}"
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    baseUrl+"/v2/debit-instruction/debit"
);

const headers = {
    "X-Service-Token": ".................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

let body = {
    "mandate_ref": "MT-JJF4342JF",
    "mandate_code": "RC\/1234\/567890",
    "amount": "1000",
    "beneficiary_account_number": "0000000000",
    "destination_bank_code": "000000",
    "name_enquiry_id": 1,
    "disbursement": true
};

fetch(url, {
    method: "POST",
    headers,
    body: JSON.stringify(body),
}).then(response => response.json());
```
{% endtab %}

{% tab title="Php" %}
```php
$client = new \GuzzleHttp\Client();
$url = $baseurl.'/v2/debit-instruction/debit';
$response = $client->post(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '.........................',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
        'json' => [
            'mandate_ref' => 'MT-JJF4342JF',
            'mandate_code' => 'RC/1234/567890',
            'amount' => '1000',
            'beneficiary_account_number' => '0000000000',
            'destination_bank_code' => '000000',
            'name_enquiry_id' => 1,
            'disbursement' => true,
        ],
    ]
);
$body = $response->getBody();
print_r(json_decode((string) $body));
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
 status: true,
 message: "string",
 data: {
 "session_id": "string",
 "transaction_status": "string",
 "moneta_reference": "string",
 "mandate_ref": "string"
  }
 statusCode: 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
 "status": false,
"data": {
 "session_id": "string|null",
 "transaction_status": "string|null",
 "moneta_reference": "string |null",
 "mandate_ref": "string|null",
     },
  "message": "string"
 "statusCode": 400,
}
```
{% endtab %}

{% tab title="500" %}
```json
{
 "status": false,
"data": {
 "session_id": "string|null",
 "transaction_status": "string|null",
 "moneta_reference": "string |null",
 "mandate_ref": "string|null",
     },
  "message": "string"
 "statusCode": 500,
}
```
{% endtab %}
{% endtabs %}
