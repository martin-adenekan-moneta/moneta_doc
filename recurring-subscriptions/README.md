---
description: Moneta NiBSS
icon: repeat
---

# Recurring Subscriptions

## Initialization

This endpoint helps our merchants to create recuring billing and subscription systems for their customers.

To use this service, you need to generate a new service token using the [example given here](../payments/#service-access-token-generation)

Base URL for Payment

<table><thead><tr><th width="220.61968994140625">Payment Environment</th><th width="303.127685546875">Base Url</th><th>Purpose</th></tr></thead><tbody><tr><td>Staging</td><td>https://api-staging.moneta.ng/api</td><td>Development &#x26; Testing</td></tr><tr><td>Production</td><td>https://api.moneta.ng/api</td><td>Live Transactions (Real Money)</td></tr></tbody></table>

Use your service token as X-Service-Token header for your request to this endpoint.
