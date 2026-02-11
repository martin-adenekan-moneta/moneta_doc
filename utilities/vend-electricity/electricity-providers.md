# Electricity Providers

This endpoint returns a list of all available electricity providers in Nigeria. Use this to retrieve the service provider codes needed for electricity bill payments.



#### Implementation Guide

1 Retrieve Provider List

Call this endpoint at application startup or when configuring electricity payment options.

2 Cache Provider Data

Store the provider list in your application cache to avoid repeated API calls.

3 Display User-Friendly Names

Use the "name" field for display purposes and the "service\_provider" code for API requests.

4 Implement Provider Selection

Create a dropdown or selection interface allowing users to choose their electricity provider.

5 Validate Selected Provider

Before proceeding with electricity payments, verify that the selected provider is in the list.



<mark style="color:$primary;">`GET`</mark>`{{`[`baseUrl`](../)`}}/v2/services-category/electricity/providers`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |
| Accept             | string | Required | Set to 'application/json'                |

#### Request Example

javascript Copy

```javascript
// Using fetch API
const url = 'https://api.moneta.ng/api/v2/services-category/electricity/providers';
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

```
{
  "data": [
    {
      "name": "IKEDC",
      "service_provider": "ikeja-electric"
    },
    {
      "name": "EKEDC",
      "service_provider": "eko-electric"
    },
    {
      "name": "KEDCO",
      "service_provider": "kano-electric"
    },
    {
      "name": "PHED",
      "service_provider": "portharcourt-electric"
    },
    {
      "name": "JED",
      "service_provider": "jos-electric"
    },
    {
      "name": "IBEDC",
      "service_provider": "ibadan-electric"
    },
    {
      "name": "KAEDCO",
      "service_provider": "kaduna-electric"
    },
    {
      "name": "AEDC",
      "service_provider": "abuja-electric"
    },
    {
      "name": "EEDC",
      "service_provider": "enugu-electric"
    },
    {
      "name": "BEDC",
      "service_provider": "benin-electric"
    },
    {
      "name": "ABA",
      "service_provider": "aba-electric"
    },
    {
      "name": "YEDC",
      "service_provider": "yola-electric"
    }
  ],
  "status": "success",
  "message": "Utility Service Category Fetched"
}
```

**Error Response**

```
{
  "status": "error",
  "message": "Authentication failed",
  "error": {
    "code": "INVALID_CREDENTIALS",
    "description": "The provided authentication credentials are invalid"
  },
  "meta": {
    "request_id": "req_20250425124332_a7b9c3"
  }
}
```

####
