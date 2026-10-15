# Vend Data

This endpoint allows you to purchase data bundles for any mobile network in Nigeria. Use this to top up data for your customers or for personal use.

#### Implementation Guide

1 Verify Data Plan Before Purchase

Use the variations endpoint to confirm the current data plan codes and prices before initiating a purchase.

2 Validate Phone Numbers

Ensure phone numbers are in the correct format and belong to the specified network to prevent failed transactions.

3 Handle Transaction States

Implement logic to handle different transaction statuses (pending, completed, failed) and provide appropriate feedback.

4 Store Transaction References

Save the returned reference for reconciliation and to check transaction status later if needed.

5 Implement Retry Logic

For transactions with 'failed' status, implement a sensible retry mechanism with appropriate delays.

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/v2/initiate`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |

#### Request Parameters

| Parameter          | Type    | Required | Description                                                                          |
| ------------------ | ------- | -------- | ------------------------------------------------------------------------------------ |
| service\_category  | string  | Required | Must be set to 'data' for data bundle purchases                                      |
| service\_provider  | string  | Required | Network provider code (e.g., 'airtel-data', 'mtn-data', 'glo-data', 'etisalat-data') |
| service\_variation | string  | Required | The specific data plan code to purchase (retrieved from the variations endpoint)     |
| phone              | string  | Required | Recipient's phone number in local format (e.g., '08012345678')                       |
| email              | string  | Required | Email address to receive transaction notifications                                   |
| is\_pos\_agent     | boolean | Optional | Flag to indicate if the transaction is being performed by a POS agent                |

#### Request Example

javascript

```js
// Using fetch API
      const url = 'https://api.moneta.ng/api/v2/initiate';
      const serviceToken = 'YOUR_SERVICE_TOKEN';
      const merchantToken = 'YOUR_MERCHANT_TOKEN';
      
      fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-Service-Token': serviceToken,
          'X-Merchant-Token': merchantToken,
          'X-Request-Resource': 'mobile'
        },
        body: JSON.stringify({
          service_category: 'data',
          service_provider: 'airtel-data',
          service_variation: 'airt-1000-7',
          phone: '08012345678',
          email: 'customer@example.com',
          is_pos_agent: false
        })
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
    "id": 114,
    "merchant_id": 1,
    "vendor_slug": "vt-pass",
    "category": "data",
    "provider": "airtel-data",
    "amount": "1000.00",
    "profit_margin": "0.00",
    "profit_amount": "0.00",
    "receiver_id": "08012345678",
    "status": "pending",
    "email": "customer@example.com",
    "phone_number": "08012345678",
    "reference": "2025042501436Usr0qrVBM",
    "date_completed": null,
    "request_payload": null,
    "response_payload": null,
    "vendor_transaction_reference": null,
    "access_token": null,
    "variation": "airt-1000-7",
    "value_code": null,
    "units": null,
    "authorization_url": null,
    "moneta_auth_token": null,
    "created_at": "2025-04-25T12:43:32.000000Z",
    "updated_at": "2025-04-25T12:43:33.000000Z"
  },
  "status": "success",
  "message": "Transaction initiated successfully"
}
```

**Error Response**

Error Response

```json
{
  "status": "error",
  "message": "Insufficient wallet balance",
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "description": "Your account balance is insufficient to complete this transaction"
  },
  "meta": {
    "request_id": "req_20250425124332_9f3e7c"
  }
}
```
