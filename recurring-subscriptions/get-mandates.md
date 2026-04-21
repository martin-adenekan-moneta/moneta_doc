# Get Mandates

## Fetch Mandate Details

<mark style="color:green;">`POST`</mark> [`{{baseUrl}}`](./)`/mandate/details`

\<Retrieve details of a mandate by providing the `mandate_code` and `mandate_ref` in the request body.>

**Headers**

| Name            | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| Content-Type    | `application/json`                                           |
| X-Service-Token | `<`[`service_token`](../payments/#payment-initialization)`>` |

**Body**

| Parameter      | Type   | Required | Description                    |
| -------------- | ------ | -------- | ------------------------------ |
| `mandate_code` | string | Required | The code for the mandate.      |
| `mandate_ref`  | string | Required | The reference for the mandate. |

Example

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST "{baseUrl}/api/mandate/details" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
           "mandate_code": "MND123456789",
           "mandate_ref": "REF987654321"
         }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getMandateDetails = async (mandateCode, mandateRef) => {
  const url = '{baseUrl}/api/mandate/details';
  
  const payload = {
    mandate_code: mandateCode,
    mandate_ref: mandateRef
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
<?php
$url = "{baseUrl}/api/mandate/details";
$apiKey = "YOUR_API_KEY";

$payload = [
    "mandate_code" => "MND123456789",
    "mandate_ref" => "REF987654321"
];

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($payload));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Authorization: Bearer ' . $apiKey,
    'Content-Type: application/json'
]);

$response = curl_exec($ch);
curl_close($ch);

$details = json_decode($response, true);
print_r($details);
?>
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

def get_mandate_details(mandate_code, mandate_ref):
    url = "{baseUrl}/api/mandate/details"
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    }
    payload = {
        "mandate_code": mandate_code,
        "mandate_ref": mandate_ref
    }

    response = requests.post(url, json=payload, headers=headers)
    return response.json()
```
{% endtab %}

{% tab title="Java Spring" %}
```ruby
import org.springframework.web.client.RestTemplate;
import org.springframework.http.*;
import java.util.HashMap;
import java.util.Map;

public class MandateService {

    public Map<String, Object> getMandateDetails(String code, String ref) {
        String url = "{baseUrl}/api/mandate/details";
        RestTemplate restTemplate = new RestTemplate();

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.setBearerAuth("YOUR_API_KEY");

        Map<String, String> payload = new HashMap<>();
        payload.put("mandate_code", code);
        payload.put("mandate_ref", ref);

        HttpEntity<Map<String, String>> entity = new HttpEntity<>(payload, headers);

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
  "message": "mandate retrieved successfully",
  "data": {
    "id": 4,
    "user_id": 1,
    "product_id": 1,
    "user_service_id": 1,
    "mandate_code": "0000004/001/877790",
    "customer_phone": "22475613334",
    "customer_account_no": "<encrypted>",
    "bank_code": "<encrypted>",
    "customer_name": "Egbeleke Daniel Fiyinfoluwa",
    "customer_address": "27, femi owolabi street, omo oye buststop, abarunje,ikotun, Lagos",
    "amount": "500",
    "frequency": "0",
    "customer_email": "www.daniko15@gmail.com",
    "start_date": "2024-07-17",
    "end_date": "2024-07-25",
    "customer_reference": "danthebadguy",
    "response_message": "Welcome to NIBSS e-mandate authentication service. Kindly proceed with a token payment of N50:00 into the account number below Account Number: 9880218357, Bank: Titan-Paystack Bank.",
    "nibss_mandate_category": "e-mandate",
    "created_at": "2024-07-16T10:16:07.000000Z",
    "updated_at": "2024-07-16T10:16:07.000000Z"
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







