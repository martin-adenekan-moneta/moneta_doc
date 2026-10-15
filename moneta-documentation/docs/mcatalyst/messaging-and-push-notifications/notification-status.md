# Notification Status

This endpoint allows you to verify the status of a specific notification by providing its reference code. This is useful for confirming delivery status and retrieving details about a particular notification.

#### Implementation Best Practices

1. &#x20;**Store Reference Codes**

Always store the reference code returned when sending notifications to facilitate status verification later.

2. &#x20;**Implement Status Checking**

For critical notifications, implement automated status verification to confirm delivery and trigger retries if needed.

3. &#x20;**Handle Failed Statuses**

When a notification shows a "failed" status, check the recipient details and consider alternative communication channels.



<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](./)`}}/notification/history?reference={reference}`&#x20;

#### Request Headers

| Parameter       | Type   | Required | Description                                |
| --------------- | ------ | -------- | ------------------------------------------ |
| X-Service-Token | string | Required | Your service-specific authentication token |

#### Query Parameters

| Parameter | Type   | Required | Description                                      |
| --------- | ------ | -------- | ------------------------------------------------ |
| reference | string | Required | The reference code of the notification to verify |

#### Request Example

```bash
curl --location '{{baseUrl}}/notification/history?reference=NOTIF-67e16c89a6758-744B17' \
  --header 'Accept: application/json' \
  --header 'X-Service-Token: {{service_token}}'
```

#### Response Format



```json
{
  "message": "Data retrieved successfully",
  "data": [
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
    "total": 1,
    "per_page": 10,
    "current_page": 1,
    "last_page": 1,
    "from": 1,
    "to": 1,
    "next_page_url": null,
    "prev_page_url": null
  },
  "total_amount": "5.00"
}
```

####
