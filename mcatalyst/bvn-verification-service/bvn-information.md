# BVN Information

This endpoint allows you to retrieve details of a previously verified BVN in your system. This is useful for accessing user information without initiating a new verification.

{% hint style="info" %}
#### Authentication Required

All API requests must include your authentication token in the header. See the Generate Token endpoint for details
{% endhint %}



<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v1/kyc/bvn`

#### Path Parameters

| Parameter    | Type   | Required | Description                                   |
| ------------ | ------ | -------- | --------------------------------------------- |
| `bvn_number` | string | Required | 11-digit Bank Verification Number to retrieve |

#### Request Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bvnNumber = '...............';
const url = `https://api.moneta.ng/api/v1/kyc/bvn/`+bvnNumber;
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

{% tab title="Python" %}
```python
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
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

