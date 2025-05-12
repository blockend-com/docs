# Configuration

**`initializeSDK(config)`**

Initialize the SDK with your credentials and configuration.

```typescript
initializeSDK({
  apiKey: string;
  integratorId: string;
  baseURL?: string;
  enableCache?: boolean;
  cacheTimeout:number;
});
```

To get config, use the `getConfig()` method.

```typescript
const config = getConfig();
```

To update config, use the `updateConfig()` method. You can update one or more parameters mentioned in the Configuration Parameters section.

```typescript
updateConfig({
  apiKey: "YOUR_API_KEY",
});
```

#### Configuration Parameters

**`apiKey` (required)**

* Your unique API key for authentication with the Blockend API
* Must be obtained through the Blockend platform
* Used to track API usage and enforce rate limits

**`integratorId` (required)**

* A unique identifier for your integration/application
* Used for analytics, tracking, and support purposes
* Helps identify your specific implementation when interacting with Blockend services

**`errorHandler` (optional)**

* A callback function to handle SDK errors globally
* Receives a `BlockendError` object with properties:
  * `message`: Description of the error
  * `code`: Error code (e.g., "CONFIGURATION\_ERROR", "NETWORK\_ERROR", "VALIDATION\_ERROR")
  * `data`: Additional error context (if available)
* Useful for centralized error handling, logging, and error reporting
* Example usage:

```typescript
initializeSDK({
  apiKey: "YOUR_API_KEY",
  integratorId: "YOUR_INTEGRATOR_ID",
  errorHandler: (error) => {
    console.error(
      `[Compass SDK Error] ${error.code}: ${error.message}`,
      error.data
    );
    // Custom error handling logic (e.g., reporting to monitoring service)
  },
});
```

**`baseURL` (optional)**

* The base URL endpoint for the Blockend API
* Defaults to `https://api2.blockend.com/v1`
* Can be modified for different environments (staging, testing, etc.)
* Should include the version prefix (`/v1`)

**`enableCache` (optional)**

* Boolean flag to enable/disable SDK's built-in caching mechanism
* When enabled, caches API responses to reduce network requests
* Particularly useful for frequently accessed data like token lists and chain information
* Defaults to `false` if not specified

**`cacheTimeout` (optional)**

* Duration in milliseconds for how long cached items should remain valid
* Only applies when `enableCache` is `true`
* Defaults to 1 hour (3600000 milliseconds)
* Can be adjusted based on your application's needs and data freshness requirements
