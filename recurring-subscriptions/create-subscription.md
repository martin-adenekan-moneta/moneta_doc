# Create Subscription

Billing & recurring subscriptions can be created using the provided customer and transaction details.

## Create Billing

<mark style="color:green;">`POST`</mark> `{{`[`baseUrl`](./)`}}/mandate/create`

\<Create recurring bills and subscription>

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |



| product\_id                   | number | True | The ID of the product                              |
| ----------------------------- | ------ | ---- | -------------------------------------------------- |
| customer\_phone               | number | True | The phone number of the customer                   |
| customer\_account\_no         | string | True | The account number of the customer                 |
| bank\_code                    | string | True | The bank code of the customer's bank               |
| customer\_name                | string | True | The name of the customer                           |
| customer\_address             | string | True | The address of the customer                        |
| customer\_account\_name       | string | True | The account name of the customer                   |
| amount                        | number | True | The amount of the mandate                          |
| narration                     | string | True | Description or reason for the mandate              |
| customer\_email               | string | True | The email address of the customer                  |
| start\_date                   | string | True | The start date of the mandate                      |
| end\_date                     | string | True | The end date of the mandate                        |
| customer\_reference           | string | True | The reference for the customer                     |
| customer\_bvn                 | string | True | The Bank Verification Number (BVN) of the customer |
| customer\_account\_kyc\_level | string | True | The KYC level of the customer's account            |
| mandate\_type                 | number | True | The type of mandate                                |
| frequency                     | number | True | The frequency of the mandate                       |

**Response**

{% tabs %}
{% tab title="200" %}
```json
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
