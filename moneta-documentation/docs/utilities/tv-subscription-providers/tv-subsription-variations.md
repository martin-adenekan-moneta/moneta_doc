# TV Subsription Variations

This endpoint allows you to retrieve all available subscription packages for a specific TV provider. Use this to display package options and pricing to your users.



#### Implementation Guide

1 Dynamically Load Packages

Fetch subscription packages dynamically when a user selects a TV provider to ensure the most current pricing is displayed.

2 Store Variation Codes

Save the service\_variation identifier for each package as this will be required when making purchase requests.

3 Format Package Information

Present subscription package details in a user-friendly format, highlighting package name and price.

4 Handle Provider-Specific Logic

Different TV providers may have unique package structures; adapt your UI to accommodate these differences.

<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../)`}}/v2/services-category/tv-subscription/[provider]/variations`

#### URL Parameters

| Parameter | Type   | Required | Description                                                              |
| --------- | ------ | -------- | ------------------------------------------------------------------------ |
| provider  | string | Required | The service provider code (e.g., 'dstv', 'gotv', 'startimes', 'showmax') |

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| Accept             | string | Required | Set to 'application/json'                |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |

#### Request Example

javascript

```javascript
// Using fetch API
const provider = 'dstv'; // Or 'gotv', 'startimes', 'showmax'
const url = `https://api.moneta.ng/api/v2/services-category/tv-subscription/${provider}/variations`;
const serviceToken = 'YOUR_SERVICE_TOKEN';
const merchantToken = 'YOUR_MERCHANT_TOKEN';

fetch(url, {
  method: 'GET',
  headers: {
    'Accept': 'application/json',
    'X-Service-Token': serviceToken,
    'X-Merchant-Token': merchantToken,
    'X-Request-Resource': 'mobile'
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

#### Response Format

Success Response

```json
{
  "data": {
    "service_provider": "dstv",
    "service_name": "DSTV Subscription",
    "service_variations": [
      {
        "name": "DStv Padi N4,400",
        "service_variation": "dstv-padi",
        "amount": "4400.00"
      },
      {
        "name": "DStv Yanga N6,000",
        "service_variation": "dstv-yanga",
        "amount": "6000.00"
      },
      {
        "name": "Dstv Confam N11,000",
        "service_variation": "dstv-confam",
        "amount": "11000.00"
      },
      {
        "name": "DStv Compact N19,000",
        "service_variation": "dstv79",
        "amount": "19000.00"
      }
    ]
  },
  "status": "success",
  "message": "Utility Service Category Fetched"
}
```

**Error Response**

Error Response

```json
{
  "status": "error",
  "message": "Provider not found",
  "error": {
    "code": "INVALID_PROVIDER",
    "description": "The specified service provider does not exist or is not available"
  },
  "meta": {
    "request_id": "req_20250428184625_36d9f1"
  }
}
```
