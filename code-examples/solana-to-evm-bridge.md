---
icon: bridge
---

# Solana to EVM Bridge

This guide demonstrates how to execute a token bridge from solana network to EVM-compatible chains using the Compass API. The example covers the complete workflow from connecting a wallet to monitoring transaction status.

### Overview

The implementation follows these key steps:

1. Wallet Connection
2. Quote Retrieval
3. Transaction Creation
4. Step-by-Step Execution
5. Status Monitoring

### Implementation Details

#### Prerequisites

* Phantom or any solana compatible wallet
* Integrator ID from Compass
* Solana Web3.js library

#### API Endpoints

The example uses the following endpoints:

**`/quotes`**

This endpoint fetches quotes from available liquidity sources and responds with quotes that are sorted by best output amount by default. The first item in the quotes array is the recommended quote.

**Request Parameters**

```javascript
let requestParams = {
  fromChainId: "sol", // Source blockchain network ID (Solana), for chain IDs reference: "/chains" api endpoint
  toChainId: "137", // Destination blockchain network ID (Solana), for chain IDs reference: "/chains" api endpoint
  fromAssetAddress: "So11111111111111111111111111111111111111112", // Source token contract address, for token addresses reference: "/tokens" api endpoint
  toAssetAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee", // Destination token contract address, for token addresses reference: "/tokens" api endpoint
  inputAmountDisplay: "0.1", // Amount of source token(Solana) to bridge, this is the amount that will be transferred from the source token chain to the destination token chain, typically received as input from the user
  userWalletAddress: "4sd..", // User's wallet address
  recipient: "0x..", // Recipient's wallet address
};
```

**Example Request**

```
GET /quotes?fromChainId=sol&toChainId=137&fromAssetAddress=So11111111111111111111111111111111111111112&toAssetAddress=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee&inputAmountDisplay=0.1&userWalletAddress=4sd..&recipient=0x..
```

**Example Response**

```javascript
let quoteResponse = {
  status: "success",
  data: {
    quotes: [
      {
        routeId: "f3c8f492-6531-414e-b16f-49146f28e562",
        from: {
          networkType: "sol",
          address: "So11111111111111111111111111111111111111112",
          chainId: "sol",
          blockchain: "Solana",
          decimals: 9,
          name: "Solana",
          symbol: "SOL",
          image:
            "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
          lastPrice: 237.56,
          isEnabled: true,
          isFlagged: false,
          isNative: true,
          isPopular: true,
        },
        to: {
          networkType: "evm",
          address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
          chainId: "137",
          blockchain: "Polygon",
          decimals: 6,
          name: "Tether",
          symbol: "USDT",
          image:
            "https://assets.coingecko.com/coins/images/325/small/Tether.png",
          lastPrice: 0.99996,
          isEnabled: true,
          isFlagged: false,
          isNative: false,
          isPopular: false,
        },
        steps: [
          {
            stepId: "f3c8f492-6531-414e-b16f-49146f28e562:0",
            stepType: "bridge",
            protocolsUsed: ["Mayan (Swift)"],
            from: {
              networkType: "sol",
              address: "So11111111111111111111111111111111111111112",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 9,
              name: "Solana",
              symbol: "SOL",
              image:
                "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
              lastPrice: 237.56,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            to: {
              networkType: "evm",
              address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.99996,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            inputAmount: "100000000",
            outputAmount: "23246337",
            fee: [
              {
                type: "NETWORK",
                token: {
                  networkType: "sol",
                  address: "So11111111111111111111111111111111111111112",
                  chainId: "sol",
                  blockchain: "Solana",
                  decimals: 9,
                  name: "Solana",
                  symbol: "SOL",
                  image:
                    "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
                  lastPrice: 237.56,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_SOURCE_WALLET",
                amountInToken: "370000",
                amountInUSD: "0.0883",
              },
            ],
            estimatedTimeInSeconds: 12,
          },
        ],
        fee: [
          {
            type: "NETWORK",
            token: {
              networkType: "sol",
              address: "So11111111111111111111111111111111111111112",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 9,
              name: "Solana",
              symbol: "SOL",
              image:
                "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
              lastPrice: 237.56,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_SOURCE_WALLET",
            amountInToken: "370000",
            amountInUSD: "0.0883",
          },
          {
            type: "BLOCKEND",
            source: "FROM_OUTPUT_AMOUNT",
            token: {
              networkType: "evm",
              address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.99996,
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
        protocolsUsed: ["Mayan (Swift)"],
        inputAmount: "100000000",
        inputAmountDisplay: "0.1",
        outputAmount: "23473512",
        outputAmountDisplay: "23.473512",
        minOutputAmount: "23.473512",
        slippage: 119,
        userWalletAddress: "4sd55KCv7RFcc14goe3yJtsBTb9XMFjR5GAnemaDdYTd",
        recipient: "0x1b1E919E51a1592Dce70a4FD74107941109B8235",
        createdAt: 1738064338811,
        deadline: 60,
        estimatedTimeInSeconds: 12,
        requestId: "",
        score: {
          outputScore: 1,
          speedScore: 1,
          feeScore: 1,
          slipparageScore: 1,
          stepScore: 1,
          outputDiffPercent: 0,
        },
        tags: ["BEST", "BEST_OUTPUT", "FAST", "LOW_SLIPPAGE"],
      },
      {
        routeId: "a6baa303-d15b-4dd1-bdf6-0278b36e4275",
        from: {
          networkType: "sol",
          address: "So11111111111111111111111111111111111111112",
          chainId: "sol",
          blockchain: "Solana",
          decimals: 9,
          name: "Solana",
          symbol: "SOL",
          image:
            "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
          lastPrice: 237.56,
          isEnabled: true,
          isFlagged: false,
          isNative: true,
          isPopular: true,
        },
        to: {
          networkType: "evm",
          address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
          chainId: "137",
          blockchain: "Polygon",
          decimals: 6,
          name: "Tether",
          symbol: "USDT",
          image:
            "https://assets.coingecko.com/coins/images/325/small/Tether.png",
          lastPrice: 0.99996,
          isEnabled: true,
          isFlagged: false,
          isNative: false,
          isPopular: false,
        },
        steps: [
          {
            stepId: "a6baa303-d15b-4dd1-bdf6-0278b36e4275:0",
            stepType: "bridge",
            protocolsUsed: ["Mayan (Wormhole)"],
            from: {
              networkType: "sol",
              address: "So11111111111111111111111111111111111111112",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 9,
              name: "Solana",
              symbol: "SOL",
              image:
                "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
              lastPrice: 237.56,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            to: {
              networkType: "evm",
              address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.99996,
              isEnabled: true,
              isFlagged: false,
              isNative: false,
              isPopular: false,
            },
            inputAmount: "100000000",
            outputAmount: "19163918",
            fee: [
              {
                type: "NETWORK",
                token: {
                  networkType: "sol",
                  address: "So11111111111111111111111111111111111111112",
                  chainId: "sol",
                  blockchain: "Solana",
                  decimals: 9,
                  name: "Solana",
                  symbol: "SOL",
                  image:
                    "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
                  lastPrice: 237.56,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_SOURCE_WALLET",
                amountInToken: "370000",
                amountInUSD: "0.0883",
              },
              {
                type: "PROVIDER",
                token: {
                  networkType: "sol",
                  address: "So11111111111111111111111111111111111111112",
                  chainId: "sol",
                  blockchain: "Solana",
                  decimals: 9,
                  name: "Solana",
                  symbol: "SOL",
                  image:
                    "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
                  lastPrice: 237.56,
                  isEnabled: true,
                  isFlagged: false,
                  isNative: true,
                  isPopular: true,
                },
                source: "FROM_OUTPUT_AMOUNT",
                amountInToken: "18750421",
                amountInUSD: "4.4750",
              },
            ],
            estimatedTimeInSeconds: 120,
          },
        ],
        fee: [
          {
            type: "NETWORK",
            token: {
              networkType: "sol",
              address: "So11111111111111111111111111111111111111112",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 9,
              name: "Solana",
              symbol: "SOL",
              image:
                "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
              lastPrice: 237.56,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_SOURCE_WALLET",
            amountInToken: "370000",
            amountInUSD: "0.0883",
          },
          {
            type: "PROVIDER",
            token: {
              networkType: "sol",
              address: "So11111111111111111111111111111111111111112",
              chainId: "sol",
              blockchain: "Solana",
              decimals: 9,
              name: "Solana",
              symbol: "SOL",
              image:
                "https://assets.coingecko.com/coins/images/4128/standard/solana.png?1696504756",
              lastPrice: 237.56,
              isEnabled: true,
              isFlagged: false,
              isNative: true,
              isPopular: true,
            },
            source: "FROM_OUTPUT_AMOUNT",
            amountInToken: "18750421",
            amountInUSD: "4.4750",
          },
          {
            type: "BLOCKEND",
            source: "FROM_OUTPUT_AMOUNT",
            token: {
              networkType: "evm",
              address: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
              chainId: "137",
              blockchain: "Polygon",
              decimals: 6,
              name: "Tether",
              symbol: "USDT",
              image:
                "https://assets.coingecko.com/coins/images/325/small/Tether.png",
              lastPrice: 0.99996,
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
        protocolsUsed: ["Mayan (Wormhole)"],
        inputAmount: "100000000",
        inputAmountDisplay: "0.1",
        outputAmount: "19260271",
        outputAmountDisplay: "19.260271",
        minOutputAmount: "19.260271",
        slippage: 1892,
        userWalletAddress: "4sd55KCv7RFcc14goe3yJtsBTb9XMFjR5GAnemaDdYTd",
        recipient: "0x1b1E919E51a1592Dce70a4FD74107941109B8235",
        createdAt: 1738064338816,
        deadline: 60,
        estimatedTimeInSeconds: 120,
        requestId: "",
        score: {
          outputScore: 0.8205108379180754,
          speedScore: 0.1,
          feeScore: 1,
          slipparageScore: 0.06289640591966174,
          stepScore: 1,
          outputDiffPercent: 0.19718549139447822,
        },
        tags: [],
      },
    ],
  },
};
```

[View full quotes response](../compass-api/api-reference/fetching-quotes.md)

**`/createTx`**

This endpoint fetches the transaction steps for the selected quote . Since this is s solana to EVM bridge example, the transaction step includes bridge step. The response includes steps that need to be executed.The Step object includes a step ID and step type (bridge).

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

This endpoint fetches the transaction data that needs to be executed for the selected step. The transaction data includes the transaction parameters for the step. Since this is an Solana to EVM bridge example, the transaction data includes the txnSol object field in the transaction data.

Note: The transaction data is specific to the step type. For example, if the step type is bridge, the transaction data includes the bridge parameters. Moreover, the next step data will be available only if the previous step is executed successfully.

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
      id: "01JJP6MS082138Y3NJB7XYDC1D",
      routeId: "01JJP6MGNXTS99P34JX6PC01GV",
      stepId: "01JJP6MGNXAZ9CV61FNX022ZTZ",
      isCompleted: false,
      networkType: "sol",
      createdAt: 1738059441160,
      txnSol: {
        data: "AQAAAAAAA..", // base64 encoded transaction data
      },
      status: "in-progress",
      fetchedAt: 1738059441160,
      nextTxStart: 1738059440990,
      nextTxEnd: 1738059441160,
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
  txnHash: nextTxResponse.data.txnData.txnSol.hash, // pass the txnHash that is received from the wallet provider once the transaction is signed successfully by the user
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

### NodeJS Implementation

```javascript
import { Connection, Keypair, VersionedTransaction } from "@solana/web3.js";
import bs58 from "bs58";

// Initialize connection and keypair
const keypair = Keypair.fromSecretKey(bs58.decode(SOLANA_PRIVATE_KEY));
console.log({ keypair: keypair.publicKey.toString() });
const connection = new Connection(RPC_URL, "confirmed");
```

### Browser Implementation with Phantom Wallet

```javascript
import { Connection, VersionedTransaction, KeyPair } from "@solana/web3.js";
import Buffer from "buffer";

// Initialize Phantom wallet connection
async function initializeWallet() {
  if (!window.solana || !window.solana.isPhantom) {
    throw new Error("Phantom wallet is not installed!");
  }

  try {
    // Connect to the wallet
    const resp = await window.solana.connect();
    console.log("Connected to wallet:", resp.publicKey.toString());
    return window.solana;
  } catch (err) {
    console.error("Failed to connect to wallet:", err);
    throw err;
  }
}

// Initialize Solana connection
const connection = new Connection(
  "https://api.mainnet-beta.solana.com",
  "confirmed"
);
```

### Common Implementation

```javascript
async function solanaToEvmBridge() {
  // Initialize wallet based on environment
  const wallet =
    typeof window !== "undefined" ? await initializeWallet() : null;

  const solQuoteReq = {
    fromChainId: "sol",
    toChainId: "sol",
    fromAssetAddress: "So11111111111111111111111111111111111111112",
    toAssetAddress: "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
    inputAmountDisplay: "0.01",
    userWalletAddress: wallet ? wallet.publicKey.toString() : "7z..",
    slippage: 2000,
    fixedSlippage: true,
    recipient: wallet ? wallet.publicKey.toString() : "7z...",
  };

  const headers = {
    "x-integrator-id": "your-integrator-id",
  };

  // Fetch quote
  const urlParams = Object.entries(solQuoteReq)
    .map(([key, value]) => `${key}=${value}`)
    .join("&");
  const fetchQuoteReq = await fetch(
    `https://api2.blockend.com/v1/quotes?${urlParams}`,
    {
      headers,
    }
  );
  const fetchQuoteRes = await fetchQuoteReq.json();
  console.log("quotes fetched");
  const quotes = fetchQuoteRes.data.quotes;

  const provider = "jupag";
  const providerQuote = quotes.find((q) => q.provider === provider);
  if (!providerQuote) throw new Error("quote not found, " + provider);

  // Create transaction
  const createdTxnReq = await fetch(
    `https://api2.blockend.com/v1/createTx?routeId=${providerQuote.routeId}`,
    {
      headers,
    }
  );
  const createdTxnRes = await createdTxnReq.json();
  console.log("txn created");

  // Get next transaction
  const nextTxnReq = await fetch(
    `https://api2.blockend.com/v1/nextTx?routeId=${providerQuote.routeId}&stepId=${createdTxnRes.data.steps[0].stepId}`,
    {
      headers,
    }
  );
  const nextTxnRes = await nextTxnReq.json();
  console.log("got next txn");

  const solTxn = nextTxnRes.data.txnData?.txnSol?.data;
  if (!solTxn) throw new Error("txn data not found");

  // Deserialize the transaction
  const txnBuffer = Buffer.from(solTxn, "base64");
  const transaction = VersionedTransaction.deserialize(txnBuffer);

  let signature;

  if (wallet) {
    // Browser environment - Sign with Phantom
    try {
      // Sign and send transaction
      let txn = await wallet.signAndSendTransaction(transaction);
      signature = txn.signature;
      console.log("Transaction sent:", signature);
    } catch (err) {
      console.error("Error sending transaction:", err);
      throw err;
    }
  } else {
    // NodeJS environment - Sign with keypair
    transaction.sign([keypair]);

    // Simulate the transaction
    const result = await connection.simulateTransaction(transaction, {
      sigVerify: true,
    });

    if (!result.value.err) {
      // Send the transaction
      signature = await connection.sendTransaction(transaction);
      console.log("Transaction sent:", signature);
    } else {
      console.error("Transaction simulation failed:", result.value.err);
      throw new Error("Transaction simulation failed");
    }
  }

  // Check transaction status
  const statusCheckReq = await fetch(
    `https://api2.blockend.com/v1/status?routeId=${providerQuote.routeId}&stepId=${createdTxnRes.data.steps[0].stepId}&txnHash=${signature}`,
    { headers }
  );
  const statusCheckRes = await statusCheckReq.json();
  console.log("Transaction status:", statusCheckRes);

  let currentStatus = "in-progress";
  let statusResponse;
  while (currentStatus === "in-progress") {
    // Check transaction status
    const transactionStatusRequest = await fetch(
      `https://api2.blockend.com/v1/status?routeId=${
        providerQuote.routeId
      }&stepId=${createdTxnRes.data.steps[0].stepId}&txnHash=${
        signature || ""
      }`,
      { headers }
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
  }
}

solanaToEvmBridge()
  .catch(console.error)
  .finally(() => process.exit(0));
```

### Usage Notes

1. For browser implementation:
   * Make sure Phantom wallet extension is installed
   * Handle wallet connection states and user interactions appropriately
2. For NodeJS implementation:
   * Ensure you have the required environment variables (SOLANA\_PRIVATE\_KEY, RPC\_URL)
   * Handle errors and cleanup appropriately
3. Common considerations:
   * Replace 'your-integrator-id' with your actual integrator ID
   * Implement proper error handling and loading states
   * Add appropriate UI feedback for transaction status
