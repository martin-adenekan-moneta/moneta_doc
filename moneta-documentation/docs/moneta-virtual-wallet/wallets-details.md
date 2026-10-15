# Wallets Details

This endpoint returns your current wallet balance along with a detailed transaction history. Use this to track all credits and debits to your account.

#### Implementation Guide

1 Secure Authentication

Store your client credentials securely and never expose them in client-side code.

2 Parse Transaction Types

Understand the different transaction types (credit, debit) to properly track your wallet activity.

3 Implement Pagination

For accounts with many transactions, implement pagination to handle the response effectively.

4 Reconcile Transactions

Use the transaction history to reconcile with your internal records and verify all transactions.

5 Monitor Unusual Activity

Regularly check your wallet history to identify and investigate any unusual transactions.

<mark style="color:$primary;">`GET`</mark> \{{[baseUrl](./)\}}`/v2/get-wallet-history`

#### Request Headers

| Parameter       | Type   | Required | Description               |
| --------------- | ------ | -------- | ------------------------- |
| Accept          | string | Required | Set to 'application/json' |
| X-Client-Id     | string | Required | Your unique client ID     |
| X-Client-Secret | string | Required | Your client secret key    |

#### Request Example

javascript

```js
// Using fetch API
const url = 'https://api.moneta.ng/api/v2/get-wallet-history';
const clientId = 'YOUR_CLIENT_ID';
const clientSecret = 'YOUR_CLIENT_SECRET';

fetch(url, {
  method: 'GET',
  headers: {
    'Accept': 'application/json',
    'X-Client-Id': clientId,
    'X-Client-Secret': clientSecret
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
    "balance": "950.00",
    "transactions": [
      {
        "id": 1,
        "wallet_id": 2,
        "amount": "200.00",
        "description": "Wallet Topup : ",
        "transaction_type": "credit",
        "created_at": "2025-02-04T13:16:08.000000Z",
        "updated_at": "2025-02-04T13:16:08.000000Z"
      },
      {
        "id": 2,
        "wallet_id": 2,
        "amount": "100.00",
        "description": "Transaction Type : Purchase of airtime mtn N/A",
        "transaction_type": "debit",
        "created_at": "2025-02-04T13:18:03.000000Z",
        "updated_at": "2025-02-04T13:18:03.000000Z"
      },
      {
        "id": 3,
        "wallet_id": 2,
        "amount": "100.00",
        "description": "Transaction Type : (REVERSAL) Purchase of airtime mtn N/A",
        "transaction_type": "credit",
        "created_at": "2025-02-04T13:18:04.000000Z",
        "updated_at": "2025-02-04T13:18:04.000000Z"
      }
    ]
  },
  "status": "success",
  "message": "Wallet history retrieved successfully"
}
```

**Error Response**

```json
{
  "status": "error",
  "message": "Authentication failed",
  "error": {
    "code": "AUTH_FAILED",
    "description": "Invalid or expired client credentials"
  },
  "meta": {
    "request_id": "req_20250425134532_8d3e7c"
  }
}
```

####
