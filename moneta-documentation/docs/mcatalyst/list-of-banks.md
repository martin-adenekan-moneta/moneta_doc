# List of Banks

This endpoint retrieves the list of avialable banks&#x20;

##

<mark style="color:green;">`POST`</mark> [`{{baseUrl}}`](./#base-url-for-mcatalyst)`/v2/banks`

&#x20;Remember to include your X-Service-Token header as [shown here](./)

**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| Content-Type      | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

Body

{% tabs %}
{% tab title="Curl" %}
```bash

curl --request POST \
    "{{baseUrl}}/v2/banks" \
    --header "X-Service-Token: ................................" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" 

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = new URL(baseUrl+"/v2/banks");

const headers = {
    "X-Service-Token": "..................................",
    "Content-Type": "application/json",
    "Accept": "application/json",
};

fetch(url, {
    method: "GET",
    headers,
}).then(response => response.json());
```
{% endtab %}

{% tab title="PHP" %}
```php
$client = new \GuzzleHttp\Client();
$url = baseUrl+'/v2/banks';
$response = $client->get(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '..............',
            'Content-Type' => 'application/json',
            'Accept' => 'application/json',
        ],
    ]
);
$body = $response->getBody();
print_r(json_decode((string) $body));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = baseUrl+'/v2/banks'
headers = {
  'X-Service-Token': '.................................',
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request('GET', url, headers=headers)
response.json()
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "status": "success",
  "data": [
    {
      "id": 1,
      "bank_name": "STERLING BANK",
      "institutionCode": "000001",
      "moneta_code": "232"
    },
    {
      "id": 2,
      "bank_name": "KEYSTONE BANK",
      "institutionCode": "000002",
      "moneta_code": "082"
    },
.....
```
{% endtab %}

{% tab title="400" %}
```json
{
    "status": false,
    "message": "Error",
    "data": [],
    "statusCode": 400
}
```
{% endtab %}
{% endtabs %}
