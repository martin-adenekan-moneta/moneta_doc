# Payment Example

Moneta payment endpoints has been summarized into simple classes to enable simple quick integration in using our payment gateway.

Remember to use the[ baseUrl ](./#payment-requirement)provided for the environment you are testing with.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
/**
 * MonetaPaymentClient - Implements the Moneta two-step authorization and transaction flow.
 * Uses the built-in 'fetch' API and 'crypto' module (Node) or SubtleCrypto (Browser) for hashing.
 * * NOTE: For Node.js, replace window.btoa with Buffer.from(...).toString('base64') and 
 * ensure you handle 'crypto' module imports for HMAC-SHA512. The below version is optimized 
 * for environments where native crypto/btoa exists (like a modern browser or Bun/Deno).
 */
class MonetaPaymentClient {
    // --- Configuration Constants ---
    static BASE_URI = 'BASE_URL'; // Replace with 'https://api-staging.moneta.ng/api/v2/' for staginag
    static TOKEN_ENDPOINT = 'generate-access-token';
    static INITIALIZE_ENDPOINT = 'transaction/initialize';
    static VERIFY_ENDPOINT = 'transaction/initialize'; // Based on your PHP code

    // Credentials
    #clientId;
    #clientSecret;
    #serviceKey;
    #macKey;

    // State
    #accessToken = null;
    #showMessage = true;

    /**
     * @param {string} clientId The Client ID.
     * @param {string} clientSecret The Client Secret.
     * @param {string} serviceKey The Service Key.
     * @param {string} macKey The MAC key used for generating the HMAC hash.
     */
    constructor(clientId, clientSecret, serviceKey, macKey) {
        this.#clientId = clientId;
        this.#clientSecret = clientSecret;
        this.#serviceKey = serviceKey;
        this.#macKey = macKey;
    }

    #echoMessage(message) {
        if (this.#showMessage) {
            console.log(message);
        }
    }

    /**
     * Generates the Base64 encoded string (client_id:client_secret:service_key).
     * @returns {string} The Base64 encoded token.
     */
    #generateBase64Token() {
        const concatenatedString = `${this.#clientId}:${this.#clientSecret}:${this.#serviceKey}`;
        // window.btoa is used for browser environments
        return btoa(concatenatedString);
    }

    /**
     * Generates the HMAC-SHA512 hash using the MAC key.
     * NOTE: This implementation is complex and requires the Node.js 'crypto' module or 
     * the browser's SubtleCrypto. The Node.js version is shown here.
     * * @param {string} email
     * @param {number} amount
     * @param {string} paymentType
     * @param {string} callbackUrl
     * @returns {string} The generated hash (Promise<string> in browser/Node).
     */
    #generateHash(email, amount, paymentType, callbackUrl) {
        // Concatenate values
        const myString = email + amount + paymentType + callbackUrl;
        
        // This part requires Node.js 'crypto' module for synchronous hashing
        try {
            const crypto = require('crypto');
            return crypto.createHmac('sha512', this.#macKey)
                         .update(myString)
                         .digest('hex');
        } catch (e) {
            // Fallback instruction for browser/environments where 'crypto' is not immediately available
            console.error("Warning: HMAC-SHA512 hashing requires 'crypto' module (Node.js) or a promise-based approach (browser/React).");
            throw new Error("Hashing dependency missing or not configured.");
        }
    }

    /**
     * Retrieves the X-Service-Token from the API using the X-Auth-Token header.
     * @returns {Promise<string>} The retrieved access token.
     */
    async #getAccessToken() {
        if (this.#accessToken) {
            return this.#accessToken;
        }

        this.#echoMessage("Retrieving new Access Token...");

        const base64Token = this.#generateBase64Token();

        try {
            const url = MonetaPaymentClient.BASE_URI + MonetaPaymentClient.TOKEN_ENDPOINT;
            
            const response = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'X-Auth-Token': base64Token, // Base64 token in custom header
                },
                // Body is intentionally empty JSON
                body: JSON.stringify({}), 
            });

            const data = await response.json();
            
            if (data.status === true && data.data) {
                this.#accessToken = data.data;
                this.#echoMessage("Access Token retrieved successfully.");
                return this.#accessToken;
            }

            throw new Error(`Failed to retrieve Access Token. API Response: ${JSON.stringify(data)}`);

        } catch (error) {
            this.#echoMessage(`Token Retrieval Error: ${error.message}`);
            throw error;
        }
    }

    /**
     * Initializes a payment transaction.
     * @param {string} email 
     * @param {number} amount 
     * @param {string} paymentType 
     * @param {string} callbackUrl 
     * @param {string} [useSplit=""] 
     * @param {boolean} [json=true] 
     * @returns {Promise<object>} The decoded API response.
     */
    async initializeTransaction(email, amount, paymentType, callbackUrl, useSplit = "", json = true) {
        const accessToken = await this.#getAccessToken();
        const hmacHash = this.#generateHash(email, amount, paymentType, callbackUrl);

        const transactionData = {
            "amount": amount,
            "email": email,
            "payment_type": paymentType,
            "channel": paymentType,
            "hash": hmacHash,
            "callback_url": callbackUrl,
            "use_split": useSplit,
            "gateway_scheme_used": 6,
            "json": json
        };

        try {
            this.#echoMessage("Initializing transaction...");
            const url = MonetaPaymentClient.BASE_URI + MonetaPaymentClient.INITIALIZE_ENDPOINT;

            const response = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'X-Service-Token': accessToken, // Retrieved token in service header
                },
                body: JSON.stringify(transactionData),
            });

            const data = await response.json();
            this.#echoMessage(`Transaction HTTP Status: ${response.status}`);
            return data;
            
        } catch (error) {
            this.#echoMessage(`Transaction Error: ${error.message}`);
            throw error;
        }
    }

    /**
     * Verifies a payment transaction.
     * @param {string} reference 
     * @returns {Promise<object>} The decoded API response.
     */
    async verifyTransaction(reference) {
        const accessToken = await this.#getAccessToken();

        try {
            this.#echoMessage("Verifying transaction...");
            const url = MonetaPaymentClient.BASE_URI + MonetaPaymentClient.VERIFY_ENDPOINT;

            const response = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'X-Service-Token': accessToken,
                },
                body: JSON.stringify({ 'reference': reference }),
            });

            const data = await response.json();
            this.#echoMessage(`Verification HTTP Status: ${response.status}`);
            return data;

        } catch (error) {
            this.#echoMessage(`Verification Error: ${error.message}`);
            throw error;
        }
    }
}
```
{% endtab %}

{% tab title="Express js" %}
```javascript
// This server MUST be run on the backend (e.g., localhost:3000)

const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');

// Import the secure client class from the previous file
const { MonetaPaymentClient } = require('./MonetaPaymentClient'); 

const app = express();
const PORT = 3000;

// --- SECRETS (Replace with actual environment variables) ---
const CLIENT_ID = 'YOUR_CLIENT_ID';
const CLIENT_SECRET = 'YOUR_CLIENT_SECRET';
const SERVICE_KEY = 'YOUR_SERVICE_KEY';
const MAC_KEY = 'YOUR_MAC_KEY';
// ------------------------------------------------------------------------

const monetaClient = new MonetaPaymentClient(CLIENT_ID, CLIENT_SECRET, SERVICE_KEY, MAC_KEY);

// Middleware
app.use(cors({ origin: 'http://localhost:5173' })); // Adjust origin for your React dev server
app.use(bodyParser.json());

// --- Endpoints ---

/**
 * Endpoint to initialize a transaction.
 * Request body must contain: email, amount, payment_type, callback_url
 */
app.post('/api/moneta/initialize', async (req, res) => {
    const { email, amount, payment_type, callback_url, use_split, json } = req.body;

    if (!email || !amount || !payment_type || !callback_url) {
        return res.status(400).json({ status: false, message: "Missing required fields." });
    }
    
    try {
        const result = await monetaClient.initializeTransaction(
            email,
            amount,
            payment_type,
            callback_url,
            use_split || "",
            json !== undefined ? json : true
        );
        return res.json(result);
    } catch (error) {
        console.error("Initialization API Error:", error.message);
        return res.status(500).json({ status: false, message: "Internal server error during transaction initialization." });
    }
});

/**
 * Endpoint to verify a transaction.
 * Request body must contain: reference
 */
app.post('/api/moneta/verify', async (req, res) => {
    const { reference } = req.body;

    if (!reference) {
        return res.status(400).json({ status: false, message: "Missing transaction reference." });
    }

    try {
        const result = await monetaClient.verifyTransaction(reference);
        return res.json(result);
    } catch (error) {
        console.error("Verification API Error:", error.message);
        return res.status(500).json({ status: false, message: "Internal server error during transaction verification." });
    }
});

// Start Server
app.listen(PORT, () => {
    console.log(`Moneta Payment Backend running on http://localhost:${PORT}`);
    console.log("WARNING: Ensure secrets are in environment variables.");
});
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php

use Exception;
use GuzzleHttp\Client;
use GuzzleHttp\Exception\GuzzleException;

class MonetaPaymentClient
{
    // --- Configuration Constants ---
    private const TOKEN_ENDPOINT = 'generate-access-token';
    private const INITIALIZE_ENDPOINT = 'transaction/initialize';

    // Credentials for Base64 token generation
    private string $clientId;
    private string $clientSecret;
    private string $serviceKey;

    // The MAC key for hash generation
    private string $macKey;

    // The actual token retrieved from the API, used in X-Service-Token header
    private ?string $accessToken = null;

    // Flag to control console output, useful for debugging or CLI environments
    private bool $showMessage = true;

    private Client $httpClient;

    /**
     * Constructor for MonetaPaymentClient.
     * @param string $clientId The Client ID.
     * @param string $clientSecret The Client Secret.
     * @param string $serviceKey The Service Key.
     * @param string $macKey The MAC key used for generating the HMAC hash.
     */
    public function __construct(string $clientId, string $clientSecret, string $serviceKey, string $macKey, bool $live = false)
    {
        $this->clientId = $clientId;
        $this->clientSecret = $clientSecret;
        $this->serviceKey = $serviceKey;
        $this->macKey = $macKey;

        if ($live) {
            $BASE_URI = 'https://app.moneta.ng/api/v2/';
        } else {
            $BASE_URI = 'https://api-staging.moneta.ng/api/v2/';
        }

        // Initialize Guzzle HTTP Client
        $this->httpClient = new Client([
            'base_uri' => $BASE_URI,
            'timeout' => 10.0,
            'headers' => [
                'Content-Type' => 'application/json',
            ],
        ]);
    }

    // --- Private Helper Methods ---

    /**
     * Echos a message to the console if $showMessage is true.
     * @param string $message The message to output.
     */
    private function echoMessage(string $message): void
    {
        if ($this->showMessage) {
            echo $message;
        }
    }

    /**
     * Generates the Base64 encoded string from client ID, secret, and service key
     * (e.g., base64_encode(client_id:client_secret:service_key)).
     * @return string The Base64 encoded token.
     */
    private function generateBase64Token(): string
    {
        // Concatenate the values with a colon separator
        $concatenated_string = implode(':', [$this->clientId, $this->clientSecret, $this->serviceKey]);

        // Apply Base64 encoding
        return base64_encode($concatenated_string);
    }

    /**
     * Retrieves the X-Service-Token from the API using the Base64 token in the X-Auth-Token header.
     * @return string The retrieved access token.
     * @throws GuzzleException|Exception If the request fails or token retrieval is unsuccessful.
     */
    private function getAccessToken(): string
    {
        // Return cached token if already retrieved
        if ($this->accessToken) {
            return $this->accessToken;
        }

        $this->echoMessage("Retrieving new Access Token...\n");

        // 1. Generate the required Base64 token for the X-Auth-Token header
        $base64Token = $this->generateBase64Token();

        try {
            // 2. Send the POST request to the token endpoint
            $response = $this->httpClient->post(self::TOKEN_ENDPOINT, [
                // Set the custom authentication header
                'headers' => [
                    'X-Auth-Token' => $base64Token,
                ],
                // Empty JSON body for the POST request
                'json' => [],
            ]);

            $body = $response->getBody()->getContents();
            $data = json_decode($body, true);

            // 3. Validate and set the access token
            if (isset($data['status']) && $data['status'] === true && isset($data['data'])) {
                $this->accessToken = $data['data'];
                $this->echoMessage("Access Token retrieved successfully.\n");
                return $this->accessToken;
            }

            // If status is not true or data is missing, throw an informative exception
            throw new \Exception("Failed to retrieve Access Token. API Response: " . $body);

        } catch (GuzzleException $e) {
            $this->echoMessage("Token Retrieval Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }

    /**
     * Generates the HMAC-SHA512 hash required for the transaction.
     * @param string $email
     * @param int $amount
     * @param string $paymentType
     * @param string $callbackUrl
     * @return string The generated hash.
     */
    private function generateHash(string $email, int $amount, string $paymentType, string $callbackUrl): string
    {
        // Concatenate the values exactly as required for hashing
        $my_string = $email . $amount . $paymentType . $callbackUrl;

        // Create HMAC-SHA512 using the MAC key
        return hash_hmac('sha512', $my_string, $this->macKey, false);
    }

    // --- Public Method ---

    /**
     * Initializes a payment transaction with the Moneta API using Guzzle.
     * @param string $email The customer's email.
     * @param int $amount The amount in the smallest currency unit (e.g., kobo).
     * @param string $paymentType The payment type (e.g., 'card').
     * @param string $callbackUrl The URL to redirect to after payment.
     * @param string $useSplit Optional split configuration string.
     * @param bool $json Optional flag, defaults to true.
     * @return array The decoded API response.
     * @throws GuzzleException|Exception If the request fails.
     */
    public function initializeTransaction(string $email, int|string $amount, string $paymentType, string $callbackUrl, string $useSplit = "", bool $json = true): array
    {
        // 1. Ensure the Access Token is retrieved and available
        $accessToken = $this->getAccessToken();

        // 2. Generate the required transaction hash
        $hmac_hash = $this->generateHash($email, $amount, $paymentType, $callbackUrl);

        // 3. Prepare the request body
        $transaction_data = [
            "amount" => $amount,
            "email" => $email,
            "payment_type" => $paymentType,
            "channel" => $paymentType,
            "hash" => $hmac_hash,
            "callback_url" => $callbackUrl,
            "use_split" => $useSplit,
            "gateway_scheme_used" => 6,
            "json" => $json
        ];

        // 4. Send the request using Guzzle
        try {
            $this->echoMessage("Initializing transaction...\n");
            $response = $this->httpClient->post(self::INITIALIZE_ENDPOINT, [
                'json' => $transaction_data,
                // Pass the retrieved access token as the X-Service-Token header
                'headers' => [
                    'X-Service-Token' => $accessToken,
                ],
            ]);

            $statusCode = $response->getStatusCode();
            $this->echoMessage("Transaction HTTP Status: " . $statusCode . "\n");

            $body = $response->getBody()->getContents();
            return json_decode($body, true);

        } catch (GuzzleException $e) {
            $this->echoMessage("Transaction Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }


    /** Verify a payment transaction with the Moneta API using Guzzle
     * @param $reference
     * @return array
     * @throws GuzzleException
     * @throws \JsonException
     */
    public function verifyTransaction($reference): array
    {

        try {
            // 1. Ensure the Access Token is retrieved and available
            $accessToken = $this->getAccessToken();

            $this->echoMessage("Verifying transaction...\n");
            $response = $this->httpClient->post(self::INITIALIZE_ENDPOINT, [
                'json' => ['reference' => $reference],
                // Pass the retrieved access token as the X-Service-Token header
                'headers' => [
                    'X-Service-Token' => $accessToken,
                ],
            ]);

            $statusCode = $response->getStatusCode();
            $this->echoMessage("Transaction HTTP Status: " . $statusCode . "\n");

            $body = $response->getBody()->getContents();
            return json_decode($body, true, 512, JSON_THROW_ON_ERROR);

        } catch (GuzzleException $e) {
            $this->echoMessage("Transaction Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }
}


namespace App\Classes;

use Exception;
use GuzzleHttp\Client;
use GuzzleHttp\Exception\GuzzleException;

class MonetaPaymentClient
{
    // --- Configuration Constants ---
    private const BASE_URI = 'BASE_URL';
    private const TOKEN_ENDPOINT = 'generate-access-token';
    private const INITIALIZE_ENDPOINT = 'transaction/initialize';

    // Credentials for Base64 token generation
    private string $clientId;
    private string $clientSecret;
    private string $serviceKey;

    // The MAC key for hash generation
    private string $macKey;

    // The actual token retrieved from the API, used in X-Service-Token header
    private ?string $accessToken = null;

    // Flag to control console output, useful for debugging or CLI environments
    private bool $showMessage = true;

    private Client $httpClient;

    /**
     * Constructor for MonetaPaymentClient.
     * @param string $clientId The Client ID.
     * @param string $clientSecret The Client Secret.
     * @param string $serviceKey The Service Key.
     * @param string $macKey The MAC key used for generating the HMAC hash.
     */
    public function __construct(string $clientId, string $clientSecret, string $serviceKey, string $macKey)
    {
        $this->clientId = $clientId;
        $this->clientSecret = $clientSecret;
        $this->serviceKey = $serviceKey;
        $this->macKey = $macKey;

        // Initialize Guzzle HTTP Client
        $this->httpClient = new Client([
            'base_uri' => self::BASE_URI,
            'timeout' => 10.0,
            'headers' => [
                'Content-Type' => 'application/json',
            ],
        ]);
    }

    // --- Private Helper Methods ---

    /**
     * Echos a message to the console if $showMessage is true.
     * @param string $message The message to output.
     */
    private function echoMessage(string $message): void
    {
        if ($this->showMessage) {
            echo $message;
        }
    }

    /**
     * Generates the Base64 encoded string from client ID, secret, and service key
     * (e.g., base64_encode(client_id:client_secret:service_key)).
     * @return string The Base64 encoded token.
     */
    private function generateBase64Token(): string
    {
        // Concatenate the values with a colon separator
        $concatenated_string = implode(':', [$this->clientId, $this->clientSecret, $this->serviceKey]);

        // Apply Base64 encoding
        return base64_encode($concatenated_string);
    }

    /**
     * Retrieves the X-Service-Token from the API using the Base64 token in the X-Auth-Token header.
     * @return string The retrieved access token.
     * @throws GuzzleException|Exception If the request fails or token retrieval is unsuccessful.
     */
    private function getAccessToken(): string
    {
        // Return cached token if already retrieved
        if ($this->accessToken) {
            return $this->accessToken;
        }

        $this->echoMessage("Retrieving new Access Token...\n");

        // 1. Generate the required Base64 token for the X-Auth-Token header
        $base64Token = $this->generateBase64Token();

        try {
            // 2. Send the POST request to the token endpoint
            $response = $this->httpClient->post(self::TOKEN_ENDPOINT, [
                // Set the custom authentication header
                'headers' => [
                    'X-Auth-Token' => $base64Token,
                ],
                // Empty JSON body for the POST request
                'json' => [],
            ]);

            $body = $response->getBody()->getContents();
            $data = json_decode($body, true);

            // 3. Validate and set the access token
            if (isset($data['status']) && $data['status'] === true && isset($data['data'])) {
                $this->accessToken = $data['data'];
                $this->echoMessage("Access Token retrieved successfully.\n");
                return $this->accessToken;
            }

            // If status is not true or data is missing, throw an informative exception
            throw new \Exception("Failed to retrieve Access Token. API Response: " . $body);

        } catch (GuzzleException $e) {
            $this->echoMessage("Token Retrieval Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }

    /**
     * Generates the HMAC-SHA512 hash required for the transaction.
     * @param string $email
     * @param int $amount
     * @param string $paymentType
     * @param string $callbackUrl
     * @return string The generated hash.
     */
    private function generateHash(string $email, int $amount, string $paymentType, string $callbackUrl): string
    {
        // Concatenate the values exactly as required for hashing
        $my_string = $email . $amount . $paymentType . $callbackUrl;

        // Create HMAC-SHA512 using the MAC key
        return hash_hmac('sha512', $my_string, $this->macKey, false);
    }

    // --- Public Method ---
    /**
     * Initializes a payment transaction with the Moneta API using Guzzle.
     * @param string $email The customer's email.
     * @param int $amount The amount in the smallest currency unit (e.g., kobo).
     * @param string $paymentType The payment type (e.g., 'card').
     * @param string $callbackUrl The URL to redirect to after payment.
     * @param string $useSplit Optional split configuration string.
     * @param bool $json Optional flag, defaults to true.
     * @return array The decoded API response.
     * @throws GuzzleException|Exception If the request fails.
     */
    public function initializeTransaction(string $email, int $amount, string $paymentType, string $callbackUrl, string $useSplit = "", bool $json = true): array
    {
        // 1. Ensure the Access Token is retrieved and available
        $accessToken = $this->getAccessToken();

        // 2. Generate the required transaction hash
        $hmac_hash = $this->generateHash($email, $amount, $paymentType, $callbackUrl);

        // 3. Prepare the request body
        $transaction_data = [
            "amount" => $amount,
            "email" => $email,
            "payment_type" => $paymentType,
            "channel" => $paymentType,
            "hash" => $hmac_hash,
            "callback_url" => $callbackUrl,
            "use_split" => $useSplit,
            "gateway_scheme_used" => 6,
            "json" => $json
        ];

        // 4. Send the request using Guzzle
        try {
            $this->echoMessage("Initializing transaction...\n");
            $response = $this->httpClient->post(self::INITIALIZE_ENDPOINT, [
                'json' => $transaction_data,
                // Pass the retrieved access token as the X-Service-Token header
                'headers' => [
                    'X-Service-Token' => $accessToken,
                ],
            ]);

            $statusCode = $response->getStatusCode();
            $this->echoMessage("Transaction HTTP Status: " . $statusCode . "\n");

            $body = $response->getBody()->getContents();
            return json_decode($body, true);

        } catch (GuzzleException $e) {
            $this->echoMessage("Transaction Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }


    /** Verify a payment transaction with the Moneta API using Guzzle
     * @param $reference
     * @return array
     * @throws GuzzleException
     * @throws \JsonException
     */
    public function verifyTransaction($reference): array
    {

        try {
            // 1. Ensure the Access Token is retrieved and available
            $accessToken = $this->getAccessToken();

            $this->echoMessage("Verifying transaction...\n");
            $response = $this->httpClient->post(self::INITIALIZE_ENDPOINT, [
                'json' => ['reference' => $reference],
                // Pass the retrieved access token as the X-Service-Token header
                'headers' => [
                    'X-Service-Token' => $accessToken,
                ],
            ]);

            $statusCode = $response->getStatusCode();
            $this->echoMessage("Transaction HTTP Status: " . $statusCode . "\n");

            $body = $response->getBody()->getContents();
            return json_decode($body, true, 512, JSON_THROW_ON_ERROR);

        } catch (GuzzleException $e) {
            $this->echoMessage("Transaction Guzzle Error: " . $e->getMessage() . "\n");
            // Re-throw the Guzzle exception
            throw $e;
        }
    }
}

```
{% endtab %}

{% tab title="Laravel" %}
```php
<?php

namespace App\Services;

use Exception;
use Illuminate\Http\Client\RequestException;
use Illuminate\Http\Client\PendingRequest;
use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\Log;

/**
 * MonetaPaymentClient interacts with the Moneta payment gateway using the
 * Laravel HTTP Client and configuration sourced from environment variables.
 */
class MonetaPaymentClient
{
    // --- Configuration Constants ---
    private const TOKEN_ENDPOINT = 'generate-access-token';
    private const INITIALIZE_ENDPOINT = 'transaction/initialize';
    private const VERIFY_ENDPOINT = 'transaction/verify';

    // Credentials properties - fetched from environment
    private string $clientId;
    private string $clientSecret;
    private string $serviceKey;
    private string $macKey;

    // The actual token retrieved from the API, used in X-Service-Token header
    private ?string $accessToken = null;

    // Base URI set in the constructor
    protected string $baseUri;

    /**
     * Constructor: Initializes credentials and determines the base URI from environment.
     * @throws Exception If critical environment variables are missing.
     */
    public function __construct()
    {
        $this->clientId = env('MONETA_CLIENT_ID', '');
        $this->clientSecret = env('MONETA_CLIENT_SECRET', '');
        $this->serviceKey = env('MONETA_SERVICE_KEY', '');
        $this->macKey = env('MONETA_MAC_KEY', '');

        if (!$this->clientId || !$this->macKey) {
            throw new Exception('Moneta API keys (CLIENT_ID, MAC_KEY) are missing in environment configuration.');
        }

        // Determine base URI
        $isLive = (bool) env('MONETA_LIVE', false);
        $this->baseUri = $isLive
            ? 'https://app.moneta.ng/api/v2/'
            : 'https://api-staging.moneta.ng/api/v2/';
    }

    /**
     * Creates a pre-configured HTTP client instance.
     * @return PendingRequest
     */
    protected function client(): PendingRequest
    {
        return Http::baseUrl($this->baseUri)
            ->withHeaders(['Content-Type' => 'application/json'])
            ->timeout(10.0)
            ->throw(); // Automatically throws RequestException on 4xx or 5xx status codes
    }

    /**
     * Generates the Base64 encoded string for the X-Auth-Token header.
     */
    private function generateBase64Token(): string
    {
        $concatenated_string = implode(':', [$this->clientId, $this->clientSecret, $this->serviceKey]);
        return base64_encode($concatenated_string);
    }

    /**
     * Retrieves the X-Service-Token from the API (cached if already available).
     * @return string The retrieved access token.
     * @throws RequestException|Exception If the request fails or token retrieval is unsuccessful.
     */
    private function getAccessToken(): string
    {
        if ($this->accessToken) {
            return $this->accessToken;
        }

        $base64Token = $this->generateBase64Token();

        try {
            $response = $this->client()
                ->withHeaders(['X-Auth-Token' => $base64Token])
                ->post(self::TOKEN_ENDPOINT);

            $data = $response->json();

            if (isset($data['status']) && $data['status'] === true && isset($data['data'])) {
                $this->accessToken = $data['data'];
                return $this->accessToken;
            }

            // If API responded 200 but status is false
            Log::error("Moneta Token Error: API response status failure.", ['response_body' => $response->body()]);
            throw new Exception("Failed to retrieve Access Token. API Response: " . $response->body());

        } catch (RequestException $e) {
            // Catches exceptions thrown by ->throw() for 4xx/5xx HTTP errors
            Log::error("Moneta Token HTTP Error: " . $e->getMessage(), ['exception' => $e]);
            throw $e;
        } catch (Exception $e) {
            // Catch all other exceptions (including the one thrown manually)
            throw $e;
        }
    }

    /**
     * Generates the HMAC-SHA512 hash required for the transaction.
     */
    private function generateHash(string $email, int $amount, string $paymentType, string $callbackUrl): string
    {
        $my_string = $email . $amount . $paymentType . $callbackUrl;
        return hash_hmac('sha512', $my_string, $this->macKey, false);
    }

    /**
     * Initializes a payment transaction with the Moneta API.
     *
     * @param string $email The customer's email.
     * @param int $amount The amount in the smallest currency unit (e.g., kobo).
     * @param string $paymentType The payment type (e.g., 'card').
     * @param string $callbackUrl The URL to redirect to after payment.
     * @param string $useSplit Optional split configuration string.
     * @return array The decoded API response.
     * @throws RequestException|Exception If the request fails.
     */
    public function initializeTransaction(string $email, int $amount, string $paymentType, string $callbackUrl, string $useSplit = ""): array
    {
        $accessToken = $this->getAccessToken();
        $hmac_hash = $this->generateHash($email, $amount, $paymentType, $callbackUrl);

        $transaction_data = [
            "amount" => $amount,
            "email" => $email,
            "payment_type" => $paymentType,
            "channel" => $paymentType,
            "hash" => $hmac_hash,
            "callback_url" => $callbackUrl,
            "use_split" => $useSplit,
            "gateway_scheme_used" => 6,
            "json" => true
        ];

        try {
            $response = $this->client()
                ->withHeaders(['X-Service-Token' => $accessToken])
                ->post(self::INITIALIZE_ENDPOINT, $transaction_data);

            return $response->json();

        } catch (RequestException $e) {
            Log::error("Moneta Transaction HTTP Error: " . $e->getMessage(), ['exception' => $e]);
            throw $e;
        }
    }

    /**
     * Verify a payment transaction with the Moneta API.
     * @param string $reference The transaction reference to verify.
     * @return array
     * @throws RequestException|Exception If the request fails.
     */
    public function verifyTransaction(string $reference): array
    {
        try {
            $accessToken = $this->getAccessToken();

            $response = $this->client()
                ->withHeaders(['X-Service-Token' => $accessToken])
                ->post(self::VERIFY_ENDPOINT, ['reference' => $reference]);

            // Since ->throw() is enabled on the client, we only catch the RequestException
            // and return the response body, which is guaranteed to be a valid 2xx response.
            return $response->json();

        } catch (RequestException $e) {
            Log::error("Moneta Verification HTTP Error: " . $e->getMessage(), ['exception' => $e]);
            throw $e;
        }
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import base64
import hashlib
import hmac
from typing import Optional, Dict, Any

class MonetaPaymentClient:
    """
    Implements the Moneta two-step authorization and transaction flow for Python/Django.
    """
    
    # --- Configuration Constants ---
    BASE_URI = 'BASE_URL'  # Replace with 'https://api-staging.moneta.ng/api/v2/'
    TOKEN_ENDPOINT = 'generate-access-token'
    INITIALIZE_ENDPOINT = 'transaction/initialize'
    VERIFY_ENDPOINT = 'transaction/initialize'

    def __init__(self, client_id: str, client_secret: str, service_key: str, mac_key: str):
        self.client_id = client_id
        self.client_secret = client_secret
        self.service_key = service_key
        self.mac_key = mac_key
        
        self.access_token: Optional[str] = None
        
    def _generate_base64_token(self) -> str:
        """Generates the Base64 encoded string (client_id:client_secret:service_key)."""
        concatenated_string = f"{self.client_id}:{self.client_secret}:{self.service_key}"
        encoded_bytes = concatenated_string.encode('utf-8')
        return base64.b64encode(encoded_bytes).decode('utf-8')

    def _get_access_token(self) -> str:
        """Retrieves the X-Service-Token from the API using the X-Auth-Token header."""
        if self.access_token:
            return self.access_token
        
        base64_token = self._generate_base64_token()
        url = self.BASE_URI + self.TOKEN_ENDPOINT
        
        headers = {
            'Content-Type': 'application/json',
            'X-Auth-Token': base64_token,
        }
        
        try:
            response = requests.post(url, headers=headers, json={}, timeout=10)
            response.raise_for_status()
            
            data = response.json()
            
            if data.get('status') is True and data.get('data'):
                self.access_token = data['data']
                return self.access_token
            
            raise Exception(f"Failed to retrieve Access Token. API Response: {response.text}")

        except requests.exceptions.RequestException as e:
            raise Exception(f"Token Retrieval Error: {e}") from e

    def _generate_hash(self, email: str, amount: int, payment_type: str, callback_url: str) -> str:
        """Generates the HMAC-SHA512 hash required for the transaction."""
        my_string = f"{email}{amount}{payment_type}{callback_url}"
        
        key_bytes = self.mac_key.encode('utf-8')
        message_bytes = my_string.encode('utf-8')
        
        hmac_hash = hmac.new(key_bytes, message_bytes, hashlib.sha512).hexdigest()
        return hmac_hash

    def initialize_transaction(self, email: str, amount: int, payment_type: str, callback_url: str, 
                               use_split: str = "", json_flag: bool = True) -> Dict[str, Any]:
        """Initializes a payment transaction."""
        access_token = self._get_access_token()
        hmac_hash = self._generate_hash(email, amount, payment_type, callback_url)

        transaction_data = {
            "amount": amount,
            "email": email,
            "payment_type": payment_type,
            "channel": payment_type,
            "hash": hmac_hash,
            "callback_url": callback_url,
            "use_split": use_split,
            "gateway_scheme_used": 6,
            "json": json_flag
        }
        
        url = self.BASE_URI + self.INITIALIZE_ENDPOINT
        headers = {
            'Content-Type': 'application/json',
            'X-Service-Token': access_token,
        }

        try:
            response = requests.post(url, headers=headers, json=transaction_data, timeout=10)
            response.raise_for_status()
            return response.json()

        except requests.exceptions.RequestException as e:
            raise Exception(f"Transaction Initialization Error: {e}") from e

    def verify_transaction(self, reference: str) -> Dict[str, Any]:
        """Verifies a payment transaction."""
        access_token = self._get_access_token()

        url = self.BASE_URI + self.VERIFY_ENDPOINT
        headers = {
            'Content-Type': 'application/json',
            'X-Service-Token': access_token,
        }

        try:
            response = requests.post(url, headers=headers, json={'reference': reference}, timeout=10)
            response.raise_for_status()
            return response.json()
            
        except requests.exceptions.RequestException as e:
            raise Exception(f"Verification Error: {e}") from e
```
{% endtab %}

{% tab title="Spring Boot" %}
```java
import org.springframework.stereotype.Service;
import java.util.Map;

// NOTE: This class assumes MonetaPaymentClient.java is available in the project structure.

/**
 * Spring Boot Service layer that encapsulates the MonetaPaymentClient.
 * This is where business logic (e.g., logging, validation, retry logic) would reside.
 */
@Service
public class MonetaPaymentService {

    // --- Placeholder Credentials (In a real application, use Spring @Value or Config class) ---
    private static final String CLIENT_ID = "spring-client-id";
    private static final String CLIENT_SECRET = "spring-client-secret";
    private static final String SERVICE_KEY = "spring-service-key";
    private static final String MAC_KEY = "spring-mac-key";
    // -----------------------------------------------------------------------------------------

    private final MonetaPaymentClient monetaClient;

    /**
     * Initializes the service by instantiating the core Moneta client.
     */
    public MonetaPaymentService() {
        // Instantiate the client from the shared file
        this.monetaClient = new MonetaPaymentClient(CLIENT_ID, CLIENT_SECRET, SERVICE_KEY, MAC_KEY);
    }

    /**
     * Initializes a transaction using the underlying client.
     * @return The API response map.
     * @throws Exception if the transaction fails or credentials are bad.
     */
    public Map<String, Object> initializePayment(
        String email, 
        int amount, 
        String paymentType, 
        String callbackUrl
    ) throws Exception {
        // Here you could add database logging before the call (e.g., save transaction attempt)
        System.out.println("Attempting to initialize payment for email: " + email);

        // Call the method in the MonetaPaymentClient.java file
        return monetaClient.initializeTransaction(
            email, 
            amount, 
            paymentType, 
            callbackUrl, 
            "", // useSplit
            true // json
        );
        // Here you would add database logging after the call (e.g., save response URL)
    }

    /**
     * Verifies a transaction using the underlying client.
     * @return The API response map.
     * @throws Exception if verification fails.
     */
    public Map<String, Object> verifyPayment(String reference) throws Exception {
        System.out.println("Attempting to verify reference: " + reference);
        
        // Call the method in the MonetaPaymentClient.java file
        return monetaClient.verifyTransaction(reference);
    }
}
```
{% endtab %}
{% endtabs %}
