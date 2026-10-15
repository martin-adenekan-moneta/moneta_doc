# Sending Notifications

This endpoint allows you to send notifications to your users through multiple channels including email, SMS, WhatsApp, and push notifications with customizable content and templates.

#### Implementation Guide

1. &#x20;Choose Notification Channel

Select the appropriate notification\_type based on your communication needs and user preferences.

2. &#x20;Prepare Content

Format the notification body according to the chosen channel's requirements.

3. &#x20;Set Recipients

Add the appropriate recipient identifiers to the user\_details array based on the notification type.

4\. Send and Track

Store the reference code from the response for tracking and potential troubleshooting.

<mark style="color:green;">`POST`</mark>` ``{{`[`baseUrl`](../../moneta-banking.md)`}}/notification`

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
curl --location '{{url}}' \
--header 'Accept: application/json' \
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
const payload = {
  title: "Welcome to Our Platform",
  body: {
    content: "Thank you for joining our platform!",
    html_content: "<h1>Welcome</h1><p>Thank you for joining our platform!</p>"
  },
  notification_type: "email",
  template: "messaging",
  notification_event: "messaging",
  user_details: ["user@example.com"],
  attachments: null,
  sensitive: false
};

async function sendNotification() {
  try {
    const response = await fetch(`${BASE_URL}/notifications`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Service-Token': serviceToken
      },
      body: JSON.stringify(payload)
    });

    const data = await response.json();
    console.log('Success:', data);
  } catch (error) {
    console.error('Error:', error);
  }
}
```
{% endtab %}

{% tab title="PHP" %}
```php
$payload = [
    'title' => 'Welcome to Our Platform',
    'body' => [
        'content' => 'Thank you for joining our platform!',
        'html_content' => '<h1>Welcome</h1><p>Thank you for joining our platform!</p>',
    ],
    'notification_type' => 'email',
    'template' => 'messaging',
    'notification_event' => 'messaging',
    'user_details' => [
        'user@example.com',
    ],
    'attachments' => null,
    'sensitive' => false,
];

$ch = curl_init($BASE_URL . '/notifications');

curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => [
        'Content-Type: application/json',
        'X-Service-Token: ' . $serviceToken,
    ],
    CURLOPT_POSTFIELDS => json_encode($payload),
]);

$response = curl_exec($ch);

if (curl_errno($ch)) {
    echo 'cURL Error: ' . curl_error($ch);
} else {
    echo 'Response: ' . $response;
}

curl_close($ch);
```
{% endtab %}

{% tab title="Laravel" %}
```php
$payload = [
    'title' => 'Welcome to Our Platform',
    'body' => [
        'content' => 'Thank you for joining our platform!',
        'html_content' => '<h1>Welcome</h1><p>Thank you for joining our platform!</p>',
    ],
    'notification_type' => 'email',
    'template' => 'messaging',
    'notification_event' => 'messaging',
    'user_details' => [
        'user@example.com',
    ],
    'attachments' => null,
    'sensitive' => false,
];

$response = Http::baseUrl($BASE_URL)
    ->withHeaders([
        'X-Service-Token' => $serviceToken,
    ])
    ->post('/notifications', $payload);

if ($response->successful()) {
    $data = $response->json();
    // Process response
} else {
    // Handle error: $response->status(), $response->body()
}
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

headers = {
    "Content-Type": "application/json",
    "X-Service-Token": service_token
}

payload = {
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
    "attachments": None,
    "sensitive": False
}

response = requests.post(f"{BASE_URL}/notifications", json=payload, headers=headers)

print(response.status_code)
print(response.json())
```
{% endtab %}

{% tab title="Java (Spring Boot)" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class NotificationSender {
    public static void main(String[] args) {
        String BASE_URL = "https://api.example.com";
        String serviceToken = "YOUR_SERVICE_TOKEN";

        String jsonPayload = """
            {
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
            }
            """;

        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(BASE_URL + "/notifications"))
                .header("Content-Type", "application/json")
                .header("X-Service-Token", serviceToken)
                .POST(HttpRequest.BodyPublishers.ofString(jsonPayload))
                .build();

        try {
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
            System.out.println("Status Code: " + response.statusCode());
            System.out.println("Response Body: " + response.body());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
{% endtab %}
{% endtabs %}

#### Response Format

{% tabs %}
{% tab title="Success 200" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "message": "Successfully queued 1 notifications",
  "data": {
    "cost": 5,
    "reference": "NOTIF-67e2a93e9ab38-561181"
  }
}
</code></pre>
{% endtab %}

{% tab title="Error 400" %}
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
{% endtab %}
{% endtabs %}



#### Notification Type Requirements

| Notification Type  | Required Body Fields | User Details Format |
| ------------------ | -------------------- | ------------------- |
| email              | html\_content        | Email addresses     |
| sms                | content              | Phone numbers       |
| whatsapp           | content              | Phone numbers       |
| push\_notification | content              | Device tokens       |

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
