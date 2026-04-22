# Enable & Disable Mandate



## Disable/Enable Mandate

<mark style="color:green;">`POST`</mark> `/v2/mandate/disable-enable`



**Headers**

| Name            | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| Content-Type    | `application/json`                                           |
| X-Service-Token | `<`[`service_token`](../payments/#payment-initialization)`>` |

**Body**

<table><thead><tr><th width="187">Name</th><th width="117">Type</th><th width="106">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>mandate_code</code></td><td>string</td><td>Required</td><td>The code for the mandate</td></tr><tr><td><code>mandate_ref</code></td><td>number</td><td>Required</td><td>The reference for the mandate.</td></tr><tr><td><code>mandate_status</code></td><td>string|int</td><td>Required</td><td>The new status of the mandate (i.e. "1" => "active", "2" => "suspend", "3" => "delete").</td></tr><tr><td><code>workflow_status</code></td><td>string|int</td><td>Required</td><td>Set Mandate workflow status. 1 => New Mandate Initiated (this is set automatically) 2 => Mandate Authorized (this is also set automatically) 3 => Mandate Rejected 5 => Mandate Disapproved</td></tr></tbody></table>

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST "{baseUrl}/api/mandate/disable-enable" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
           "mandate_code": "RC1675938/1666/0006552884",
           "mandate_ref": "MT/c0f879167a4223fd99ed",
           "mandate_status": "1",
           "workflow_status": "4"
         }'
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

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "id": 1,
  "name": "John",
  "age": 30
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}



