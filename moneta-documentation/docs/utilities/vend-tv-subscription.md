# Vend TV Subscription

This endpoint allows you to purchase or renew TV subscriptions for various providers including DSTV, GOTV, Startimes, and ShowMax.

#### Implementation Guide

1 Validate Smart Card Number

Implement validation for smart card/IUC numbers to ensure they are in the correct format before submitting.

2 Use Current Package Codes

Always fetch the latest subscription packages from the variations endpoint to ensure correct pricing and package codes.

3 Implement Customer Verification

Consider adding a customer verification step where possible to confirm subscriber details before processing payment.

4 Store Transaction Information

Save transaction references and details for customer support inquiries and reconciliation purposes.

5 Provide Clear Success Messages

Display detailed success information to users, including the package name, smart card number, and activation status.

<mark style="color:green;">`POST`</mark> \{{baseUrl\}}`/v2/initiate`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| Accept             | string | Required | Set to 'application/json'                |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |

#### Request Parameters

| Parameter          | Type    | Required | Description                                                                     |
| ------------------ | ------- | -------- | ------------------------------------------------------------------------------- |
| service\_category  | string  | Required | Must be set to 'tv-subscription' for TV subscription purchases                  |
| service\_provider  | string  | Required | TV provider code (e.g., 'dstv', 'gotv', 'startimes', 'showmax')                 |
| service\_variation | string  | Required | The specific subscription package code (retrieved from the variations endpoint) |
| billerCode         | string  | Required | The smart card number or IUC number of the TV subscription                      |
| phone              | string  | Required | Customer's phone number in local format (e.g., '08012345678')                   |
| email              | string  | Required | Email address to receive transaction notifications                              |
| is\_pos\_agent     | boolean | Optional | Flag to indicate if the transaction is being performed by a POS agent           |

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
    'Accept': 'application/json',
    'Content-Type': 'application/json',
    'X-Service-Token': serviceToken,
    'X-Merchant-Token': merchantToken,
    'X-Request-Resource': 'mobile'
  },
  body: JSON.stringify({
    service_category: 'tv-subscription',
    service_provider: 'dstv',
    service_variation: 'dstv-padi',
    billerCode: '1234567890',
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
    "id": 103,
    "merchant_id": 1,
    "vendor_slug": "vt-pass",
    "category": "tv-subscription",
    "provider": "dstv",
    "amount": "4400.00",
    "receiver_id": "1234567890",
    "status": "success",
    "email": "customer@example.com",
    "phone_number": "08012345678",
    "reference": "2025042818462mLt871hTB",
    "date_completed": "2025-04-28 18:46:50",
    "request_payload": "{\"phone\": \"08012345678\", \"amount\": \"4400.00\", \"quantity\": 1, \"serviceID\": \"dstv\", \"request_id\": \"2025042818462mLt871hTB\", \"billersCode\": \"1234567890\", \"variation_code\": \"dstv-padi\", \"subscription_type\": \"change\"}",
    "response_payload": "{\"name\": null, \"type\": \"TV Subscription\", \"email\": \"info@moneta.ng\", \"phone\": \"07062057116\", \"amount\": \"4400.00\", \"method\": \"api\", \"status\": \"delivered\", \"channel\": \"api\", \"discount\": null, \"platform\": \"api\", \"quantity\": 1, \"commission\": 66, \"unit_price\": \"4400.00\", \"product_name\": \"DSTV Subscription\", \"total_amount\": 4334, \"transactionId\": \"17426982460561704770569815\", \"unique_element\": \"1234567890\", \"convinience_fee\": \"0.00\", \"commission_details\": {\"rate\": \"1.50\", \"amount\": 66, \"rate_type\": \"percent\", \"computation_type\": \"default\"}, \"service_verification\": null}",
    "vendor_transaction_reference": null,
    "access_token": "17426982460561704770569815",
    "variation": "dstv-padi",
    "value_code": "",
    "units": null,
    "authorization_url": null,
    "moneta_auth_token": null,
    "created_at": "2025-04-28T18:46:41.000000Z",
    "updated_at": "2025-04-28T18:46:50.000000Z"
  },
  "status": "success",
  "message": "Transaction Successfully"
}
```

**Error Response**

```json
{
  "status": "error",
  "message": "Invalid smart card number",
  "error": {
    "code": "INVALID_BILLER_CODE",
    "description": "The provided smart card number is incorrect or not found"
  },
  "meta": {
    "request_id": "req_20250428184715_9c2d8e"
  }
}
```
