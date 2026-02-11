---
description: Account Setup (Test & Live Mode)
icon: road-lock
---

# Get Started

We operate two environments to help you develop and test your integrations before going live (receiving real money).  You can switch between the environments by clicking on the test mode toggle on the top right hand side of your screen (Laptop mode) or in your main menu dropdown option.

<table><thead><tr><th width="245.127685546875">Base Url</th><th>Purpose</th></tr></thead><tbody><tr><td><a href="https://merchant.moneta.ng/">https://merchant.moneta.ng</a></td><td>Merchant dashboard</td></tr></tbody></table>

> Note: Each main page of this documentation contains urls representing **baseUrls for that particular service used on the page e.g the url above is for visiting the online merchant dashboard and other statistics.**

## Registration Steps

To use Moneta merchant services, visit the registration page and fill in your valid information.

### How to Register

To begin integrating with Moneta, You can start in Test Mode by doing the following:

1. **Create an account:** Sign up at the [Merchant Portal](https://merchant.moneta.ng/). As a developer, you can immediately access the testing environment to explore our API endpoints features.
2. **Verify Your Email:** Check your inbox for a verification link sent to your registered email address. You need to click this link to confirm and activate your account.
3.  _**Retrieve your API Tokens:**_ Once logged in, follow these steps to get your credentials:&#x20;

    1. Navigate to Settings > API Keys or go directly to the [API Settings Page](https://merchant.moneta.ng/account/profile-settings/api-keys).
    2. Scroll down to the Client Credentials section.
    3. Generate your unique Client ID and Client Secret.

    &#x20;

{% hint style="info" %}
<mark style="color:$warning;">Important Note</mark>: API keys are environment-specific. Your credentials and additional service keys will also be sent to your registered email address for your records.
{% endhint %}

### IP WhiteList

Moneta implements a very <mark style="color:$danger;">S</mark><mark style="color:$danger;">**trict IP Monitoring**</mark> policy to ensure your transactions are protected from third party hijack. Before a successful response can be gotten from our servers, you are required to include your current IP address on your **IP whitelist** in your settings window. This will prevent unauthorized locations from sending payment request using your credentials even when it is exposed.

Visit your dashboard settings and add your current IP in the list of allowed IP addresses to allow you access to API calls from our servers.
