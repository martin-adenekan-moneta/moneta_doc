# BVN Information

This endpoint allows you to retrieve details of a previously verified BVN in your system. This is useful for accessing user information without initiating a new verification.

{% hint style="info" %}
#### Authentication Required

All API requests must include your authentication token in the header. See the Generate Token endpoint for details
{% endhint %}



<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](../#base-url-for-mcatalyst)`}}/v1/kyc/bvn`

#### Path Parameters

| Parameter    | Type   | Required | Description                                   |
| ------------ | ------ | -------- | --------------------------------------------- |
| `bvn_number` | string | Required | 11-digit Bank Verification Number to retrieve |

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
use GuzzleHttp\Client;
use GuzzleHttp\Exception\RequestException;

$client = new Client();

$bvnNumber = '...............';
$url = $baseUrl."/v1/kyc/bvn/" . $bvnNumber;
$token = '...................';

try {
    $response = $client->request('GET', $url, [
        'headers' => [
            'Authorization' => 'Bearer ' . $token,
            'Accept'        => 'application/json',
        ],
        'json' => [
            'bvn' => '..................'
        ]
    ]);

    $data = json_decode($response->getBody(), true);
    print_r($data);

} catch (RequestException $e) {
    echo "Error: " . $e->getMessage();
}
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

