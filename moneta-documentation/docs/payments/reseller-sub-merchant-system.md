# Reseller Sub Merchant System

Reseller Transaction Initialization & Dynamic Split Settlement (API v2)

{% hint style="warning" %}
Settlement Eligibility Notice (Important): Reseller sub-merchant delegation and dynamic split settlement are currently supported only for BulkSettlement accounts ( settlement\_type\_id = 2 / Secondary T+1 Bulk Settlement). Individual (per-transaction) settlement is not supported for this flow.
{% endhint %}

### Endpoint & Authentication

[](../authentication-and-service-token.md)Authentication:&#x20;Use yourReseller [service token generated from here](../authentication-and-service-token.md) to initialize the transaction

```
Authorization: Bearer <RESELLER_API_TOKEN> 
Content-Type: application/json
Accept: application/json
```

#### Mode 1 — Initialize on Behalf of a Sub-Merchant (Direct Attribution)

Use this mode when you want the**transaction record to go directly to the sub-merchant's account**\
while splitting proceeds into the sub-merchant's specified bank accounts (or pre-configured split rule).

**How It Works**

1. Authenticate with your Reseller Bearer token and MAC key.&#x20;
2. Include `sub_merchant_id` at the top level of the request body.&#x20;
3. Moneta verifies that your account is an active Reseller and that `sub_merchant_id` belongs to your reseller hierarchy.&#x20;
4. &#x20;The transaction record is attributed directly to the sub-merchant while tracking your `reseller_id` and `affiliate_code` for reporting and commission.&#x20;
5. Funds are split to the sub-merchant's accounts via split (or an existing `use_split` split code).

Example A — Direct Sub-Merchant Attribution with Percentage Split

```json
{ 
    "sub_merchant_id": 1042, 
    "amount": 5000000, 
    "email": "customer@example.com", 
    "payment_type": "card", 
    "callback_url": "https://merchant.example.com/payment/callback", 
    "hash": "<HMAC_SHA512_HASH>", 
    "json": "true", 
    "transaction_fee_bearer": "customer", 
    "split": { 
        "type": "percentage", 
        "subaccounts": [ 
            { "subaccount": "0123456789", "bank_code": "058", "share": 85 }, 
            { "subaccount": "9876543210", "bank_code": "044", "share": "Residue" } 
        ] 
    }
}
```

**Example B** — Direct Sub-Merchant Attribution with Fixed Amount (_flat_) Split

In&#x20;_flat_&#x20;mode, all numeric&#x20;share&#x20;values are passed in&#x20;**kobo**.

```json
{ 
    "sub_merchant_id": 1042, 
    "amount": 5000000, 
    "email": "customer@example.com",
    "payment_type": "card", 
    "callback_url": "https://merchant.example.com/payment/callback", 
    "hash": "<HMAC_SHA512_HASH>", 
    "json": "true", 
    "transaction_fee_bearer": "merchant", 
    "split": { 
        "type": "flat", 
        "subaccounts": [ 
            { "subaccount": "0123456789", "bank_code": "058", "share": 4500000, "fee_bearer": true }, 
            { "subaccount": "9876543210", "bank_code": "044", "share": 500000 } 
        ] 
    }
}
```

{% hint style="info" %}
Tip: If the sub-merchant already has a default or named split group configured on Moneta, you may omit split and pass "use\_split": "true" or "use\_split": "\<split\_code>" .
{% endhint %}

#### Mode 2 — Initialize Under Reseller's Own Account & Split to Sub-Merchant(s)

Use this mode when you want the&#x20;transaction record to remain under your Reseller account\
while splitting an exact amount or percentage of the payment to one or more sub-merchants.

**How It Works**

1. Authenticate with your   &#x20;Reseller   &#x20;Bearer token and MAC key.
2. Omit _`sub_merchant_id`_ at the top level — the transaction record stays attributed to your Reseller account.
3. Inside `split.subaccounts` , pass each sub-merchant's `sub_merchant_id` and share (in kobo for `flat` ,or % for `percentage` ). You do not need to pass `subaccount` or `bank_code` — Moneta verifies reseller ownership and automatically resolves each sub-merchant's configured default settlement account.
4. For your own reseller cut, you can pass your reseller user ID as `sub_merchant_id` , or pass an explicit `subaccount` + `bank_code` with `"share": "Residue"` (both formats can be mixed in the same `subaccounts` array).

Example A - Reseller-Owned Transaction Splitting by `sub_merchant_id` (Fixed Amount / `flat` )

```json
{ 
    "amount": 5000000, 
    "email": "customer@example.com", 
    "payment_type": "card", 
    "callback_url": "https://reseller.example.com/payment/callback", 
    "hash": "<HMAC_SHA512_HASH>", 
    "json": "true", 
    "transaction_fee_bearer": "customer", 
    "split": { 
        "type": "flat", 
        "subaccounts": [ 
            { "sub_merchant_id": 1042, "share": 4200000 }, 
            { "subaccount": "1122334455", "bank_code": "057", "share": "Residue" }
        ] 
    }
}
```

Out of ₦50,000 (`5000000` kobo), ₦42,000 is automatically routed to sub-merchant\
`1042's` default settlementaccount and the remaining ₦8,000 (`Residue`) settles to the reseller's Zenith Bank account (`1122334455`).

Example B — Reseller-Owned Transaction Splitting to Multiple Sub-Merchants (`percentage`)

```json
{ 
    "amount": 5000000, 
    "email": "customer@example.com", 
    "payment_type": "card", 
    "callback_url": "https://reseller.example.com/payment/callback", 
    "hash": "<HMAC_SHA512_HASH>", 
    "json": "true", 
    "transaction_fee_bearer": "merchant", 
    "split": { 
        "type": "percentage", 
        "subaccounts": [ 
            { "sub_merchant_id": 1042, "share": 70, "fee_bearer": true }, 
            { "sub_merchant_id": 1088, "share": 20 }, 
            { "subaccount": "1122334455", "bank_code": "057", "share": "Residue" } 
        ] 
    }
}
```



#### Validation Rules & Constraints

{% columns %}
{% column valign="bottom" %}
Rule
{% endcolumn %}

{% column valign="bottom" %}
Details
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
Settlement Type
{% endcolumn %}

{% column width="66.66666666666667%" %}
Bulk Settlement only ( `settlement_type_id = 2` ). Individual settlement is not supported for reseller dynamic split transactions.
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
Monetary Units
{% endcolumn %}

{% column width="66.66666666666667%" %}
All monetary values (&#x20;`amount`&#x20;and&#x20;`flat`&#x20;shares) must be sent in&#x20;**kobo**&#x20;(`₦1 = 100` kobo).
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
`percentage` Split Totals
{% endcolumn %}

{% column width="66.66666666666667%" %}
Numeric shares must sum to&#x20;100%&#x20;(or&#x20;< 100%&#x20;when exactly one subaccountuses&#x20;"`share`": "`Residue`").
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
`flat`&#x20;Split Totals
{% endcolumn %}

{% column width="66.66666666666667%" %}
Numeric shares must sum to&#x20;amount&#x20;in kobo (or&#x20;< amount&#x20;when exactly one subaccount uses&#x20;"`share`": "`Residue`"&#x20;)
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
`Residue`&#x20;Limit
{% endcolumn %}

{% column width="66.66666666666667%" %}
At most&#x20;one&#x20;subaccount entry in&#x20;split.subaccounts&#x20;may specify&#x20;"`share`":"`Residue`".
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
Fee Bearer
{% endcolumn %}

{% column width="66.66666666666667%" %}
"`customer`"&#x20;(default): customer pays the transaction fee;&#x20;fee\_bearer&#x20;is optional.\
\
"`merchant`" :&#x20;mandatory&#x20;to set&#x20;"`fee_bearer`": true&#x20;on one subaccount whose share covers at least the expected transaction fee.
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="33.33333333333333%" %}
Sub-Merchant Ownership & Account Resolution
{% endcolumn %}

{% column width="66.66666666666667%" %}
Any `sub_merchant_id` (at the top level or inside `split.subaccounts`\[] ) mustbelong to the authenticating reseller. When used inside `split.subaccounts`\[] ,Moneta resolves that sub-merchant's default settlement subaccount automatically( `subaccount` and `bank_code` become optional for that entry).
{% endcolumn %}
{% endcolumns %}





