# Create Debit Instruction

Creates a Debit Instruction On a Customer Account. The Owner of the profiled Account is expected to Pay the Authorization Amount less than 30 minutes after the creation of the Debit-Instruction. The Mandate expires if no Payment is made in the first 30 minutes after the creation of the Debit-Instruction.

### Debit Instruction

This method fetches mandates for the currently authenticated user, using optional query parameters for `date_from` and `date_to` to filter mandates within a specific date range. If no dates are provided, it defaults to the last 30 days.

## Create Debit Instruction

<mark style="color:green;">`POST`</mark> `/debit-instruction-/create`



**Headers**

| Name              | Value              |
| ----------------- | ------------------ |
| Content-Type      | `application/json` |
| `X-Service-Token` | `Bearer <token>`   |

**Body**

| Name                     | Type    | Description                                                                   |
| ------------------------ | ------- | ----------------------------------------------------------------------------- |
| `mandate_type`           | number  | The type of mandate (1 for Debit Instruction, 2 for Paper Debit Instruction). |
| `customer_phone`         | string  | Customer Phone                                                                |
| `customer_account_no`    | string  |                                                                               |
| `bank_code`              | number/ |                                                                               |

Examples

{% tabs %}
{% tab title="Curl" %}
```bash
curl --request POST \
    "https://api.moneta.ng/api/v2/debit-instruction/create" \
    --header "X-Service-Token: 6337|0IjdE3othWKaECr2GL0ynUWGlmsdFsYI90bqyn7Md2cc154d" \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --data "{
    \"mandate_type\": 1,
    \"customer_phone\": \"08012345678.\",
    \"customer_account_no\": \"1234567890.\",
    \"bank_code\": 0,
    \"customer_name\": \"John Doe.\",
    \"customer_address\": \"123 Main Street.\",
    \"customer_account_name\": \"John Doe.\",
    \"amount\": 1000,
    \"narration\": \"opfuudtdsufvyvddqamni\",
    \"frequency\": 1,
    \"customer_email\": \"2K8lE@example.com.\",
    \"start_date\": \"2106-10-16\",
    \"end_date\": \"2106-10-16\",
    \"customer_reference\": \"mqeopfuudtdsufvyv\",
    \"customer_bvn\": \"12345678901.\",
    \"customer_account_kyc_level\": 1,
    \"customer_signature_file_base_encoded\": \"(base64 encoded string)\",
    \"CreateMandateRequest\": \"consequatur\"
}"
```
{% endtab %}

{% tab title="Javascript" %}
```js
const createMonetaMandate = async (customerData) => {
    const API_URL = {{baseUrl}};
    
    // 1. Setup Headers
    const headers = {
        "X-Service-Token": ".........", // Keep this in .env for production!
        "Content-Type": "application/json",
        "Accept": "application/json",
    };

    try {
        // 2. Execute Request
        const response = await fetch(API_URL, {
            method: "POST",
            headers: headers,
            body: JSON.stringify(customerData),
        });

        // 3. Handle HTTP-level Errors (4xx, 5xx)
        if (!response.ok) {
            const errorBody = await response.json();
            throw new Error(`Moneta API Error (${response.status}): ${errorBody.message || 'Unknown Error'}`);
        }

        // 4. Parse and return successful response
        const result = await response.json();
        console.log("Mandate successfully created:", result.data?.reference || 'No ref returned');
        return result;

    } catch (error) {
        // 5. Centralized Error Logging
        console.error("Mandate Creation Failed:", error.message);
        throw error; // Rethrow so the UI/calling function can show an alert
    }
};

// --- Example Usage ---
const myMandate = {
    mandate_type: 1,
    customer_phone: "08012345678",
    customer_account_no: "1234567890",
    bank_code: "058", // GTBank example
    customer_name: "John Doe",
    amount: 500000,     // (In Kobo)
    narration: "Monthly Subscription",
    frequency: 1,
    start_date: "2026-03-24",
    end_date: "2027-03-24",
    customer_reference: `REF-${Date.now()}`, // Dynamic unique reference
    customer_signature_file_base_encoded: "iVBORw0KGgoAAAANSUhEUg...", // Raw Base64
};
```
{% endtab %}

{% tab title="Php" %}

{% endtab %}

{% tab title="Laravel" %}

{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
```
{% endtab %}

{% tab title="Java" %}
```java
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

