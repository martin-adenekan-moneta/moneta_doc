# Airtime Providers

This endpoint allows you to retrieve a list of all available airtime service providers, including local Nigerian networks and international airtime options.

#### Implementation Guide

1 Cache Provider Information

Store airtime provider information locally as it changes infrequently, reducing unnecessary API calls.

2 Use Provider Codes

Save the service\_provider codes as they will be required when making airtime purchase requests.

3 Handle Special Cases

Note that international airtime ('foreign-airtime') may require additional parameters when making transactions.

4 Error Handling

Implement appropriate error handling for authentication issues and network failures.

<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../)`}}/v2/services-category/airtime/providers`

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
const url = 'https://api.moneta.ng/api/v2/services-category/airtime/providers';
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
      "name": "Airtel Airtime VTU",
      "service_provider": "airtel"
    },
    {
      "name": "MTN Airtime VTU",
      "service_provider": "mtn"
    },
    {
      "name": "GLO Airtime VTU",
      "service_provider": "glo"
    },
    {
      "name": "9mobile Airtime VTU",
      "service_provider": "etisalat"
    },
    {
      "name": "International Airtime",
      "service_provider": "foreign-airtime"
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
    "request_id": "req_20250428180514_63a1f8"
  }
}
```
