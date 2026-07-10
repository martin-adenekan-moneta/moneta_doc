# Send notification

This endpoint allows you to send notifications to your users through multiple channels including email, SMS, WhatsApp, and push notifications with customizable content and templates.

#### Implementation Guide

1 Choose Notification Channel

Select the appropriate notification\_type based on your communication needs and user preferences.

2 Prepare Content

Format the notification body according to the chosen channel's requirements.

3 Set Recipients

Add the appropriate recipient identifiers to the user\_details array based on the notification type.

4 Send and Track

Store the reference code from the response for tracking and potential troubleshooting.

<mark style="color:green;">`POST`</mark>` ``{{`[`baseUrl`](../moneta-banking.md)`}}/v2/notification`

#### Request Headers

| Parameter       | Type   | Required | Description                                |
| --------------- | ------ | -------- | ------------------------------------------ |
| Accept          | string | Required | Must be set to 'application/json'          |
| X-Service-Token | string | Required | Your service-specific authentication token |

#### Request Parameters

| Parameter           | Type        | Required | Description                                                                          |
| ------------------- | ----------- | -------- | ------------------------------------------------------------------------------------ |
| title               | string      | Required | Title or subject of the notification                                                 |
| body                | object      | Required | Content of the notification with specific fields based on notification type          |
| body.content        | string      | Optional | Plain text content for SMS and WhatsApp notifications                                |
| body.otp            | string      | Optional | OTP code when using the 'otp' template                                               |
| body.html\_content  | string      | Optional | HTML content for email notifications                                                 |
| notification\_type  | string      | Required | Channel to use for notification (email, sms, whatsapp, push)                         |
| template            | string      | Required | Template type to use (messaging, otp)                                                |
| notification\_event | string      | Required | Event category for analytics and tracking purposes                                   |
| user\_details       | array       | Required | Array of recipient identifiers (emails, phone numbers, or device tokens)             |
| attachments         | array\|null | Optional | Array of attachment objects for email notifications                                  |
| sensitive           | boolean     | Optional | Flag to indicate whether the content contains sensitive information (default: false) |

#### Request Example

{% tabs %}
{% tab title="Curl" %}
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
        "content": "Thank you for joining our platform!",
        "html_content": "<h1>Welcome</h1><p>Thank you for joining our platform!</p>"
    },
    "notification_type": "email",
    "template": "messaging",
    "notification_event": "messaging",
    "user_details": [
        "user@example.com"
    ],
    "attachments": null,
    "sensitive": false
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = 'https://api.example.com/notify';
const data = {
    "notification_type": "email",
    "template": "messaging",
    "notification_event": "messaging",
    "user_details": ["user@example.com"],
    "attachments": null,
    "sensitive": false
};

fetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
})
.then(response => response.json())
.then(result => console.log('Success:', result))
.catch(error => console.error('Error:', error));
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
$url = 'https://api.example.com/notify';
$data = [
    "notification_type" => "email",
    "template" => "messaging",
    "notification_event" => "messaging",
    "user_details" => ["user@example.com"],
    "attachments" => null,
    "sensitive" => false
];

$payload = json_encode($data);

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
    'Content-Length: ' . strlen($payload)
]);

$response = curl_exec($ch);

if (curl_errno($ch)) {
    echo 'Error: ' . curl_error($ch);
} else {
    echo 'Response: ' . $response;
}

curl_close($ch);
?>
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = 'https://api.example.com/notify'
data = {
    "notification_type": "email",
    "template": "messaging",
    "notification_event": "messaging",
    "user_details": ["user@example.com"],
    "attachments": None, # Python uses None for null
    "sensitive": False   # Python uses capitalized Booleans
}

try:
    response = requests.post(url, json=data)
    response.raise_for_stdio() # Raises an error for bad status codes
    print("Success:", response.json())
)
except requests.exceptions.RequestException as e:
    print("Error:", e)
```
{% endtab %}

{% tab title="Java (Spring Boot)" %}
```java
import java.util.List;

public class NotificationRequest {
    public String notification_type = "email";
    public String template = "messaging";
    public String notification_event = "messaging";
    public List<String> user_details = List.of("user@example.com");
    public Object attachments = null;
    public boolean sensitive = false;
}
```
{% endtab %}
{% endtabs %}

#### Response Format

**Success Response**

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
  "message": "Failed to queue notifications",
  "error": {
    "code": "INVALID_PARAMETERS",
    "description": "The notification_type and user_details format do not match"
  },
  "meta": {
    "request_id": "req_20250425163412_j8k9l0"
  }
}
```

#### Notification Type Requirements

| Notification Type | Required Body Fields | User Details Format |
| ----------------- | -------------------- | ------------------- |
| email             | html\_content        | Email addresses     |
| sms               | content              | Phone numbers       |
| whatsapp          | content              | Phone numbers       |
| push              | content              | Device tokens       |

#### Template Types

| Template  | Description                           | Special Requirements    |
| --------- | ------------------------------------- | ----------------------- |
| messaging | General purpose notification template | None                    |
| otp       | One-time password template            | body.otp field required |

####

#### Best Practices

* Keep SMS and WhatsApp messages concise to avoid additional charges for longer messages.
* Use responsive HTML templates for email notifications to ensure proper display across devices.
* Set the sensitive flag to true when sending OTPs or other confidential information.
* Group recipients by notification type rather than sending individual requests for each recipient.
* When using the OTP template, ensure the OTP value is clearly visible in the content.
