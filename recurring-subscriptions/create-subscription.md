# Create Subscription

Billing & recurring subscriptions can be created using the provided customer and transaction details.

## Create Billing

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](./)`}}/mandate/create`

\<Create recurring bills and subscription>

**Headers**

| Name          | Value                                                                  |
| ------------- | ---------------------------------------------------------------------- |
| Content-Type  | `application/json`                                                     |
| Authorization | `Bearer <`[`service_token`](../authentication-and-service-token.md)`>` |

Body

<table data-header-hidden><thead><tr><th width="271">Parameters</th><th width="122">Type</th><th width="109">Required</th><th>Description</th></tr></thead><tbody><tr><td>product_id</td><td>number</td><td>True</td><td>The ID of the product</td></tr><tr><td>customer_phone</td><td>number</td><td>True</td><td>The phone number of the customer</td></tr><tr><td>customer_account_no</td><td>string</td><td>True</td><td>The account number of the customer</td></tr><tr><td>bank_code</td><td>string</td><td>True</td><td>The bank code of the customer's bank</td></tr><tr><td>customer_name</td><td>string</td><td>True</td><td>The name of the customer</td></tr><tr><td>customer_address</td><td>string</td><td>True</td><td>The address of the customer</td></tr><tr><td>customer_account_name</td><td>string</td><td>True</td><td>The account name of the customer</td></tr><tr><td>amount</td><td>number</td><td>True</td><td>The amount of the mandate</td></tr><tr><td>narration</td><td>string</td><td>True</td><td>Description or reason for the mandate</td></tr><tr><td>customer_email</td><td>string</td><td>True</td><td>The email address of the customer</td></tr><tr><td>start_date</td><td>string</td><td>True</td><td>The start date of the mandate</td></tr><tr><td>end_date</td><td>string</td><td>True</td><td>The end date of the mandate</td></tr><tr><td>customer_reference</td><td>string</td><td>True</td><td>The reference for the customer</td></tr><tr><td>customer_bvn</td><td>string</td><td>True</td><td>The Bank Verification Number (BVN) of the customer</td></tr><tr><td>customer_account_kyc_level</td><td>string</td><td>True</td><td>The KYC level of the customer's account</td></tr><tr><td>mandate_type</td><td>number</td><td>True</td><td>The type of mandate</td></tr><tr><td>frequency</td><td>number</td><td>True</td><td>The frequency of the mandate</td></tr></tbody></table>



{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST "YOUR_BASE_URL" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
           "product_id": 12345,
           "customer_phone": 2348012345678,
           "customer_account_no": "1234567890",
           "bank_code": "058",
           "customer_name": "John Doe",
           "customer_address": "123 Street, City, Country",
           "customer_account_name": "John Doe",
           "amount": 100000,
           "narration": "Subscription payment",
           "customer_email": "johndoe@example.com",
           "start_date": "2025-04-01",
           "end_date": "2026-04-01",
           "customer_reference": "REF123456789",
           "customer_bvn": "22334455667",
           "customer_account_kyc_level": "Level 2",
           "mandate_type": 1,
           "frequency": 30
         }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const createMandateRequest = async () => {
  const url = baseUrl;
  const payload = {
    product_id: 12345,
    customer_phone: 2348012345678,
    customer_account_no: '1234567890',
    bank_code: '058',
    customer_name: 'John Doe',
    customer_address: '123 Street, City, Country',
    customer_account_name: 'John Doe',
    amount: 100000,
    narration: 'Subscription payment',
    customer_email: 'johndoe@example.com',
    start_date: '2025-04-01',
    end_date: '2026-04-01',
    customer_reference: 'REF123456789',
    customer_bvn: '22334455667',
    customer_account_kyc_level: 'Level 2',
    mandate_type: 1,
    frequency: 30
  };

  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(payload)
  });

  return await response.json();
};
```
{% endtab %}

{% tab title="PHP" %}
```php
$baseUrl = "YOUR_BASE_URL";
$apiKey = "YOUR_API_KEY";

$payload = [
    "product_id" => 12345,
    "customer_phone" => 2348012345678,
    "customer_account_no" => "1234567890",
    "bank_code" => "058",
    "customer_name" => "John Doe",
    "customer_address" => "123 Street, City, Country",
    "customer_account_name" => "John Doe",
    "amount" => 100000,
    "narration" => "Subscription payment",
    "customer_email" => "johndoe@example.com",
    "start_date" => "2025-04-01",
    "end_date" => "2026-04-01",
    "customer_reference" => "REF123456789",
    "customer_bvn" => "22334455667",
    "customer_account_kyc_level" => "Level 2",
    "mandate_type" => 1,
    "frequency" => 30
];

$ch = curl_init($baseUrl);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($payload));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Authorization: Bearer ' . $apiKey,
    'Content-Type: application/json'
]);

$response = curl_exec($ch);
curl_close($ch);

$data = json_decode($response, true);
print_r($data);
?>
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

def create_mandate_request():
    url = "YOUR_BASE_URL"
    api_key = "YOUR_API_KEY"
    
    payload = {
        "product_id": 12345,
        "customer_phone": 2348012345678,
        "customer_account_no": "1234567890",
        "bank_code": "058",
        "customer_name": "John Doe",
        "customer_address": "123 Street, City, Country",
        "customer_account_name": "John Doe",
        "amount": 100000,
        "narration": "Subscription payment",
        "customer_email": "johndoe@example.com",
        "start_date": "2025-04-01",
        "end_date": "2026-04-01",
        "customer_reference": "REF123456789",
        "customer_bvn": "22334455667",
        "customer_account_kyc_level": "Level 2",
        "mandate_type": 1,
        "frequency": 30
    }

    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }

    response = requests.post(url, json=payload, headers=headers)
    return response.json()

# Example usage:
# print(create_mandate_request())
```
{% endtab %}

{% tab title="Java Spring" %}
```ruby
import org.springframework.web.client.RestTemplate;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import java.util.HashMap;
import java.util.Map;

public class MandateService {

    public Object createMandateRequest() {
        String url = "YOUR_BASE_URL";
        String apiKey = "YOUR_API_KEY";
        RestTemplate restTemplate = new RestTemplate();

        // Prepare Headers
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.setBearerAuth(apiKey);

        // Prepare Payload
        Map<String, Object> payload = new HashMap<>();
        payload.put("product_id", 12345);
        payload.put("customer_phone", 2348012345678L);
        payload.put("customer_account_no", "1234567890");
        payload.put("bank_code", "058");
        payload.put("customer_name", "John Doe");
        payload.put("customer_address", "123 Street, City, Country");
        payload.put("customer_account_name", "John Doe");
        payload.put("amount", 100000);
        payload.put("narration", "Subscription payment");
        payload.put("customer_email", "johndoe@example.com");
        payload.put("start_date", "2025-04-01");
        payload.put("end_date", "2026-04-01");
        payload.put("customer_reference", "REF123456789");
        payload.put("customer_bvn", "22334455667");
        payload.put("customer_account_kyc_level", "Level 2");
        payload.put("mandate_type", 1);
        payload.put("frequency", 30);

        HttpEntity<Map<String, Object>> entity = new HttpEntity<>(payload, headers);

        // Execute Request
        return restTemplate.postForObject(url, entity, Map.class);
    }
}
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "status": true,
  "message": "Mandate created successfully",
  "data": {
    "mandate_ref": "MND123456789",
    "mandate_code": "MND-CODE-987654",
    "response_message": "Mandate successfully created"
  },
  "statusCode": 200
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "responseErrorCode": "05",
    "status": "error",
    "message": "Service token not provided"
}
```
{% endtab %}
{% endtabs %}
