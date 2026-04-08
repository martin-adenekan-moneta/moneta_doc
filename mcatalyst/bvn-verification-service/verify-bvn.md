# Verify BVN

This endpoint allows you to verify and onboard a customer's BVN by checking against the Bank records. It returns comprehensive identification details about the customer.



#### **Implementation Guide**

1 Generate Authentication Token

Before making any API calls, you need to generate an authentication token using your client ID and secret. See the Generate Token endpoint for details.

2 Collect User Information

Collect the user's BVN. For higher verification accuracy, collect other personal information like name and phone number.

3 Make the API Request

Send a POST request to the endpoint with the required parameters, including the user's BVN and any additional verification fields.

4 Process the Response

The API will return comprehensive details about the BVN holder if verification is successful. Compare the returned information with the user-provided data to confirm identity.

#### Authentication Required

All API requests must include your authentication token in the header. See the Generate Token endpoint for details.



<mark style="color:green;">`POST`</mark>   `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v1/kyc/bvn/verify`

#### Request Parameters

| Parameter          | Type   | Required | Description                                                            |
| ------------------ | ------ | -------- | ---------------------------------------------------------------------- |
| `bvn`              | string | Required | 11-digit Bank Verification Number of the customer                      |
| `first_name`       | string | Optional | Customer's first name for additional verification (recommended)        |
| `last_name`        | string | Optional | Customer's last name for additional verification (recommended)         |
| `account_number`   | string | Required | Customer's Account Number for Specific Bank verification (recommended) |
| `institution_code` | string | Required | The Institution Code for the customer's Bank (recommended).            |

#### Example

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST "{{baseUrl}}/v1/kyc/verify" \
     -H "Authorization: Bearer ........................." \
     -H "Content-Type: application/json" \
     -d '{
        "bvn": "22222222222",
        "first_name": "John",
        "last_name": "Doe",
        "account_number": "00012345678",
        "bank_code": "000"
     }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = '{{baseUrl}}/v1/kyc/bvn/verify';
const token = '.........................';

fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer ' + token
  },
  body: JSON.stringify({
    bvn: '22222222222',
    first_name: 'John',
    last_name: 'Doe',
    account_number: '00012345678',
    bank_code: '000',
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```
{% endtab %}

{% tab title="PHP" %}
```php
use GuzzleHttp\Client;

$client = new Client();
$token = '.........................';
$url = $baseUrl.'/v1/kyc/verify';

$response = $client->post($url, [
    'headers' => [
        'Authorization' => 'Bearer ' . $token,
        'Accept'        => 'application/json',
    ],
    'json' => [
        'bvn'            => '22222222222',
        'first_name'     => 'John',
        'last_name'      => 'Doe',
        'account_number' => '00012345678',
        'bank_code'      => '000',
    ]
]);

$data = json_decode($response->getBody(), true);
print_r($data);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = baseUrl+"/v1/kyc/verify"
token = "........................."

headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

payload = {
    "bvn": "22222222222",
    "first_name": "John",
    "last_name": "Doe",
    "account_number": "00012345678",
    "bank_code": "000"
}

response = requests.post(url, json=payload, headers=headers)

if response.status_code == 200:
    print(response.json())
else:
    print(f"Failed with status: {response.status_code}")
    print(response.text)
```
{% endtab %}
{% endtabs %}



#### Response Format

{% tabs %}
{% tab title="200" %}
```javascript
{
  "status": true,
  "message": "Success",
  "data": {
    "account_number": "0343634941",
    "bank_verification_number": "22475613314",
    "first_name": "John",
    "last_name": "Idowu",
    "account_name": "JOHN DANIEL IDOWU",
    "bank_code": "000013",
    "bvn_match": true,
    "name_match_percentage": 100
  },
  "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```python
{
  "status": "error",
  "message": "BVN verification failed",
  "error": {
    "code": "INVALID_BVN",
    "description": "The BVN provided does not exist or is invalid"
  }
}
```
{% endtab %}
{% endtabs %}

####
