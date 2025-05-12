---
icon: bridge
---

# EVM to SOL Bridge

This guide demonstrates how to execute a token bridge from EVM-compatible chains to Solana using the Compass API. The example covers the complete workflow from connecting a wallet to monitoring transaction status.

### Overview

The implementation follows these key steps:

1. Wallet Connection
2. Quote Retrieval
3. Transaction Creation
4. Step-by-Step Execution
5. Status Monitoring

### Implementation Details

#### Prerequisites

* MetaMask or any EVM-compatible wallet
* Integrator ID from Compass
* ethers.js library

### API Endpoints

The example uses the following endpoints:

**`/quotes`**

This endpoint fetches quotes from available liquidity sources and responds with quotes that are sorted by best output amount by default. The first item in the quotes array is the recommended quote.

**Request Parameters**

```javascript
let requestParams = {
  fromChainId: "137", // Source blockchain network ID (Polygon), for chain IDs reference: "/chains" api endpoint
  toChainId: "sol", // Destination blockchain network ID (Solana), for chain IDs reference: "/chains" api endpoint
  fromAssetAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee", // Source token contract address, for token addresses reference: "/tokens" api endpoint
  toAssetAddress: "So11111111111111111111111111111111111111112", // Destination token contract address, for token addresses reference: "/tokens" api endpoint
  inputAmountDisplay: "10", // Amount of source token to bridge, this is the amount that will be transferred from the source token chain to the destination token chain, typically received as input from the user
  userWalletAddress: "0x..", // User's wallet address
  recipient: "4s..", // Recipient's wallet address
};
```

**Example Request**

```
GET /quotes?fromChainId=137&toChainId=sol&fromAssetAddress=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee&toAssetAddress=So11111111111111111111111111111111111111112&inputAmountDisplay=10&userWalletAddress=0x1...&recipient=4s...
```

**Example Response**

```javascript
let quoteResponse = {
  status: "success",
  data: {
    quotes: [
      {
        routeId: "bae216ad-de5f-4a28-bf49-8b5d80cd722a",
        from: {
          networkType: "evm",
          address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          chainId: "137",
          blockchain: "Polygon",
          decimals: 18,
          name: "Matic",
          symbol: "MATIC",
          image:
            "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
          lastPrice: 0.412124,
          isEnabled: true,
          isFlagged: false,
          isNative: true,
          isPopular: true,
        },
        to: {
          networkType: "sol",
          address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
          chainId: "sol",
          blockchain: "Solana",
          decimals: 6,
          name: "Tether",
          symbol: "USDT",
          image:
            "https://assets.coingecko.com/coins/images/325/small/Tether.png",
          lastPrice: 0.999919,
          isEnabled: true,
          isFlagged: false,
          isNative: false,
          isPopular: false,
        },
        steps: [
          {
            stepId: "bae216ad-de5f-4a28-bf49-8b5d80cd722a:0",
            stepType: "bridge",
            protocolsUsed: ["Allbridge"],
            from: {
              networkType: "evm",
              address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 18,
              name: "Matic",
              symbol: "MATIC",
              image:
                "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
              lastPrice: 0.412124,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            to: {
              networkType: "sol",
              address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.999919,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            inputAmount: "10000000000000000000",
            outputAmount: "2208730",
            fee: [
              {
                type: "NETWORK",
                token: {
                  networkType: "evm",
                  address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                  chainId: "137",
                  blockchain: "Polygon",
                  decimals: 18,
                  name: "Matic",
                  symbol: "MATIC",
                  image:
                    "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
                  lastPrice: 0.412124,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_SOURCE_WALLET",
                amountInToken: "13350000089000000",
                amountInUSD: "0.0055",
              },
              {
                type: "PROVIDER",
                token: {
                  networkType: "evm",
                  address: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174",
                  chainId: "137",
                  blockchain: "Polygon",
                  decimals: 6,
                  name: "USDC",
                  symbol: "USDC.e",
                  image:
                    "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                  lastPrice: 1,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: false,
                  isPopular: false,
                },
                source: "FROM_OUTPUT_AMOUNT",
                amountInToken: "2127239",
                amountInUSD: 2.1271999999999998,
              },
              {
                type: "PROVIDER",
                token: {
                  networkType: "evm",
                  address: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174",
                  chainId: "137",
                  blockchain: "Polygon",
                  decimals: 6,
                  name: "USDC",
                  symbol: "USDC.e",
                  image:
                    "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                  lastPrice: 1,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: false,
                  isPopular: false,
                },
                source: "FROM_OUTPUT_AMOUNT",
                amountInToken: "13008",
                amountInUSD: "0.0130",
              },
            ],
            estimatedTimeInSeconds: 765,
          },
        ],
        fee: [
          {
            type: "NETWORK",
            token: {
              networkType: "evm",
              address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 18,
              name: "Matic",
              symbol: "MATIC",
              image:
                "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
              lastPrice: 0.412124,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_SOURCE_WALLET",
            amountInToken: "13350000089000000",
            amountInUSD: "0.0055",
          },
          {
            type: "PROVIDER",
            token: {
              networkType: "evm",
              address: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 6,
              name: "USDC",
              symbol: "USDC.e",
              image:
                "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
              lastPrice: 1,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            source: "FROM_OUTPUT_AMOUNT",
            amountInToken: "2127239",
            amountInUSD: 2.1271999999999998,
          },
          {
            type: "BLOCKEND",
            source: "FROM_OUTPUT_AMOUNT",
            token: {
              networkType: "sol",
              address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.999919,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            amountInToken: "0",
            amountInUSD: "0",
          },
        ],
        provider: "lifi",
        providerDetails: {
          id: "lifi",
          name: "LI.FI",
          logoUrl:
            "https://raw.githubusercontent.com/blockend-com/docs-v1/refs/heads/main/resources/png/lifi.png",
          websiteUrl: "https://li.fi/",
        },
        protocolsUsed: ["Allbridge"],
        inputAmount: "10000000000000000000",
        inputAmountDisplay: "10",
        outputAmount: "2224279",
        outputAmountDisplay: "2.224279",
        minOutputAmount: "2.224279",
        slippage: 4603,
        userWalletAddress: "0x1b1E919E51a1592Dce70a4FD74107941109B8235",
        recipient: "4sd55KCv7RFcc14goe3yJtsBTb9XMFjR5GAnemaDdYTd",
        createdAt: 1738063547633,
        deadline: 60,
        estimatedTimeInSeconds: 765,
        requestId: "",
        score: {
          outputScore: 1,
          speedScore: 0.00130718954248366,
          feeScore: 1,
          slipparageScore: 1,
          stepScore: 1,
          outputDiffPercent: 0,
        },
        tags: ["BEST_OUTPUT", "CHEAP", "LOW_SLIPPAGE"],
      },
      {
        routeId: "01JJPAJ2FS477PTT6EW97H52G6",
        from: {
          networkType: "evm",
          address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          chainId: "137",
          blockchain: "Polygon",
          decimals: 18,
          name: "Matic",
          symbol: "MATIC",
          image:
            "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
          lastPrice: 0.412124,
          isEnabled: true,
          isFlagged: false,
          isNative: true,
          isPopular: true,
        },
        to: {
          networkType: "sol",
          address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
          chainId: "sol",
          blockchain: "Solana",
          decimals: 6,
          name: "Tether",
          symbol: "USDT",
          image:
            "https://assets.coingecko.com/coins/images/325/small/Tether.png",
          lastPrice: 0.999919,
          isEnabled: true,
          isFlagged: false,
          isNative: false,
          isPopular: false,
        },
        steps: [
          {
            stepId: "01JJPAJ2FS5AYXGCPYB817R7JP",
            stepType: "bridge",
            protocolsUsed: ["deBridge"],
            provider: "dln",
            from: {
              networkType: "evm",
              address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 18,
              name: "Matic",
              symbol: "MATIC",
              image:
                "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
              lastPrice: 0.412124,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            to: {
              networkType: "sol",
              address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.999919,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            fee: [
              {
                type: "NETWORK",
                token: {
                  networkType: "evm",
                  address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                  chainId: "137",
                  blockchain: "Polygon",
                  decimals: 18,
                  name: "Matic",
                  symbol: "MATIC",
                  image:
                    "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
                  lastPrice: 0.412124,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_SOURCE_WALLET",
                amountInToken: "17802000070200000",
                amountInUSD: "0.0073366314769311046",
              },
              {
                type: "PROVIDER",
                token: {
                  networkType: "evm",
                  address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                  chainId: "137",
                  blockchain: "Polygon",
                  decimals: 18,
                  name: "Matic",
                  symbol: "MATIC",
                  image:
                    "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
                  lastPrice: 0.412124,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_SOURCE_WALLET",
                amountInToken: "500000000000000000",
                amountInUSD: "0.206062",
              },
              {
                type: "BLOCKEND",
                source: "FROM_OUTPUT_AMOUNT",
                token: {
                  networkType: "sol",
                  address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
                  chainId: "sol",
                  blockchain: "Solana",
                  decimals: 6,
                  name: "Tether",
                  symbol: "USDT",
                  image:
                    "https://assets.coingecko.com/coins/images/325/small/Tether.png",
                  lastPrice: 0.999919,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: false,
                  isPopular: false,
                },
                amountInToken: "0",
                amountInUSD: "0",
              },
            ],
            inputAmount: "10000000000000000000",
            outputAmount: "1807631",
            estimatedTimeInSeconds: 1,
          },
        ],
        fee: [
          {
            type: "NETWORK",
            token: {
              networkType: "evm",
              address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 18,
              name: "Matic",
              symbol: "MATIC",
              image:
                "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
              lastPrice: 0.412124,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_SOURCE_WALLET",
            amountInToken: "17802000070200000",
            amountInUSD: "0.0073366314769311046",
          },
          {
            type: "PROVIDER",
            token: {
              networkType: "evm",
              address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 18,
              name: "Matic",
              symbol: "MATIC",
              image:
                "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
              lastPrice: 0.412124,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_SOURCE_WALLET",
            amountInToken: "500000000000000000",
            amountInUSD: "0.206062",
          },
          {
            type: "BLOCKEND",
            source: "FROM_OUTPUT_AMOUNT",
            token: {
              networkType: "sol",
              address: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.999919,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            amountInToken: "0",
            amountInUSD: "0",
          },
        ],
        provider: "dln",
        providerDetails: {
          id: "dln",
          name: "DLN",
          logoUrl:
            "https://raw.githubusercontent.com/blockend-com/docs-v1/refs/heads/main/resources/png/dln.png",
          websiteUrl: "https://dln.trade/",
        },
        protocolsUsed: ["deBridge"],
        inputAmount: "10000000000000000000",
        inputAmountDisplay: "10",
        outputAmount: "1807631",
        outputAmountDisplay: "1.807631",
        minOutputAmount: "1.807631",
        minOutputAmountDisplay: "1.807631",
        slippage: 5614,
        userWalletAddress: "0x1b1E919E51a1592Dce70a4FD74107941109B8235",
        recipient: "4sd55KCv7RFcc14goe3yJtsBTb9XMFjR5GAnemaDdYTd",
        createdAt: 1738063546873,
        deadline: 30,
        estimatedTimeInSeconds: 1,
        requestId: "",
        score: {
          outputScore: 0.8126817723855685,
          speedScore: 1,
          feeScore: 0.7496628414952957,
          slipparageScore: 0.8199144994656217,
          stepScore: 1,
          outputDiffPercent: 0.20667524820742533,
        },
        tags: ["BEST", "FAST"],
      },
    ],
  },
};
```

[View full quotes response](../compass-api/api-reference/fetching-quotes.md)

**`/createTx`**

This endpoint fetches the transaction steps for the selected quote. A transaction can have multiple steps, e.g., token approval, swap/bridge, etc. Since this is an EVM to Solana bridge example, the transaction steps can include token approval and bridge, or only bridge if the source token is already approved. The response includes steps that need to be executed sequentially. Each step includes a step ID and step type (approval, swap, bridge, etc.).

**Request Parameters**

```javascript
let requestParams = {
  routeId: quoteResponse.data.quotes[0].routeId, // routeId is available in the selected quote response
};
```

**Example Request**

```
GET /createTx?routeId=a1b2c3d4-e5f6-g7h8-i9j0
```

**Example Response**

```javascript
let createTxResponse = {
  status: "success",
  data: {
    steps: [
      {
        stepId: "step_12345",
        stepType: "approval",
      },
      {
        stepId: "step_67890",
        stepType: "bridge",
      },
    ],
  },
};
```

[View full createTx API response](../compass-api/api-reference/create-transaction.md)

**`/nextTx`**

This endpoint fetches the transaction data that needs to be executed for the selected step. The transaction data includes the transaction parameters for the step. Since this is an EVM to Solana bridge example, the transaction data includes the txnEvm object field in the transaction data.

Note: The transaction data is specific to the step type. For example, if the step type is brige, the transaction data includes the bridge parameters. If the step type is token approval, the transaction data includes the token approval parameters. Moreover, the next step data will be available only if the previous step is executed successfully.

**Request Parameters**

```javascript
let requestParams = {
  routeId: quoteResponse.data.quotes[0].routeId, // routeId is available in the selected quote response
  stepId: createTxResponse.data.steps[0].stepId, // stepId is available in the createTx steps[] response
};
```

**Example Request**

```
GET /nextTx?routeId=a1b2c3d4-e5f6-g7h8-i9j0&stepId=step_12345
```

**Example Response**

```javascript
let nextTxResponse = {
  status: "success",
  data: {
    txnData: {
      txnEvm: {
        from: "0x1b1E919E51a1592Dce70a4FD74107941109B8235",
        to: "0xcontractAddress",
        data: "0x...",
        value: "0",
        chainId: 137,
        gasLimit: "300000",
      },
    },
  },
};
```

[View full nextTx API response](../compass-api/api-reference/get-raw-transaction-to-execute.md)

**`/status`**

This endpoint fetches the transaction status for the selected step once the transaction step is submitted to the blockchain. The transaction status includes: the status of the transaction, the source transaction hash, the source transaction URL, the destination transaction hash, the destination transaction URL, the output amount received, and the output token details.

[View full status API documentation](../compass-api/api-reference/check-transaction-status.md)

#### Transaction Status Codes

* `in-progress`: Transaction is pending
* `success`: Transaction completed successfully
* `partial-success`: If output amount token is different from the destination token, the transaction is partial success.
* `failed`: Transaction failed

**Request Parameters**

```javascript
let requestParams = {
  routeId: quoteResponse.data.quotes[0].routeId, // routeId is available in the selected quote response
  stepId: createTxResponse.data.steps[0].stepId, // stepId is available in the createTx steps[] response
  txnHash: nextTxResponse.data.txnData.txnEvm.hash, // pass the txnHash that is received from the wallet provider once the transaction is signed successfully by the user
};
```

**Example Request**

```
GET /status?routeId=a1b2c3d4-e5f6-g7h8-i9j0&stepId=step_12345&txnHash=0x1234...abcd
```

**Example Response**

```javascript
let statusResponse = {
  status: "success",
  data: {
    status: "success",
    srcTxnHash: "0x1234...abcd",
    srcTxnUrl: "https://polygonscan.com/tx/0x1234...abcd",
    destTxnHash: "0x5678...efgh",
    destTxnUrl: "https://polygonscan.com/tx/0x5678...efgh",
    outputAmount: "449144858547000000",
    outputToken: {
      address: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
      symbol: "MATIC",
      decimals: 18,
      name: "MATIC",
    },
  },
};
```

### Code Example

In the code example below, we use plain JavaScript for simplicity, but the code is framework-agnostic and can be easily adapted to React, Next.js, Vite, or any other JavaScript framework. Simply modify the syntax and component structure to match your chosen framework's conventions.

```javascript
import { BrowserProvider, JsonRpcProvider } from "ethers";
const headers = {
  "x-integrator-id": "YOUR_INTEGRATOR_ID",
};

// Helper function to establish connection with an EVM wallet
// Returns essential wallet interaction objects: provider, signer, and wallet address
async function connectWallet() {
  if (!window.ethereum) {
    throw new Error("MetaMask or EVM wallet not detected");
  }

  // For browser environment
  const provider = new BrowserProvider(window.ethereum);
  const accounts = await provider.send("eth_requestAccounts", []);
  const signer = await provider.getSigner();
  // For server environment(nodejs)
  // const provider = new JsonRpcProvider(process.env.RPC_URL);
  // const accounts = await provider.send("eth_requestAccounts", []);
  // const signer = await provider.getSigner();
  return { provider, signer, address: accounts[0] };
}

async function evmToSolBridge() {
  let baseUrl = "https://api2.blockend.com/v1/";

  let queryParams = {
    fromChainId: "137",
    toChainId: "137",
    fromAssetAddress: "0xb7b31a6bc18e48888545ce79e83e06003be70930",
    toAssetAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
    inputAmountDisplay: "0.449594453",
    userWalletAddress: "0x...",
    recipient: "4s...",
  };

  // For details about each parameter, refer end points section above

  // Construct API query string for quote retrieval
  let quotesQueryString = `quotes?fromChainId=${queryParams.fromChainId}&toChainId=${queryParams.toChainId}&fromAssetAddress=${queryParams.fromAssetAddress}&toAssetAddress=${queryParams.toAssetAddress}&inputAmountDisplay=${queryParams.inputAmountDisplay}&userWalletAddress=${queryParams.userWalletAddress}&recipient=${queryParams.recipient}`;

  try {
    // Step 1: Initialize wallet connection
    // Establishes connection and gets necessary wallet interfaces
    const { provider, signer, address } = await connectWallet();

    // Step 2: Fetch available quotes
    // Retrieves possible bridge routes with pricing information
    // Response: { status: string, data: { quotes: [{ routeId: string, ... }] } }
    const quoteRequest = await fetch(`${baseUrl}${quotesQueryString}`, {
      headers,
    });
    const quotes = await quoteRequest.json();

    // Step 3: Initialize transaction sequence
    // Gets all necessary transaction steps using the selected quote
    // Response: { status: string, data: {  steps:[{stepId: string}] } }
    // Note: Production implementations should include quote selection logic
    let routeId = quotes.data.quotes[0].routeId;
    let createTransactionQueryString = `createTx?routeId=${routeId}`;
    const createTxRequest = await fetch(
      `${baseUrl}${createTransactionQueryString}`,
      {
        headers,
      }
    );

    const createTransactionData = await createTxRequest.json();

    // Step 4: Execute transaction sequence
    // Processes each step (e.g., token approval, bridge)
    // Each step requires separate blockchain transaction
    // Response: { status: string, data: {  steps:[{stepId: string}] } }
    let stepCount = 1;
    for (let step of createTransactionData.data.steps) {
      let stepId = step.stepId;
      let nextTxQueryString = `nextTx?routeId=${routeId}&stepId=${stepId}`;
      const nextTxRequest = await fetch(`${baseUrl}${nextTxQueryString}`, {
        headers,
      });
      const nextTxData = await nextTxRequest.json();

      // Step 5: Submit transaction
      // Sends the transaction to the blockchain network
      // Response includes transaction hash for status tracking
      const transaction = await signer.sendTransaction(
        nextTxData.data.txnData.txnEvm
      );
      console.log("Transaction Result:", transaction);
      let txnHash = transaction.hash;

      // Step 6: Monitor transaction status
      // Polls status endpoint until the status is not in-progress
      // Response: { status: string, data: {  status: string,srcTxnHash: string,srcTxnUrl: string,destTxnHash: string,destTxnUrl: string,outputAmount: string,outputToken:{...} } }
      // Status codes: in-progress, success, partial-success, failed
      let currentStatus = "in-progress";
      let statusResponse;

      while (currentStatus === "in-progress") {
        let transactionStatusQueryString = `status?routeId=${routeId}&stepId=${stepId}&txnHash=${txnHash}`;
        const transactionStatusRequest = await fetch(
          `${baseUrl}${transactionStatusQueryString}`,
          {
            headers,
          }
        );
        statusResponse = await transactionStatusRequest.json();

        if (statusResponse.status === "error") {
          currentStatus = "in-progress";
          continue;
        } else {
          currentStatus = statusResponse.data.status;
        }

        // Handle different transaction states
        if (currentStatus === "failed") {
          throw new Error("Transaction failed");
        }

        if (currentStatus === "in-progress") {
          // Wait 2 seconds before next status check
          await new Promise((resolve) => setTimeout(resolve, 2000));
        }

        // Update progress for multi-step transactions
        if (currentStatus === "success") {
          stepCount === createTransactionData.data.steps.length
            ? alert("Transaction successful, TxnHash: " + txnHash)
            : stepCount++;
        }
        if (currentStatus === "partial-success") {
          stepCount === createTransactionData.data.steps.length
            ? alert("Transaction partially successful, txnHash: " + txnHash)
            : stepCount++;
        }
      }
    }
  } catch (error) {
    console.error("Error:", error);
    throw new Error(error.message);
  }
} 
```
