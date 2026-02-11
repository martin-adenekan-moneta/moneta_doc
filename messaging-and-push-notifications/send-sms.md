# Send SMS

This endpoint allows you to send SMS messages to your users with customizable content. SMS notifications are useful for time-sensitive alerts, verification codes, and important updates.

#### Implementation Best Practices

1 Format Phone Numbers Correctly

Always use international format for phone numbers, removing any ' '+' prefix (e.g., '2348012345678' instead of '+2348012345678').

2 Keep Messages Concise

SMS messages exceeding 160 characters will be split into multiple units and incur additional costs. Keep messages brief and focused.

3 Include Sender Information

When sending transactional or informational messages, include your company name at the beginning to prevent messages from being marked as spam.

4 Handle Delivery Status

Store the reference code from the response to track the status of the notification in your system.

<mark style="color:green;">`POST`</mark> \{{[baseUrl](./)\}}`/v2/notification`

#### Request Headers

| Parameter        | Type   | Required | Description                                |
| ---------------- | ------ | -------- | ------------------------------------------ |
| Accept           | string | Required | Must be set to 'application/json'          |
| X-Client-Id      | string | Required | Your API client ID                         |
| X-Client-Secret  | string | Required | Your API client secret                     |
| X-Merchant-Token | string | Required | Your merchant token for authentication     |
| X-Service-Token  | string | Required | Your service-specific authentication token |

#### Request Parameters for SMS

| Parameter           | Type    | Required | Description                                                                          |
| ------------------- | ------- | -------- | ------------------------------------------------------------------------------------ |
| title               | string  | Required | Title of the SMS notification                                                        |
| body.content        | string  | Required | Text content of the SMS message                                                      |
| body.otp            | string  | Optional | OTP code when using the 'otp' template                                               |
| notification\_type  | string  | Required | Must be set to 'sms'                                                                 |
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
            "title": "Account Alert",
            "body": {
                "content": "Your transaction of NGN 5,000 was successful. Thank you for using our service."
            },
            "notification_type": "sms",
            "template": "messaging",
            "notification_event": "transaction_alert",
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
            "title": "Verification Code",
            "body": {
                "content": "Your verification code is: ",
                "otp": "123456"
            },
            "notification_type": "sms",
            "template": "otp",
            "notification_event": "user_verification",
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
  "message": "Failed to queue SMS notification",
  "error": {
    "code": "INVALID_PHONE_NUMBER",
    "description": "One or more phone numbers in user_details are in an invalid format"
  },
  "meta": {
    "request_id": "req_20250425172105_m2n3p4"
  }
}
```

####
