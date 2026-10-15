# Push Notification

### Send Push Notification

This endpoint allows you to send push notifications & messages to your users for broadcasting of updates.&#x20;

#### Implementation Guide

1. &#x20;Choose Notification Channel

Select the appropriate notification\_type based on your communication needs and user preferences.

2. &#x20;Prepare Content

Format the notification body according to the chosen channel's requirements.

3. &#x20;Get Device Token

Add the appropriate recipient identifiers to the user\_details array based on the notification type.

4 Send and Track

Store the reference code from the response for tracking and potential troubleshooting.



<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](./)`}}/notification`

#### Request Headers

| Parameter       | Type   | Required | Description                                |
| --------------- | ------ | -------- | ------------------------------------------ |
| Accept          | string | Required | Must be set to 'application/json'          |
| X-Service-Token | string | Required | Your service-specific authentication token |

#### Request Parameters for WhatsApp

| Parameter           | Type    | Required | Description                                                                          |
| ------------------- | ------- | -------- | ------------------------------------------------------------------------------------ |
| title               | string  | Required | Title of the WhatsApp notification                                                   |
| body.content        | string  | Required | Text content of the WhatsApp message                                                 |
| body.otp            | string  | Optional | OTP code when using the 'otp' template                                               |
| notification\_type  | string  | Required | Must be set to 'whatsapp'                                                            |
| template            | string  | Required | Template type to use (messaging, otp)                                                |
| notification\_event | string  | Required | Event category for analytics and tracking purposes                                   |
| user\_details       | array   | Required | Array of recipient phone numbers without the '+' symbol (e.g., '2348012345678')      |
| sensitive           | boolean | Optional | Flag to indicate whether the content contains sensitive information (default: false) |

#### Request Example

bash

```bash
curl --location 'https://api.moneta.ng/api/v2/notification' \
        --header 'Accept: application/json' \
        --header 'X-Client-Id: {{client_Id}}' \
        --header 'X-Client-Secret: {{client_secret}}' \
        --header 'X-Merchant-Token: {{merchant_token}}' \
        --header 'X-Service-Token: {{service_token}}' \
        --data-raw '{
            "title": "Payment Confirmation",
            "body": {
                "content": "Thank you for your payment of NGN 10,000. Your transaction has been processed successfully."
            },
            "notification_type": "whatsapp",
            "template": "messaging",
            "notification_event": "payment_confirmation",
            "user_details": [
                "2348012345678"
            ],
            "sensitive": false
        }'
```

#### OTP Message Example

bash

```bash
curl --location 'https://api.moneta.ng/api/v2/notification' \
        --header 'Accept: application/json' \
        --header 'X-Client-Id: {{client_Id}}' \
        --header 'X-Client-Secret: {{client_secret}}' \
        --header 'X-Merchant-Token: {{merchant_token}}' \
        --header 'X-Service-Token: {{service_token}}' \
        --data-raw '{
            "title": "Security Verification",
            "body": {
                "content": "Your one-time verification code is: ",
                "otp": "123456"
            },
            "notification_type": "whatsapp",
            "template": "otp",
            "notification_event": "account_verification",
            "user_details": [
                "2348012345678"
            ],
            "sensitive": true
        }'
```

#### Response Format

Success Response

```json
{
  "message": "Successfully queued 1 notifications",
  "data": {
    "cost": 5,
    "reference": "NOTIF-67e2a93e9ab38-561181"
  }
}
```

**Error Response**



```json
{
  "status": "error",
  "message": "Failed to queue WhatsApp notification",
  "error": {
    "code": "INVALID_PHONE_NUMBER",
    "description": "One or more phone numbers in user_details are in an invalid format"
  },
  "meta": {
    "request_id": "req_20250425174231_q5r6s7"
  }
}
```

####
