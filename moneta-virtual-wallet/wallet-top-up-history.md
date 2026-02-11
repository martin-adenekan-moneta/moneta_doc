# Wallet Top-up History

This endpoint allows you to retrieve a paginated list of all your wallet top-up transactions, including amount, status, and balance after each transaction.

#### Implementation Guide

1 Set Up Authentication

Include all required authentication headers in your request.

2 Handle Pagination

Implement pagination controls in your UI using the meta information provided in the response.

3 Display Transactions

Format and display transaction amounts and dates in a user-friendly manner.

4 Status Indicators

Use visual indicators (colors, icons) to represent different transaction statuses for better user experience.



<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../messaging-and-push-notifications/)`}}/v2/wallet/top-up-history`

#### Request Headers

| Parameter        | Type   | Required | Description                                |
| ---------------- | ------ | -------- | ------------------------------------------ |
| Accept           | string | Required | Must be set to 'application/json'          |
| X-Client-Id      | string | Required | Your API client ID                         |
| X-Client-Secret  | string | Required | Your API client secret                     |
| X-Merchant-Token | string | Required | Your merchant token for authentication     |
| X-Service-Token  | string | Required | Your service-specific authentication token |

#### Query Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| page      | number | Optional | Page number for pagination (default: 1)            |
| per\_page | number | Optional | Number of records per page (default: 10, max: 100) |

#### Request Example

bashCopy

```bash
curl --location 'https://api.moneta.ng/api/v2/wallet/top-up-history' \
--header 'Accept: application/json' \
--header 'X-Client-Id: {{client_Id}}' \
--header 'X-Client-Secret: {{client_secret}}' \
--header 'X-Merchant-Token: {{merchant_token}}' \
--header 'X-Service-Token: {{service_token}}'
```

#### Response Format

Success Response

```json
{
  "message": "Data retrieved successfully",
  "data": [
    {
      "id": 1,
      "merchant_id": 2,
      "transaction_ref": "MNTALUUCEXCZ0RLGEDVQ",
      "amount": "10.00",
      "wallet_balance": "1915.00",
      "status": "success",
      "deleted_at": null,
      "created_at": "2025-03-25T12:56:47.000000Z",
      "updated_at": "2025-03-25T12:56:47.000000Z"
    }
  ],
  "meta": {
    "total": 1,
    "per_page": 10,
    "current_page": 1,
    "last_page": 1,
    "from": 1,
    "to": 1,
    "next_page_url": null,
    "prev_page_url": null
  }
}
```

**Error Response**



```json
{
  "status": "error",
  "message": "Failed to retrieve top-up history",
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "description": "Invalid or expired authentication credentials"
  },
  "meta": {
    "request_id": "req_20250425160721_f5g7h9"
  }
}
```

#### Response Properties

| Parameter        | Type   | Required | Description                                              |
| ---------------- | ------ | -------- | -------------------------------------------------------- |
| id               | number | Required | Unique identifier of the top-up transaction              |
| merchant\_id     | number | Required | Identifier of the merchant who performed the top-up      |
| transaction\_ref | string | Required | Unique reference code for the transaction                |
| amount           | string | Required | Amount topped up in the main currency unit (e.g., naira) |
| wallet\_balance  | string | Required | Wallet balance after the top-up transaction              |
| status           | string | Required | Status of the transaction (success, pending, failed)     |
| created\_at      | string | Required | ISO 8601 datetime when the transaction was created       |
| updated\_at      | string | Required | ISO 8601 datetime when the transaction was last updated  |

#### Pagination

The response includes a `meta` object with pagination information:

| Parameter       | Type         | Required | Description                                                             |
| --------------- | ------------ | -------- | ----------------------------------------------------------------------- |
| total           | number       | Required | Total number of records available                                       |
| per\_page       | number       | Required | Number of records per page                                              |
| current\_page   | number       | Required | Current page number                                                     |
| last\_page      | number       | Required | Total number of pages available                                         |
| from            | number       | Required | Index of the first record on the current page                           |
| to              | number       | Required | Index of the last record on the current page                            |
| next\_page\_url | string\|null | Required | URL to fetch the next page of results, or null if on the last page      |
| prev\_page\_url | string\|null | Required | URL to fetch the previous page of results, or null if on the first page |

####
