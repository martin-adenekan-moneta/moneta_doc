# Data Providers

This endpoint allows you to retrieve a list of all available data service providers in Nigeria, including mobile networks and internet service providers.

#### Implementation Guide

1 Authentication Setup

Ensure your service and merchant tokens are properly configured in your application.

2 Cache Provider Data

Consider caching the provider list as it changes infrequently, reducing API calls and improving performance.

3 Error Handling

Implement proper error handling for authentication issues and network failures.



<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../)`}}/v2/services-category/data/providers`

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
const url = 'https://api.moneta.ng/api/v2/services-category/data/providers';
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
      "name": "Airtel Data",
      "service_provider": "airtel-data"
    },
    {
      "name": "MTN Data",
      "service_provider": "mtn-data"
    },
    {
      "name": "GLO Data",
      "service_provider": "glo-data"
    },
    {
      "name": "9mobile Data",
      "service_provider": "etisalat-data"
    },
    {
      "name": "Smile Payment",
      "service_provider": "smile-direct"
    },
    {
      "name": "Spectranet Internet Data",
      "service_provider": "spectranet"
    },
    {
      "name": "GLO Data (SME)",
      "service_provider": "glo-sme-data"
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
    "request_id": "req_20250428172045_98d2f1"
  }
}
```
