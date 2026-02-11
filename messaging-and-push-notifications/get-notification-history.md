# Get Notification History

This endpoint allows you to retrieve the history of notifications sent through your account. You can view the status, recipient details, and costs of all your previous notifications.

#### Implementation Notes

* The response is paginated with 10 records per page by default.
* The `total_amount` field shows the total cost of all notifications in the current result set.
* The `status` field can be either " sent" or "failed" to indicate delivery status.



<mark style="color:$primary;">`GET`</mark> \{{[baseUrl](./)\}}`/v2/notification/history`

#### Request Headers

| Parameter        | Type   | Required | Description                            |
| ---------------- | ------ | -------- | -------------------------------------- |
| Accept           | string | Required | Must be set to 'application/json'      |
| X-Client-Id      | string | Required | Your API client ID                     |
| X-Client-Secret  | string | Required | Your API client secret                 |
| X-Merchant-Token | string | Required | Your merchant token for authentication |

#### Optional Query Parameters

| Parameter | Type    | Required | Description                                       |
| --------- | ------- | -------- | ------------------------------------------------- |
| page      | integer | Optional | Page number for paginated results                 |
| reference | string  | Optional | Filter history by specific notification reference |

#### Request Example

bashCopy

```bash
curl --location 'https://api.moneta.ng/api/v2/notification/history' \
  --header 'Accept: application/json' \
  --header 'X-Client-Id: {{client_Id}}' \
  --header 'X-Client-Secret: {{client_secret}}' \
  --header 'X-Merchant-Token: {{merchant_token}}'
```

#### Response Format

Success Response

```json
{
  "message": "Data retrieved successfully",
  "data": [
    {
      "title": "Welcome to Our Platform",
      "notification_type": "sms",
      "amount": "5.00",
      "to": "07065216112",
      "status": "sent",
      "reference": "NOTIF-67e16ca375620-79A4A4",
      "date": "2025-03-24 14:30:59"
    },
    {
      "title": "Welcome to Our Platform",
      "notification_type": "sms",
      "amount": "5.00",
      "to": "+2347065216112",
      "status": "sent",
      "reference": "NOTIF-67e16c89a6758-744B17",
      "date": "2025-03-24 14:30:33"
    }
  ],
  "meta": {
    "total": 70,
    "per_page": 10,
    "current_page": 1,
    "last_page": 7,
    "from": 1,
    "to": 10,
    "next_page_url": "http://127.0.0.1:8000/api/notification/history?page=2",
    "prev_page_url": null
  },
  "total_amount": "206.00"
}
```
