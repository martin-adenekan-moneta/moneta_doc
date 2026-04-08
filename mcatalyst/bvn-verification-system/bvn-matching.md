# BVN Matching

This advanced endpoint allows you to query specific attributes of a BVN record to match against provided values. This is useful for validating specific customer details.

{% hint style="info" %}
#### Usage Limits

This endpoint has higher rate limits and usage costs. Please refer to your pricing plan for details.
{% endhint %}



<mark style="color:green;">`POST`</mark>  `{{baseUrl}}/v1/kyc/bvn/query`

#### Request Parameters

| Parameter    | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| `bvn`        | string | Required | 11-digit Bank Verification Number of the customer            |
| `query_type` | string | Required | Type of query: 'MATCH\_ALL', 'MATCH\_ANY', or 'CUSTOM'       |
| `attributes` | object | Required | Object containing attributes to match against the BVN record |

#### Request Example



```js
// Using fetch API
const url = 'https://api.moneta.ng/api/v1/kyc/bvn/query';
const token = 'YOUR_AUTH_TOKEN';

fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer ' + token
  },
  body: JSON.stringify({
    bvn: '..........',
    query_type: 'MATCH_ALL',
    attributes: {
      first_name: 'John',
      last_name: 'Doe',
      date_of_birth: '1990-01-01',
      phone_number: '08012345678'
    }
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

#### Response Example

Success Response

<pre class="language-json"><code class="lang-json">{
  "status": "success",
  "message": "BVN query completed",
  "data": {
    "bvn": "..........",
    "match_results": {
      "first_name": true,
      "last_name": true,
      "date_of_birth": true,
      "phone_number": true
<strong>    },
</strong>    "match_score": 100,
    "verification_status": "VERIFIED"
  },
  "meta": {
    "request_id": "..........",
    "transaction_ref": "............"
  }
}
</code></pre>
