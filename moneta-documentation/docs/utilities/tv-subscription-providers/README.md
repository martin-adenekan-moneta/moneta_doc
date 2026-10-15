# TV Subscription Providers

This endpoint allows you to retrieve a list of all available TV subscription service providers supported by the Moneta API.



#### Implementation Guide

1\. Cache Provider Information

Store TV subscription provider information locally as it changes infrequently, reducing unnecessary API calls.

2\. Use Provider Codes

Save the service\_provider codes as they will be required when fetching subscription packages and making purchase requests.

3\. Error Handling

Implement appropriate error handling for authentication issues and network failures.

<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../)`}}/v2/services-category/tv-subscription/providers`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| Accept             | string | Required | Set to 'application/json'                |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |

#### Request Example

javascript

```js
// Using fetch API
const url = '{{baseUrl}}/tv-subscription/providers';
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
  "data": [
    {
      "name": "DSTV Subscription",
      "service_provider": "dstv"
    },
    {
      "name": "Gotv Payment",
      "service_provider": "gotv"
    },
    {
      "name": "Startimes Subscription",
      "service_provider": "startimes"
    },
    {
      "name": "ShowMax",
      "service_provider": "showmax"
    }
  ],
  "status": "success",
  "message": "Utility Service Category Fetched"
}
```

**Error Response**

```json
{
  "status": "error",
  "message": "Authentication failed",
  "error": {
    "code": "INVALID_CREDENTIALS",
    "description": "The provided authentication tokens are invalid or expired"
  },
  "meta": {
    "request_id": "req_20250428184512_7b8d3c"
  }
}
```
