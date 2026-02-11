# Data Providers Variation

This endpoint allows you to retrieve all available data plans and bundles for a specific network provider. You can use this to display plan options and pricing to your users.

#### Implementation Guide

1 Dynamically Load Data Plans

Fetch plans dynamically when a user selects a provider to ensure the most current offers are displayed.

2 Store Service Variations

Save the service\_variation identifier for each plan as this will be required when making purchase requests.

3 Display Plan Details Clearly

Present data plan details in a user-friendly format, highlighting data amount, validity period, and price.

4 Handle Network-Specific Logic

Some networks may have unique plan types or restrictions; adapt your UI to handle provider-specific details.

<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../)`}}/v2/services-category/data/[provider]/variations`

#### URL Parameters

| Parameter | Type   | Required | Description                                                          |
| --------- | ------ | -------- | -------------------------------------------------------------------- |
| provider  | string | Required | The service provider code (e.g., 'mtn', 'airtel', 'glo', 'etisalat') |

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
const provider = 'mtn'; // Or 'airtel', 'glo', 'etisalat'
const url = `https://api.moneta.ng/api/v2/services-category/data/${provider}/variations`;
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
    "service_provider": "airtel",
    "service_name": "Airtel Data",
    "service_variations": [
      {
        "name": "250MB Night Plan (12 - 5 AM) - 50 Naira  - 1Day",
        "service_variation": "airt-50",
        "amount": "50.00"
      },
      {
        "name": "200MB Social Plan (2 Days) - 100 Naira - 1Day",
        "service_variation": "airt-100",
        "amount": "100.00"
      },
      {
        "name": "200MB Daily Plan (2 Days) - 200 Naira - 200MB - 1Day",
        "service_variation": "airt-200",
        "amount": "200.00"
      },
      {
        "name": "Airtel Data - 100 Naira - 100MB - 1 Day",
        "service_variation": "airt-daily-100",
        "amount": "100.00"
      },
      {
        "name": "500MB Weekly Plan (7 Days) - 500 Naira",
        "service_variation": "airt-500",
        "amount": "500.00"
      }
    ]
  },
  "status": "success",
  "message": "Variations fetched successfully"
}
```

**Error Response**

```json
{
  "status": "error",
  "message": "Provider not found",
  "error": {
    "code": "INVALID_PROVIDER",
    "description": "The specified service provider does not exist or is not available"
  },
  "meta": {
    "request_id": "req_20250428172345_72e4b9"
  }
}
```
