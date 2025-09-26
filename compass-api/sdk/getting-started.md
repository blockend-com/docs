# Getting Started

### Installation

```bash
npm install @blockend/compass-sdk @solana/web3.js @cosmjs/stargate 
# or
yarn add @blockend/compass-sdk @solana/web3.js @cosmjs/stargate
```

### Quick Start

```typescript
import {
  initializeSDK,
  getQuotes,
  executeTransaction,
  createTransaction,
  getNextTxn,
  checkStatus,
} from "@blockend/compass-sdk";

// Initialize the SDK with your API key
initializeSDK({
  apiKey: "YOUR_API_KEY",
  integratorId: "YOUR_INTEGRATOR_ID",
});

// Get quotes for a token swap
const quotes = await getQuotes({
  fromChainId: "137",
  fromAssetAddress: "0x...",
  toChainId: "sol",
  toAssetAddress: "...",
  inputAmount: "1000000000000000000", // Amount in wei
  inputAmountDisplay: "1.0", // Human readable amount
  userWalletAddress: "0x...",
  recipient: "0x...",
});

// Execute the transaction with the best quote
const result = await executeTransaction({
  quote: quotes.data.quotes[0],
  provider: ethersProvider, // For EVM chains
  walletAdapter: solanaWallet, // For Solana
  cosmosClient: cosmosClient, // For Cosmos
  onStatusUpdate: (status, data) => {
    console.log(`Transaction status: ${status}`, data);
  },
});

// Check transaction status, poll the status until the status is "success" or "failed" or "partial-success". "in-progress" means the transaction is still being processed.
const status = await checkStatus({
  routeId: transaction.routeId,
  stepId: transaction.steps[0].id,
  txnHash: result.hash,
});
```

See checkStatus example implementation using while loop in link below

{% content-ref url="core-methods/check-status.md" %}
[check-status.md](core-methods/check-status.md)
{% endcontent-ref %}

