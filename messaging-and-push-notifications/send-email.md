# Send Email

This endpoint allows you to send email notifications to your users with customizable HTML content. Email notifications are ideal for detailed information, receipts, and content that requires rich formatting.

#### Implementation Best Practices

1 HTML Email Design

Create responsive HTML templates that display properly across various email clients and screen sizes.

2 Always Include Plain Text

Provide both HTML and plain text content to ensure deliverability and accessibility for all recipients.

3 Subject Line Optimization

Keep subject lines under 50 characters and avoid using spam trigger words to improve deliverability.

4 Email Validation

Validate email addresses before sending to prevent bounces and maintain a good sender reputation.

5 Handling Attachments

Keep attachments small (under 10MB total) and use widely supported file formats to ensure delivery.



<mark style="color:green;">`POST`</mark>  `{{baseUrl}}/v2/notification`

#### Request Headers

| Parameter        | Type   | Required | Description                                |
| ---------------- | ------ | -------- | ------------------------------------------ |
| Accept           | string | Required | Must be set to 'application/json'          |
| X-Client-Id      | string | Required | Your API client ID                         |
| X-Client-Secret  | string | Required | Your API client secret                     |
| X-Merchant-Token | string | Required | Your merchant token for authentication     |
| X-Service-Token  | string | Required | Your service-specific authentication token |

#### Request Parameters for Email

| Parameter           | Type        | Required | Description                                                                          |
| ------------------- | ----------- | -------- | ------------------------------------------------------------------------------------ |
| title               | string      | Required | Subject line of the email                                                            |
| body.content        | string      | Optional | Plain text content (fallback for email clients that don't support HTML)              |
| body.html\_content  | string      | Required | HTML content of the email body                                                       |
| body.otp            | string      | Optional | OTP code when using the 'otp' template                                               |
| notification\_type  | string      | Required | Must be set to 'email'                                                               |
| template            | string      | Required | Template type to use (messaging, otp)                                                |
| notification\_event | string      | Required | Event category for analytics and tracking purposes                                   |
| user\_details       | array       | Required | Array of recipient email addresses                                                   |
| attachments         | array\|null | Optional | Array of attachment objects (optional)                                               |
| sensitive           | boolean     | Optional | Flag to indicate whether the content contains sensitive information (default: false) |

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
            "title": "Welcome to Our Platform",
            "body": {
                "content": "Welcome to our platform! Thank you for signing up.",
                "html_content": "<h1>Welcome</h1><p>Thank you for joining our platform! We are excited to have you on board.</p>"
            },
            "notification_type": "email",
            "template": "messaging",
            "notification_event": "user_onboarding",
            "user_details": [
                "user@example.com"
            ],
            "attachments": null,
            "sensitive": false
        }'
```

#### OTP Email Example

bash

```bash
curl --location 'https://api.moneta.ng/api/v2/notification' \
        --header 'Accept: application/json' \
        --header 'X-Client-Id: {{client_Id}}' \
        --header 'X-Client-Secret: {{client_secret}}' \
        --header 'X-Merchant-Token: {{merchant_token}}' \
        --header 'X-Service-Token: {{service_token}}' \
        --data-raw '{
            "title": "Verify Your Account",
            "body": {
                "content": "Your verification code is 123456",
                "html_content": "<h2>Account Verification</h2><p>Use the following code to verify your account:</p><h1 style=\"color: #007bff; font-size: 32px; letter-spacing: 5px;\">123456</h1>",
                "otp": "123456"
            },
            "notification_type": "email",
            "template": "otp",
            "notification_event": "account_verification",
            "user_details": [
                "user@example.com"
            ],
            "attachments": null,
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
  "message": "Failed to queue email notification",
  "error": {
    "code": "INVALID_EMAIL",
    "description": "One or more email addresses in user_details are invalid"
  },
  "meta": {
    "request_id": "req_20250425175412_t8u9v0"
  }
}
```

####
