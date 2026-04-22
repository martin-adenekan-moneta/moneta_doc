# Get Wallet Balance

This endpoint allows you to retrieve the current balance and details of your merchant wallet.

#### Implementation Guide

1 Set Up Authentication

Include all required authentication headers in your request.

2 Parse Response

Extract the wallet information from the response, with particular attention to the available\_balance field.

3 Format Balance

Convert the balance from the smallest currency unit (e.g., kobo) to the main currency unit (e.g., naira) for display purposes.

4 Implement Error Handling

Handle potential error responses, especially authentication failures, by prompting the user to re-authenticate if needed.



<mark style="color:$primary;">`GET`</mark> `{{`[`baseUrl`](../messaging-and-push-notifications/)`}}/v2/wallet/balance`

#### Request Headers

| Parameter        | Type   | Required | Description                                |
| ---------------- | ------ | -------- | ------------------------------------------ |
| Accept           | string | Required | Must be set to 'application/json'          |
| X-Client-Id      | string | Required | Your API client ID                         |
| X-Client-Secret  | string | Required | Your API client secret                     |
| X-Merchant-Token | string | Required | Your merchant token for authentication     |
| X-Service-Token  | string | Required | Your service-specific authentication token |

#### Request Example

{% tabs %}
{% tab title="Curl" %}
```bash
'Accept: application/json' \
--header 'X-Client-Id: {{client_Id}}' \
--header 'X-Client-Secret: {{client_secret}}' \
--header 'X-Merchant-Token: {{merchant_token}}' \
--header 'X-Service-Token: {{service_token}}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const message = "hello world";
console.log(message);
```
{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
message = "hello world"
puts message
```
{% endtab %}
{% endtabs %}

#### Response Format

{% tabs %}
{% tab title="200" %}
```json
{
  "message": "Wallet Balance",
  "data": {
    "id": 2,
    "client_id": 1,
    "merchant_name": "Meyor",
    "merchant_email": "meyorpop1@gmail.com",
    "merchant_phone_number": "07065216112",
    "push_notification_alias": null,
    "status": "inactive",
    "wallet": {
      "id": 2,
      "merchant_id": 2,
      "available_balance": 1915,
      "created_at": "2025-03-07T18:28:15.000000Z",
      "updated_at": "2025-03-25T12:56:47.000000Z"
    }
  }
}
```
{% endtab %}

{% tab title="404" %}
```json
{
  "status": "error",
  "message": "Failed to retrieve wallet balance",
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "description": "Invalid or expired authentication credentials"
  },
  "meta": {
    "request_id": "req_20250425154532_c2d9e1"
  }
}
```
{% endtab %}
{% endtabs %}

#### Response Properties

<table><thead><tr><th width="243">Parameter</th><th>Type</th><th width="114">Required</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>number</td><td>Required</td><td>Unique identifier of the merchant</td></tr><tr><td>client_id</td><td>number</td><td>Required</td><td>Identifier of the associated client account</td></tr><tr><td>merchant_name</td><td>string</td><td>Required</td><td>Name of the merchant</td></tr><tr><td>merchant_email</td><td>string</td><td>Required</td><td>Email address of the merchant</td></tr><tr><td>merchant_phone_number</td><td>string</td><td>Required</td><td>Contact phone number of the merchant</td></tr><tr><td>status</td><td>string</td><td>Required</td><td>Current status of the merchant account (active/inactive)</td></tr><tr><td>wallet.available_balance</td><td>number</td><td>Required</td><td>Current available balance in the smallest currency unit</td></tr><tr><td>wallet.created_at</td><td>string</td><td>Required</td><td>ISO 8601 datetime when the wallet was created</td></tr><tr><td>wallet.updated_at</td><td>string</td><td>Required</td><td>ISO 8601 datetime when the wallet was last updated</td></tr></tbody></table>
