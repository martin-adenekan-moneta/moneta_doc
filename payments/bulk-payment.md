# Bulk Payment

Aside from the individual payment system, we provide a straightforward disbursement system for processing bulk payment.

## bulk payment&#x20;

<mark style="color:green;">`POST`</mark> /v2/debit-instruction/debit/bulk

disbursement based on series of transactions to be paid at a go.

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |

**Body**

<table><thead><tr><th>Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>batch_reference</code></td><td>string</td><td>Name of the user</td></tr><tr><td><code>disbursement_webhook_url</code></td><td>number</td><td>Age of the user</td></tr><tr><td>beneficiaries</td><td>array</td><td><p>an array of beneficiary accounts to be paid into </p><pre class="language-postman_json"><code class="lang-postman_json">[{
      "transaction_reference": ".....",
      "amount": 39000,
      "account_number": "...",
      "institution_code": "...",
      "narration": "...."
    },.....
]
</code></pre></td></tr></tbody></table>



{% tabs %}
{% tab title="Curl" %}


```bash
curl --location 'https://your-api-domain.com/v2/debit-instruction/debit/bulk' \
--header 'Content-Type: application/json' \
--data '{
  "batch_reference": "883264642938",
  "disbursement_webhook_url": "https://webhook.site/your-test-endpoint",
  "beneficiaries": [
    {
      "transaction_reference": "83487-2025954-00051",
      "amount": 39000,
      "account_number": "0123869864",
      "institution_code": "000013",
      "narration": "Staff training "
    },
    {
      "transaction_reference": "6823498-089766",
      "amount": 39000,
      "account_number": "09127843913",
      "institution_code": "000013",
      "narration": "imbursement"
    }
  ]
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const url = 'https://your-api-domain.com/v2/debit-instruction/debit/bulk';

const payload = {
  batch_reference: "883264642938",
  disbursement_webhook_url: "https://webhook.site/your-test-endpoint",
  beneficiaries: [
    {
      transaction_reference: "83487-2025954-00051",
      amount: 39000,
      account_number: "0123869864",
      institution_code: "000013",
      narration: "Staff training "
    },
    {
      transaction_reference: "6823498-089766",
      amount: 39000,
      account_number: "09127843913",
      institution_code: "000013",
      narration: "imbursement"
    }
  ]
};

async function sendBulkDebit() {
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(payload)
    });

    const data = await response.json();
    console.log('Response:', data);
  } catch (error) {
    console.error('Error:', error);
  }
}

sendBulkDebit();
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php

$url = 'https://your-api-domain.com/v2/debit-instruction/debit/bulk';

$payload = [
    "batch_reference" => "883264642938",
    "disbursement_webhook_url" => "https://webhook.site/your-test-endpoint",
    "beneficiaries" => [
        [
            "transaction_reference" => "83487-2025954-00051",
            "amount" => 39000,
            "account_number" => "0123869864",
            "institution_code" => "000013",
            "narration" => "Staff training "
        ],
        [
            "transaction_reference" => "6823498-089766",
            "amount" => 39000,
            "account_number" => "09127843913",
            "institution_code" => "000013",
            "narration" => "imbursement"
        ]
    ]
];

$ch = curl_init($url);

curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($payload));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json'
]);

$response = curl_exec($ch);

if (curl_errno($ch)) {
    echo 'Error: ' . curl_error($ch);
} else {
    echo 'Response: ' . $response;
}

curl_close($ch);
```
{% endtab %}

{% tab title="Laravel (PHP)" %}
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Http;

class DebitInstructionController extends Controller
{
    public function sendBulkDebit()
    {
        $url = 'https://your-api-domain.com/v2/debit-instruction/debit/bulk';

        $payload = [
            "batch_reference" => "883264642938",
            "disbursement_webhook_url" => "https://webhook.site/your-test-endpoint",
            "beneficiaries" => [
                [
                    "transaction_reference" => "83487-2025954-00051",
                    "amount" => 39000,
                    "account_number" => "0123869864",
                    "institution_code" => "000013",
                    "narration" => "Staff training "
                ],
                [
                    "transaction_reference" => "6823498-089766",
                    "amount" => 39000,
                    "account_number" => "09127843913",
                    "institution_code" => "000013",
                    "narration" => "imbursement"
                ]
            ]
        ];

        // Send POST request with JSON payload
        $response = Http::acceptJson()
            ->post($url, $payload);

        // Check if the request was successful (2xx status code)
        if ($response->successful()) {
            return response()->json([
                'status' => 'success',
                'data' => $response->json()
            ]);
        }

        // Handle error responses (4xx or 5xx)
        return response()->json([
            'status' => 'error',
            'message' => 'Failed to process bulk debit',
            'details' => $response->json()
        ], $response->status());
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://your-api-domain.com/v2/debit-instruction/debit/bulk"

payload = {
    "batch_reference": "883264642938",
    "disbursement_webhook_url": "https://webhook.site/your-test-endpoint",
    "beneficiaries": [
        {
            "transaction_reference": "83487-2025954-00051",
            "amount": 39000,
            "account_number": "0123869864",
            "institution_code": "000013",
            "narration": "Staff training "
        },
        {
            "transaction_reference": "6823498-089766",
            "amount": 39000,
            "account_number": "09127843913",
            "institution_code": "000013",
            "narration": "imbursement"
        }
    ]
}

headers = {
    "Content-Type": "application/json"
}

response = requests.post(url, json=payload, headers=headers)

print("Status Code:", response.status_code)
print("Response:", response.json())
```
{% endtab %}

{% tab title="Untitled" %}
```java
import java.util.List;

public record Beneficiary(
    String transaction_reference,
    long amount,
    String account_number,
    String institution_code,
    String narration
) {}

public record BulkDebitRequest(
    String batch_reference,
    String disbursement_webhook_url,
    List<Beneficiary> beneficiaries
) {}



import org.springframework.stereotype.Service;
import org.springframework.web.client.RestClient;
import org.springframework.http.MediaType;
import java.util.List;

@Service
public class DebitInstructionService {

    private final RestClient restClient;

    public DebitInstructionService() {
        this.restClient = RestClient.builder()
                .baseUrl("https://your-api-domain.com")
                .build();
    }

    public String sendBulkDebit() {
        List<Beneficiary> beneficiaries = List.of(
            new Beneficiary("83487-2025954-00051", 39000, "0123869864", "000013", "Staff training "),
            new Beneficiary("6823498-089766", 39000, "09127843913", "000013", "imbursement")
        );

        BulkDebitRequest requestBody = new BulkDebitRequest(
            "883264642938",
            "https://webhook.site/your-test-endpoint",
            beneficiaries
        );

        return restClient.post()
                .uri("/v2/debit-instruction/debit/bulk")
                .contentType(MediaType.APPLICATION_JSON)
                .body(requestBody)
                .retrieve()
                .body(String.class);
    }
}
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
