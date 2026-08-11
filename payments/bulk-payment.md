# Bulk Payment

Aside from the individual payment system, we provide a straightforward disbursement system for processing bulk payment.

## Bulk Disbursement

<mark style="color:green;">`POST`</mark>  \{{[baseUrl](./#base-url-for-payment)\}}/v2/debit-instruction/debit/bulk

disbursement based on series of transactions to be paid at a go.

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |

**Body**

<table><thead><tr><th width="238">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>batch_reference</code></td><td>string</td><td>Name of the user</td></tr><tr><td><code>disbursement_webhook_url</code></td><td>number</td><td>Age of the user</td></tr><tr><td>beneficiaries</td><td>array</td><td><p>an array of beneficiary accounts to be paid into </p><pre class="language-postman_json"><code class="lang-postman_json">[{
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
curl --location '{{baseUrl}}/v2/debit-instruction/debit/bulk' \
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
const url = '{{baseUrl}}/v2/debit-instruction/debit/bulk';

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

$url = '{{baseUrl}}/v2/debit-instruction/debit/bulk';

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
        $url = '{{baseUrl}}/v2/debit-instruction/debit/bulk';

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

url = "{{baseUrl}}/v2/debit-instruction/debit/bulk"

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

{% tab title="Java" %}
<pre class="language-java"><code class="lang-java"><strong>// DTO Classes
</strong>import java.util.List;

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
    List&#x3C;Beneficiary> beneficiaries
) {}

//Service Class
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
        List&#x3C;Beneficiary> beneficiaries = List.of(
            new Beneficiary("83487-2025954-00051", 39000, "0123869864", "000013", "Staff training "),
            new Beneficiary("6823498-089766", 39000, "09127843913", "000013", "imbursement")
        );

        BulkDebitRequest requestBody = new BulkDebitRequest(
            "883264642938",
            "https://webhook.site/your-test-endpoint",
            beneficiaries
        );

        return restClient.post()
                .uri("{{baseUrl}}/v2/debit-instruction/debit/bulk")
                .contentType(MediaType.APPLICATION_JSON)
                .body(requestBody)
                .retrieve()
                .body(String.class);
    }
}

</code></pre>
{% endtab %}
{% endtabs %}

**Response**

**Note: When a disbursement batch is submitted, when there are unsuccessful transactions in it, it doesn't cause the whole batch to fail, you can always resend any failed transaction when it has been resolved.**

{% tabs %}
{% tab title="200" %}
```json
{
    "status": true,
    "message": "Batch has been created for the Disbursement",
    "data": {
        "batch_reference": "BATCH-2025-TEST-002",
        "processed_count": 2,
        "unprocessed_count":0
    },
    "statusCode": 200,
    "errors": []   
}
```
{% endtab %}

{% tab title="Possible Errors Within a success response" %}
```json
{
    "status": true,
    "message": "Batch has been created for the Disbursement",
    "data": {
        "batch_reference": "BATCH-2025-TEST-002",
        "processed_count": 20,
        "unprocessed_count": 5
    },
    "statusCode": 200,
    "errors": [
        {
            "beneficiary": {
                "transaction_reference": "TXN-20250520-00070",
                "amount": 0,
                "account_number": "0123456770",
                "institution_code": "000014",
                "narration": "INVALID: zero amount fails min:1 validation"
            },
            "errors": [
                {
                    "field": "amount",
                    "message": [
                        "The amount field must be at least 1."
                    ]
                }
            ]
        },
        {
            "beneficiary": {
                "transaction_reference": "TXN-20250520-00071",
                "amount": -1500,
                "account_number": "0123456771",
                "institution_code": "000015",
                "narration": "INVALID: negative amount not permitted"
            },
            "errors": [
                {
                    "field": "amount",
                    "message": [
                        "The amount field must be at least 1."
                    ]
                }
            ]
        },
        {
            "beneficiary": {
                "transaction_reference": "TXN-20250520-00072",
                "amount": 18000,
                "account_number": null,
                "institution_code": "000016",
                "narration": "INVALID: account_number is empty"
            },
            "errors": [
                {
                    "field": "account_number",
                    "message": [
                        "The account number field is required."
                    ]
                }
            ]
        },
        {
            "beneficiary": {
                "transaction_reference": "TXN-20250520-00074",
                "amount": 33000,
                "account_number": "0123456774",
                "institution_code": null,
                "narration": "INVALID: institution_code is empty"
            },
            "errors": [
                {
                    "field": "institution_code",
                    "message": [
                        "The institution code field is required."
                    ]
                }
            ]
        },
        {
            "beneficiary": {
                "transaction_reference": "TXN-20250520-00051",
                "amount": 7000,
                "account_number": "0123456775",
                "institution_code": "000013",
                "narration": "INVALID: duplicate transaction_reference within batch (same as TXN-20250520-00051)"
            },
            "errors": [
                {
                    "field": "transaction_reference",
                    "message": "Duplicate transaction_reference in this batch (first seen on row 1)."
                }
            ]
        }
    ]
}
```
{% endtab %}
{% endtabs %}



## Get Data

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](./#base-url-for-payment)`}}/v2/debit-instruction/debit/bulk/data`

Check the details of your batch or transaction in a batch

**Headers**

| Name            | Value                                               |
| --------------- | --------------------------------------------------- |
| Content-Type    | `application/json`                                  |
| X-Service-Token |  `<`[`service_token`](./#payment-initialization)`>` |



**Body**

| Name                    | Type   | Description                                                                             |
| ----------------------- | ------ | --------------------------------------------------------------------------------------- |
| `batch_reference`       | string | Batch Reference (optional)                                                              |
| `transaction_reference` | string | Transaction reference (provide if you want to view only a single transaction reference) |

**Response**

{% tabs %}
{% tab title="Batch Response 200" %}
```json
{
    "status": true,
    "message": "Bulk Direct Debit Mandeto",
    "data": [
        {
            "id": 52,
            "user_id": 22,
            "batch_reference": "BATCH-2025-TEST-002",
            "transaction_reference": "TXN-20250520-00051",
            "session_id": "Tu4X9XTiaukApdcg6VHpOhX9gwtYGx",
            "moneta_reference": "MNTXTXN-202505260520234706874992979554",
            "amount": "34000.00",
            "beneficiary_account_name": null,
            "beneficiary_account_number": "0123456751",
            "destination_instituion_code": "000013",
            "narration": "Staff training reimbursement",
            "transaction_status": "FAILED",
            "response_code": null,
            "response_message": null,
            "fee": "0.00",
            "total_amount": "0.00",
            "retry_count": 0,
            "processed_at": null,
            "created_at": "2026-05-20T22:47:06.000000Z",
            "updated_at": "2026-05-20T22:47:06.000000Z"
        }, ...
    ],
    "statusCode": 200,
    "errors": null
}
```
{% endtab %}

{% tab title="Single Transaction 200" %}
```json
{
    "status": true,
    "message": "Bulk Direct Debit Mandeto",
    "data":
        {
            "id": 62,
            "user_id": 22,
            "batch_reference": "BATCH-2025-TEST-002",
            "transaction_reference": "TXN-20250520-00061",
            "session_id": "8R96ZLguVqOGLzPJwWYgXuQSHuOiXc",
            "moneta_reference": "MNTXTXN-202505260520234706606936241548",
            "amount": "38500.00",
            "beneficiary_account_name": null,
            "beneficiary_account_number": "0123456761",
            "destination_instituion_code": "090110",
            "narration": "VFD MFB supplier payment",
            "transaction_status": "SUCCESS",
            "response_code": null,
            "response_message": null,
            "fee": "0.00",
            "total_amount": "0.00",
            "retry_count": 0,
            "processed_at": null,
            "created_at": "2026-05-20T22:47:06.000000Z",
            "updated_at": "2026-05-20T23:25:55.000000Z"
        }
    "statusCode": 200,
    "errors": null
}
```
{% endtab %}
{% endtabs %}



