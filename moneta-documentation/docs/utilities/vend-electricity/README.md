# Vend Electricity

This endpoint allows you to purchase electricity tokens or pay electricity bills for any supported provider in Nigeria. Use this to top up prepaid meters or pay postpaid bills.



#### Implementation Guide

1 Validate Meter Numbers

Use the meter validation endpoint to verify meter numbers before making payments.

2 Determine Meter Type

Ensure you're using the correct service\_variation ('prepaid' or 'postpaid') for the meter.

3 Display Token Information

For prepaid transactions, prominently display the meter token and units purchased to the customer.

4 Handle Transaction States

Implement logic to handle different transaction statuses (pending, completed, failed) and provide appropriate feedback.

5 Store Transaction References

Save the returned reference and value\_code (token) for customer support and reconciliation purposes.

<mark style="color:green;">`POST`</mark>`{{`[`baseUrl`](../)`}}/v2/initiate`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |
| Accept             | string | Required | Set to 'application/json'                |

#### Request Parameters

| Parameter          | Type    | Required | Description                                                           |
| ------------------ | ------- | -------- | --------------------------------------------------------------------- |
| service\_category  | string  | Required | Must be set to 'electricity' for electricity payments                 |
| service\_provider  | string  | Required | Electricity provider code (e.g., 'abuja-electric', 'ikeja-electric')  |
| service\_variation | string  | Required | Payment type, either 'prepaid' or 'postpaid'                          |
| meter\_number      | string  | Required | The customer's meter number                                           |
| amount             | number  | Required | Amount to purchase in Naira (minimum 500)                             |
| phone              | string  | Required | Customer's phone number in local format (e.g., '08012345678')         |
| email              | string  | Required | Email address to receive transaction notifications                    |
| number\_of\_months | number  | Optional | Number of months to pay for (only for postpaid bills)                 |
| is\_pos\_agent     | boolean | Optional | Flag to indicate if the transaction is being performed by a POS agent |

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
    'Accept': 'application/json',
    'X-Service-Token': serviceToken,
    'X-Merchant-Token': merchantToken,
    'X-Request-Resource': 'mobile'
  },
  body: JSON.stringify({
    service_category: 'electricity',
    service_provider: 'abuja-electric',
    service_variation: 'prepaid',
    meter_number: '0137217065093',
    amount: 900,
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
    "id": 69,
    "client_id": 2,
    "vendor_slug": "i-recharge",
    "category": "electricity",
    "provider": "AEDC",
    "amount": "900.00",
    "receiver_id": "0137217065093",
    "status": "success",
    "email": "customer@example.com",
    "phone_number": "08012345678",
    "reference": "213410940317",
    "date_completed": "2025-02-06 11:10:51",
    "request_payload": null,
    "response_payload": "{\"ref\": \"250206617391\", \"tax\": null, \"kct1\": null, \"kct2\": null, \"units\": \"13.2 Kwh\", \"amount\": 900, \"status\": \"00\", \"tariff\": null, \"address\": \"&dq;PLOT 6126 AFTER RAW MATERIAL QTRS,  ABUJA, , \", \"message\": \"Successfull\", \"disco_ref\": null, \"debt_amount\": 0, \"meter_token\": \"50333870742767213902\", \"token_amount\": null, \"response_hash\": \"4f2966645d631b2ee73344c8673712f7bd924303\", \"wallet_balance\": 4732}",
    "vendor_transaction_reference": null,
    "access_token": "250206617391",
    "variation": "prepaid",
    "value_code": "50333870742767213902",
    "units": "13.2 Kwh",
    "authorization_url": null,
    "moneta_auth_token": null,
    "created_at": "2025-02-06T11:10:41.000000Z",
    "updated_at": "2025-02-06T11:10:51.000000Z"
  },
  "status": "success",
  "message": "Transaction successful"
}
```

**Error Response**

```json
{
  "status": "error",
  "message": "Invalid meter number",
  "error": {
    "code": "INVALID_METER",
    "description": "The provided meter number is invalid or not found"
  },
  "meta": {
    "request_id": "req_20250425124332_5d8e9f"
  }
}
```
