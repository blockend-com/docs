# executeQuote

Execute quote with the provided quote and wallet. This method handles the actual execution of the cross-chain transaction using the appropriate wallet/provider based on the chain type.

```typescript
interface ExecuteTransactionParams {
  quote: Quote;
  // For EVM chains (Ethereum, BSC, Polygon, etc.)
  provider?: Provider | WalletClient; // Supports ethers.js BrowserProvider or viem WalletClient (including wagmi's getWalletClient)
  // For Solana chain
  walletAdapter?: WalletAdapter; // Compatible with @solana/web3.js and @solana/wallet-adapter-base wallets
  // For Cosmos-based chains
  cosmosClient?: any; // Compatible with @cosmjs/stargate and @cosmjs/cosmwasm-stargate clients
  // Optional callback for tracking transaction status
  onStatusUpdate?: (status: TransactionStatus, data?: ExecuteTransactionResult) => void;
  onCreateTxComplete?: (tx: CreateTransactionResponse) => void;
  onNextTxComplete?: (tx: TransactionDataResponse) => void;
  onSignComplete?: (txDetail: { stepId: string; txHash: string }) => void;
  solanaRpcUrl?: string;
}
```

**Examples for different chains:**

```typescript
// 1. EVM Chains Examples
// Using ethers.js BrowserProvider
import { BrowserProvider } from "ethers";
const provider = new BrowserProvider(window.ethereum);
const evmResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  provider: provider,
});

// Using viem WalletClient
import { createWalletClient, custom } from "viem";
const walletClient = createWalletClient({
  transport: custom(window.ethereum),
});
const evmViemResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  provider: walletClient,
});

// Using wagmi's getWalletClient
import { getWalletClient } from "@wagmi/core";
const wagmiClient = await getWalletClient();
const evmWagmiResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  provider: wagmiClient,
});

// 2. Solana Examples
// Using Phantom Wallet
import { PhantomWalletAdapter } from "@solana/wallet-adapter-phantom";
const phantomWallet = new PhantomWalletAdapter();
await phantomWallet.connect();
const solanaResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  walletAdapter: phantomWallet,
});

// Using Solana Wallet Adapter
import { useWallet } from "@solana/wallet-adapter-react";
const { wallet } = useWallet();
const solanaAdapterResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  walletAdapter: wallet,
});

// 3. Cosmos Examples
// Using CosmJS with Keplr
import { SigningStargateClient } from "@cosmjs/stargate";
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";

// For standard Cosmos chains
const getStargateClient = async () => {
  if (!window.keplr) throw new Error("Keplr not installed");
  await window.keplr.enable("cosmoshub-4"); // or your chain ID
  const offlineSigner = window.keplr.getOfflineSigner("cosmoshub-4");
  const client = await SigningStargateClient.connectWithSigner(
    "https://rpc.cosmos.network", // your RPC endpoint
    offlineSigner
  );
  return client;
};

const cosmosResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  cosmosClient: await getStargateClient(),
});

// For CosmWasm chains (e.g., Terra, Secret Network)
const getCosmWasmClient = async () => {
  if (!window.keplr) throw new Error("Keplr not installed");
  await window.keplr.enable("phoenix-1"); // or your chain ID
  const offlineSigner = window.keplr.getOfflineSigner("phoenix-1");
  const client = await SigningCosmWasmClient.connectWithSigner(
    "https://terra-rpc.example.com", // your RPC endpoint
    offlineSigner
  );
  return client;
};

const cosmWasmResult = await executeTransaction({
  quote: quotes.data.quotes[0],
  cosmosClient: await getCosmWasmClient(),
});
```

The `provider`, `walletAdapter`, and `cosmosClient` parameters are mutually exclusive - you should provide the appropriate one based on the chain you're interacting with:

* For EVM chains: provide the `provider` parameter
* For Solana: provide the `walletAdapter` parameter
* For Cosmos chains: provide the `cosmosClient` parameter

### onStatusUpdate

The `onStatusUpdate` callback receives real-time updates about the transaction status and optional data payload.\


<pre class="language-typescript"><code class="lang-typescript">export interface ExecuteTransactionResult {
  routeId: string;
  status: TransactionStatus;
  srcTxnHash?: string;
  srcTxnUrl?: string;
  destTxnHash?: string;
  destTxnUrl?: string;
  outputAmount: string;
  outputToken: Token;
  warnings?: string[];
  points?: number;
  stepId?: string;
  txHash?: string;
}
export type Status="not-started" | "in-progress" | "success" | "failed" | "partial-success";

<strong>onSuccessUpdate:(status:string,data:ExecuteTransactionResult)=>{
</strong>console.log(status)// current status of the transaction
console.log(data)// data realted to the current transaction like transaction hash, transaction url, output token details etc..,
<strong>}
</strong></code></pre>

### onCreateTxComplete

The `onCreateTxComplete` callback receives the success response of /createTx endpoint

```typescript
onCreateTxComplete?: (tx: CreateTransactionResponse) => void;
onCreateTxComplete:(createTxData:CreateTransactionData)=>{
console.log(createTxData)
}
```

Refer the  page below for more details related to createTxData

{% content-ref url="../../api-reference/create-transaction.md" %}
[create-transaction.md](../../api-reference/create-transaction.md)
{% endcontent-ref %}

### onNextTxComplete

The `onNextTxComplete` callback receives the success response of /nextTx endpoint

```typescript
onNextTxComplete?: (tx: TransactionDataResponse) => void;
onCreateTxComplete:(nextTxData:TransactionDataResponse)=>{
console.log(nextTxData)
}
```

Refer the  page below for more details related to nextTxData

{% content-ref url="../../api-reference/get-raw-transaction-to-execute.md" %}
[get-raw-transaction-to-execute.md](../../api-reference/get-raw-transaction-to-execute.md)
{% endcontent-ref %}

### onSignComplete

The `onSignComplete` callback receives stepId of the signed transaction and the transaction hash

```typescript
onSignComplete?: (txDetail: { stepId: string; txHash: string }) => void;
onSignComplete:(txDetail:{ stepId: string; txHash: string})=>{
console.log(txDetail)
}
```

\
Errors will be thrown if any of the apis or signing transaction failed so that developers can catch the error and show it to their users.
