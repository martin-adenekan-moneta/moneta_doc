---
description: Moneta Payment Gateway
icon: box-dollar
---

# Payments

### Overview

You are expected to complete your account setup by adding a **settlement bank account** for your payout.

To setup the account your money will be sent to when using our system, please visit the **Account** tab on your settings page and add your default settlement account.&#x20;

Visit: [https://merchant.moneta.ng/account/profile-settings/bank-accounts](https://merchant.moneta.ng/account/profile-settings/bank-accounts)

Next visit the API keys and settings page to get your API credentials.

&#x20;Once you have your **Client ID and Client Secret, Service Keys** (from either the Staging or Production environment delivered to your email), and **MAC Key** on your api keys page,  you can start testing or recieving payment .

Base URL for Paymnent

<table><thead><tr><th width="220.61968994140625">Payment Environment</th><th width="303.127685546875">Base Url</th><th>Purpose</th></tr></thead><tbody><tr><td>Staging</td><td>https://api-staging.moneta.ng/api</td><td>Development &#x26; Testing</td></tr><tr><td>Production</td><td>https://api.moneta.ng/api/v2</td><td>Live Transactions (Real Money)</td></tr></tbody></table>

#### Authentication & Security Requirement

Before you can access the payment APIs, you are expected to have a service token. To generate a service token (also known as service key), create a base64 hash string  using your client\_id, client\_secret, and **service keys** for the service you want to access e.g payment _<mark style="color:$info;">**(Check the mail sent to you after registration and copy the Service Key for Moneta Payment Gateway)**</mark>_&#x20;

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Variables
const client_id = "[CLIENT_ID]";
const client_secret = "[CLIENT_SECRET]";
const service_key = "[SERVICE_KEY]";

function encodeToBase64(client_id, client_secret, service_key) {
    // 1. Concatenate the values using a colon
    const mystring = `${client_id}:${client_secret}:${service_key}`;
    
    // 2. Create a Buffer from the string (specifying UTF-8 encoding)
    const buffer = Buffer.from(mystring, 'utf8');
    
    // 3. Base64 encode the buffer and return the resulting string
    // The 'base64' argument specifies the encoding for the output string
    return buffer.toString('base64');
}

const token = encodeToBase64(client_id, client_secret, service_key);
console.log(token);
```
{% endtab %}

{% tab title="PHP" %}
```php
$client_id = "[CLIENT_ID]";
$client_secret = "[CLIENT_SECRET]";
$service_key = "[SERVICE_KEY]";

function encode_to_base64(...$values){
    return base64_encode(implode(':',$values));
}
$token = encode_to_base64($client_id,$client_secret,$service_key);
echo $token;
```
{% endtab %}

{% tab title="Python" %}
```python
import base64

# Variables
client_id = "[CLIENT_ID]"
client_secret = "[CLIENT_SECRET]"
service_key = "[SERVICE_KEY]"

def encode_to_base64(client_id, client_secret, service_key):
    # 1. Concatenate the values using a colon
    mystring = f"{client_id}:{client_secret}:{service_key}"
    
    # 2. Convert the string to bytes (UTF-8)
    string_bytes = mystring.encode('utf-8')
    
    # 3. Base64 encode the bytes
    encoded_bytes = base64.b64encode(string_bytes)
    
    # 4. Convert the encoded bytes back to a string and return
    return encoded_bytes.decode('utf-8')

token = encode_to_base64(client_id, client_secret, service_key)
print(token)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Base64;
import java.nio.charset.StandardCharsets;

public class Base64Encoder {

    // Variables
    private static final String CLIENT_ID = "[CLIENT_ID]";
    private static final String CLIENT_SECRET = "[CLIENT_SECRET]";
    private static final String SERVICE_KEY = "[SERVICE_KEY]";

    public static String encodeToBase64(String clientId, String clientSecret, String serviceKey) {
        // 1. Concatenate the values using a colon
        String myString = String.join(":", clientId, clientSecret, serviceKey);
        
        // 2. Convert the string to bytes using UTF-8 encoding
        byte[] stringBytes = myString.getBytes(StandardCharsets.UTF_8);
        
        // 3. Base64 encode the bytes and convert the resulting byte array back to a string
        String encodedString = Base64.getEncoder().encodeToString(stringBytes);
        
        return encodedString;
    }

    public static void main(String[] args) {
        String token = encodeToBase64(CLIENT_ID, CLIENT_SECRET, SERVICE_KEY);
        System.out.println(token);
    }
}
```
{% endtab %}

{% tab title="C# (.Net)" %}
```csharp
using System;
using System.Text;

public class Base64Encoder
{
    // Variables
    private const string ClientId = "[CLIENT_ID]";
    private const string ClientSecret = "[CLIENT_SECRET]";
    private const string ServiceKey = "[SERVICE_KEY]";

    public static string EncodeToBase64(string clientId, string clientSecret, string serviceKey)
    {
        // 1. Concatenate the values using a colon
        string myString = $"{clientId}:{clientSecret}:{serviceKey}";
        
        // 2. Convert the string to a byte array using UTF-8 encoding
        byte[] stringBytes = Encoding.UTF8.GetBytes(myString);
        
        // 3. Base64 encode the byte array and return the string
        return Convert.ToBase64String(stringBytes);
    }

    public static void Main()
    {
        string token = EncodeToBase64(ClientId, ClientSecret, ServiceKey);
        Console.WriteLine(token);
    }
}
```
{% endtab %}
{% endtabs %}

Once you have generated your base64 encoded token, add it to as `X-Auth-Token` header to generate your access token from the right endpoint.

{% hint style="warning" %}
Please ensure that all requests to our API services are coming from your backend server to protect your sensitive credentials.
{% endhint %}

#### Service Access Token Generation

<mark style="color:green;">`POST`</mark> `{{baseUrl}}/generate-access-token`

\<This endpoint generates your access token for payment API Services>

**Headers**



| Name         | Value                       |
| ------------ | --------------------------- |
| Content-Type | `application/json`          |
| X-Auth-Token | `<generated service token>` |

**Body  :** None

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
 "status": true,
    "data": "3|bLBcbEd3oztpXyg9jSrK7HjWu2QuyeDpxfTKcQg9a0494882"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
  "responseErrorCode": "08",
    "status": "error",
    "message": "Service not available"
}
```
{% endtab %}
{% endtabs %}

***





Quick Links

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="image">Cover image</th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h4>Settlement Account</h4></td><td><a href="https://images.unsplash.com/photo-1626266061368-46a8f578ddd6?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxhY2NvdW50fGVufDB8fHx8MTc3MDA0MTU2M3ww&#x26;ixlib=rb-4.1.0&#x26;q=85">https://images.unsplash.com/photo-1626266061368-46a8f578ddd6?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxhY2NvdW50fGVufDB8fHx8MTc3MDA0MTU2M3ww&#x26;ixlib=rb-4.1.0&#x26;q=85</a></td><td><a href="bank-accounts.md#create-sub-account">#create-sub-account</a></td></tr><tr><td><h4>Transaction Split</h4></td><td><a href="https://images.unsplash.com/photo-1644952354935-0bc0d25a9996?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw3fHx0cmFuc2FjdGlvbnxlbnwwfHx8fDE3NzAwNDE4Mjd8MA&#x26;ixlib=rb-4.1.0&#x26;q=85">https://images.unsplash.com/photo-1644952354935-0bc0d25a9996?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw3fHx0cmFuc2FjdGlvbnxlbnwwfHx8fDE3NzAwNDE4Mjd8MA&#x26;ixlib=rb-4.1.0&#x26;q=85</a></td><td><a href="transaction-split.md">transaction-split.md</a></td></tr><tr><td><h4>Accept Payment</h4></td><td><a href="https://images.unsplash.com/photo-1556742031-c6961e8560b0?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MXwxfHNlYXJjaHwzfHxwYXl8ZW58MHx8fHwxNzY0MjQ2OTAyfDA&#x26;ixlib=rb-4.1.0&#x26;q=85">https://images.unsplash.com/photo-1556742031-c6961e8560b0?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MXwxfHNlYXJjaHwzfHxwYXl8ZW58MHx8fHwxNzY0MjQ2OTAyfDA&#x26;ixlib=rb-4.1.0&#x26;q=85</a></td><td><a href="accept-payment.md">accept-payment.md</a></td></tr><tr><td><h4>Verify Payment</h4></td><td><a href="https://images.unsplash.com/photo-1563013544-824ae1b704d3?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw5fHxwYXl8ZW58MHx8fHwxNzY0MjQ2OTAyfDA&#x26;ixlib=rb-4.1.0&#x26;q=85">https://images.unsplash.com/photo-1563013544-824ae1b704d3?crop=entropy&#x26;cs=srgb&#x26;fm=jpg&#x26;ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw5fHxwYXl8ZW58MHx8fHwxNzY0MjQ2OTAyfDA&#x26;ixlib=rb-4.1.0&#x26;q=85</a></td><td><a href="verify-payment.md">verify-payment.md</a></td></tr></tbody></table>
