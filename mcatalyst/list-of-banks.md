# List of Banks

This endpoint retrieves the list of avialable banks&#x20;

##

<mark style="color:green;">`POST`</mark> `/v2/get-banks`

&#x20;Remember to include your X-Service-Token header as [shown here](./)

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/bvn/account_identity_check" \
    --header "X-Service-Token: 6337|0IjdE3othWKaECr2GL0ynUWGlmsdFsYI90bqyn7Md2cc154d" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"bvn\": \"consequatur\",
    \"account_number\": \"consequatur\",
    \"institution_code\": \"consequatur\",
    \"first_name\": \"consequatur\",
    \"last_name\": \"consequatur\"
}"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = new URL(
    "https://api.moneta.ng/api/v2/get-banks"
);

const headers = {
    "X-Service-Token": "6337|0IjdE3othWKaECr2GL0ynUWGlmsdFsYI90bqyn7Md2cc154d",
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
$url = 'https://api.moneta.ng/api/v2/get-banks';
$response = $client->get(
    $url,
    [
        'headers' => [
            'X-Service-Token' => '.....',
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
 "status": true,
"message": "Banks fetched successfully",
"data": [
 {
  "id": 1,
  "bank_name": "Bank A",
  "mandate_bank_code": "000"
  "institutionCode": "123456",
  "moneta_code": "040",
  "category": ""
 },
],
"statusCode": 200
}
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
