# All Mandates

## Fetch Mandate

<mark style="color:green;">`POST`</mark> [`{{baseUrl}}`](./)`/mandate/details`

\<Retrieve details of a mandate by providing the `mandate_code` and `mandate_ref` in the request body.>

**Headers**

| Name            | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| Content-Type    | `application/json`                                           |
| X-Service-Token | `<`[`service_token`](../payments/#payment-initialization)`>` |

**Body**

| Parameter     | Type   | Required | Description                    |
| ------------- | ------ | -------- | ------------------------------ |
| mandate\_code | string | Required | The code for the mandate.      |
| mandate\_ref  | string | Required | The reference for the mandate. |

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
