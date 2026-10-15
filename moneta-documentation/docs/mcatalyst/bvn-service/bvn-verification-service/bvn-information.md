# BVN Information

This endpoint allows you to retrieve details of a previously verified BVN in your system. This is useful for accessing user information without initiating a new verification.

{% hint style="info" %}
#### Authentication Required

All API requests must include your authentication token in the header. See the Generate Token endpoint for details
{% endhint %}



<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../../#base-url-for-mcatalyst)`}}/v1/kyc/bvn`

#### Path Parameters

<table><thead><tr><th width="156">Parameter</th><th width="84">Type</th><th width="117">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>scope</code></td><td>string</td><td>required</td><td>The scope of the bvn validation. This can be either "accounts" TO RETRIEVE ACCOUNTS or "profile" TO RETRIEVE PROFILE</td></tr><tr><td>bvn</td><td>int</td><td>required</td><td>The BVN of the user. Example: <code>22448484848</code></td></tr><tr><td>channel_code</td><td>string</td><td>required</td><td>The channel/medium used in Processing the BVN. This can be any of the following: mobile_app, customer_web_portal, internal_web_portal, atm, pos, other. Example: <code>internal_web_portal</code></td></tr><tr><td>otp_method</td><td>string</td><td>required</td><td>The otp method to be used. This is to Specify Phone Number that must be tied to an Account Under the BVN that the Customer Can Use to Receive the OTP. Example: <code>08012345678</code></td></tr></tbody></table>

#### Request Example

{% tabs %}
{% tab title="Curl" %}
```bash
curl --location --request GET '{{baseUrl}}/v1/kyc/bvn/...............' \
--header 'Authorization: Bearer ...................' \
--header 'Content-Type: application/json' \
--data-raw '{
    "bvn": ".................."
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const bvnNumber = '...............';
const url = baseUrl+`/v1/kyc/bvn/`+bvnNumber;
const token = '...................';

fetch(url, {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer ' + token
  }
     body: JSON.stringify({
    bvn: '..................'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```
{% endtab %}

{% tab title="PHP" %}
```php

$client = new Client();
$headers = [
  'X-Service-Token' => ''
];
$body = '{
  "scope": "profile",
  "channel_code": "internal_web_portal",
  "bvn": "22302148176",
  "otp_method": "08133667142"
}';
$request = new Request('POST', 'https://api.moneta.ng/api/v2/bvn/query', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

bvn_number = '...............'
base_url = 'https://api.example.com'
url = f"{base_url}/v1/kyc/bvn/{bvn_number}"
token = '...................'

# Request headers
headers = {
    'Authorization': f'Bearer {token}',
    'Content-Type': 'application/json'
}

# Request body
payload = {
    'bvn': '..................'
}

try:
    # Most APIs don't use a body for GET, but if yours does:
    response = requests.get(url, headers=headers, json=payload)
    
    # Raise an exception for 4XX or 5XX status codes
    response.raise_for_status()
    
    # Parse JSON response
    data = response.json()
    print(data)

except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```
{% endtab %}
{% endtabs %}

#### Response Format

The response format is identical to the one from the Verify BVN endpoint.

{% tabs %}
{% tab title="200" %}
```json
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
```
{% endtab %}
{% endtabs %}

