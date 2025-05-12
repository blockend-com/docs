---
icon: circle-chevron-right
---

# Getting Started



### Base URL:

```
https://api2.blockend.com/v1/
```

All type definitions can be found [here](type-definations.md)

### Core Transaction Flow <a href="#core-transaction-flow" id="core-transaction-flow"></a>

**1. Fetching quotes**

User flow begins with fetching quotes, which returns a list of quotes for the given input and output assets, along with the steps involved in the transaction. Quotes are by default sorted via best output and contains a scores object and tags to determine fastest/cheapest/best output routes.

{% content-ref url="fetching-quotes.md" %}
[fetching-quotes.md](fetching-quotes.md)
{% endcontent-ref %}

**2. Creating a transaction**

After fetching quotes, you can create a transaction using the selected quote by passing in the `routeId` of the quote.

{% content-ref url="create-transaction.md" %}
[create-transaction.md](create-transaction.md)
{% endcontent-ref %}

**3. Getting transaction data to execute**

CommentCall this api with `routeId` and `stepId` of individual steps to get the transaction data to execute.

{% content-ref url="get-raw-transaction-to-execute.md" %}
[get-raw-transaction-to-execute.md](get-raw-transaction-to-execute.md)
{% endcontent-ref %}

**4. Check status of a transaction**

After user signs and submits the transaction on chain, check the status of the transaction by passing in the signature hash of the transaction.

{% content-ref url="check-transaction-status.md" %}
[check-transaction-status.md](check-transaction-status.md)
{% endcontent-ref %}

**5. Using WebSockets for realtime updates (in active development)**

You can also check the status of a transaction using WebSockets. This can provide faster and almost realtime updates on the status of a transaction.

> Note: this feature is in active development and will be generally available soon. Contact us to get access to this feature.

**6. Single API for simple flow execution (in active development)**\
\
A simple 1 step endpoint is being worked on where a request directly gives you a single quote in response along with array of transaction data to execute to complete the transaction

> Note: this feature is in active development and will be generally available soon. Contact us to get access to this feature.

### Meta Endpoints <a href="#meta-endpoints-1" id="meta-endpoints-1"></a>

#### **​**[**Tokens**](get-supported-chains-and-tokens.md)**​**

Get a list of supported tokens and their details.

```
curl -X GET "https://api2.blockend.com/v1/tokens"
```

#### **​**[**Chains**](../supported-chains.md)**​**

Get a list of supported chains and their details.

```
curl -X GET "https://api2.blockend.com/v1/chains"
```
