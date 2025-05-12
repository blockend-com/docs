---
icon: shield
---

# Authentication

### Rate Limiting <a href="#rate-limiting" id="rate-limiting"></a>

Currently we only allow authenticated requests to our system, our APIs will be generally available soon.\
Rate limits with authentication: 200 requests per minute

### :handshake:Request for an API Key:

**Drop an email at**

&#x20;:e-mail: [sohail@blockend.com](mailto:sohail@blockend.com)

\
**Or Reach out on TG at**

&#x20;:mailbox\_with\_mail:  [https://t.me/StrayNomad](https://t.me/StrayNomad)

### :shield:Authentication <a href="#authentication" id="authentication"></a>

While Compass API can be accessed without authentication, it is recommended to authenticate to get access to more features and higher rate limits.

To authenticate, you need to pass in the api-key in the request header.

```
const headers = {
    'x-api-key': 'YOUR_API_KEY'
};

fetch('https://api2.blockend.com/v1/tokens', { headers })
    .then(response => response.json())
    .then(data => console.log(data));
```

```
curl -X GET "https://api2.blockend.com/v1/tokens" \
    -H "x-api-key: YOUR_API_KEY"
```
