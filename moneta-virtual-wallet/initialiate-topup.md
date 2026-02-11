# Initialiate Topup

This endpoint allows you to initiate a payment transaction to top up your account, generating a payment link that can be used to complete the transaction.

#### Implementation Guide

1 Set Up Authentication

Include your client ID, client secret, and merchant token in the request headers.

2 Specify Amount

Provide the amount to be topped up in the smallest currency unit (e.g., kobo for NGN).

3 Handle Payment Link

Upon successful response, redirect the user to the payment link returned in the response data to complete the transaction.

4 Process Completion

After payment is completed, the user will be redirected back to your application with the transaction status.



<mark style="color:green;">`POST`</mark>` ``{{`[`baseUrl`](../messaging-and-push-notifications/)`}}/v2/initiate/payment`

#### Request Parameters

| Parameter | Type   | Required | Description                                                               |
| --------- | ------ | -------- | ------------------------------------------------------------------------- |
| amount    | number | Required | Amount to be topped up in the smallest currency unit (e.g., kobo for NGN) |

#### Request Headers

| Parameter        | Type   | Required | Description                            |
| ---------------- | ------ | -------- | -------------------------------------- |
| Accept           | string | Required | Must be set to 'application/json'      |
| X-Client-Id      | string | Required | Your API client ID                     |
| X-Client-Secret  | string | Required | Your API client secret                 |
| X-Merchant-Token | string | Required | Your merchant token for authentication |

#### Request Example

bashCopy

```bash
curl --location 'https://api.moneta.ng/api/v2/initiate/payment' \
--header 'Accept: application/json' \
--header 'X-Client-Id: {{client_Id}}' \
--header 'X-Client-Secret: {{client_secret}}' \
--header 'X-Merchant-Token: {{merchant_token}}' \
--data '{
  "amount": 1000
}'
```

#### Response Format

Success Response

```json
{
  "message": "Use the link to make payment",
  "data": "https://app.moneta.ng/api/v1/txn/isw/MNTAFVGRZQEDWEHYNWLD"
}
```

**Error Response**

Error Response

```json
{
  "status": "error",
  "message": "Payment initiation failed",
  "error": {
    "code": "INVALID_AMOUNT",
    "description": "The provided amount is invalid or below the minimum allowed"
  },
  "meta": {
    "request_id": "req_20250425153045_a7b3c9"
  }
}
```

####
