# Retrieve All Merchant Associated Mandates

## All Merchant

<mark style="color:green;">`POST`</mark> `/users`



**Headers**

| Name            | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| Content-Type    | `application/json`                                           |
| X-Service-Token | `<`[`service_token`](../payments/#payment-initialization)`>` |

**Example**

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X GET "{baseUrl}/api/mandate/all" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getAllMandates = async () => {
  const url = '{baseUrl}/api/mandate/all';

  const response = await fetch(url, {
    method: 'GET',
    headers: {
      'Authorization': `Bearer ${API_KEY}`,
      'Content-Type': 'application/json'
    }
  });

  return await response.json();
};
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
$url = "{baseUrl}/api/mandate/all";
$apiKey = "YOUR_API_KEY";

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Authorization: Bearer ' . $apiKey,
    'Content-Type: application/json'
]);

$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);
// Accessing the list: $mandates = $result['data'];
print_r($result);
?>
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

def get_all_mandates():
    url = "{baseUrl}/api/mandate/all"
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    }

    response = requests.get(url, headers=headers)
    return response.json()

# Example:
# mandates = get_all_mandates().get('data', [])
```
{% endtab %}

{% tab title="Java Spring" %}
```ruby
import org.springframework.web.client.RestTemplate;
import org.springframework.http.*;
import java.util.Map;

public class MandateService {

    public Map<String, Object> getAllMandates() {
        String url = "{baseUrl}/api/mandate/all";
        RestTemplate restTemplate = new RestTemplate();

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.setBearerAuth("YOUR_API_KEY");

        HttpEntity<String> entity = new HttpEntity<>(headers);

        ResponseEntity<Map> response = restTemplate.exchange(
            url, 
            HttpMethod.GET, 
            entity, 
            Map.class
        );

        return response.getBody();
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













