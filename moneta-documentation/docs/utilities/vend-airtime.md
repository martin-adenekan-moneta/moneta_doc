# Vend Airtime

This endpoint allows you to purchase airtime for any mobile network in Nigeria. You can use this to top up airtime for your customers or for personal use.

#### Implementation Guide

1 Validate Phone Numbers

Ensure phone numbers are in the correct format and belong to the specified network to prevent failed transactions.

2 Implement Amount Constraints

Apply minimum (50 Naira) and maximum (10,000 Naira) amount validations in your application before making API calls.

3 Save Transaction References

Store the returned reference for reconciliation and to handle customer support queries.

4 Implement Retry Logic

For transactions with network or system errors, implement a reasonable retry strategy with appropriate delays.

5 Display Success Messages

Show clear success notifications to users, including the amount, recipient number, and network provider.

<mark style="color:green;">`POST`</mark> \{{baseUrl\}}`/v2/initiate`

#### Request Headers

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| Accept             | string | Required | Set to 'application/json'                |
| X-Service-Token    | string | Required | Your service authentication token        |
| X-Merchant-Token   | string | Required | Your merchant authentication token       |
| X-Request-Resource | string | Required | Resource type, typically set to 'mobile' |

#### Request Parameters

| Parameter          | Type    | Required | Description                                                                         |
| ------------------ | ------- | -------- | ----------------------------------------------------------------------------------- |
| service\_category  | string  | Required | Must be set to 'airtime' for airtime purchases                                      |
| service\_provider  | string  | Required | Network provider code (e.g., 'mtn', 'airtel', 'glo', 'etisalat', 'foreign-airtime') |
| service\_variation | string  | Optional | Leave empty for standard airtime purchases                                          |
| amount             | number  | Required | Amount of airtime to purchase in Naira (minimum: 50, maximum: 10,000)               |
| phone              | string  | Required | Recipient's phone number in local format (e.g., '08012345678')                      |
| email              | string  | Required | Email address to receive transaction notifications                                  |
| is\_pos\_agent     | boolean | Optional | Flag to indicate if the transaction is being performed by a POS agent               |

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
    service_category: 'airtime',
    service_provider: 'mtn',
    service_variation: '',
    amount: 100,
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
    "id": 65,
    "client_id": 2,
    "vendor_slug": "i-recharge",
    "category": "airtime",
    "provider": "mtn",
    "amount": "100.00",
    "receiver_id": "08012345678",
    "status": "success",
    "email": "customer@example.com",
    "phone_number": "08012345678",
    "reference": "147134181245",
    "date_completed": "2025-04-28 18:12:38",
    "request_payload": null,
    "response_payload": "{\"ref\": \"250428357330\", \"order\": \"API MTN N100 to 08012345678. \", \"amount\": 100, \"status\": \"00\", \"message\": \"Successful\", \"receiver\": \"080123**678\", \"response_hash\": \"8a6570f734eb4f5f3eea8acab2df10a044718b6e\", \"wallet_balance\": 509}",
    "vendor_transaction_reference": null,
    "access_token": "",
    "variation": "N/A",
    "value_code": null,
    "units": null,
    "authorization_url": null,
    "moneta_auth_token": null,
    "created_at": "2025-04-28T18:12:35.000000Z",
    "updated_at": "2025-04-28T18:12:38.000000Z"
  },
  "status": "success",
  "message": "Transaction successful"
}
```

**Error Response**



```json
{
  "status": "error",
  "message": "Transaction failed",
  "error": {
    "code": "INSUFFICIENT_BALANCE",
    "description": "Your wallet balance is insufficient to complete this transaction"
  },
  "meta": {
    "request_id": "req_20250428181238_5f2e9d"
  }
}
```
