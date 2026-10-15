---
title: Generate Access Token
---

{% code lineNumbers="true" %}
```php
<?php 
$email="";
$amount=""; 
$payment_type=""; 
$callback_url="";
$mac = "";
$mystring = $email . $amount. $payment_type . $callback_url;
$hash = hash_hmac('sha512', $mystring, $mac, false);
```
{% endcode %}

<pre><code><strong>curl -X POST 'https://staging.moneta.ng/api/generate-access-token' \
</strong><strong>-H 'X-Auth-Token: YOUR_HASH'
</strong></code></pre>
