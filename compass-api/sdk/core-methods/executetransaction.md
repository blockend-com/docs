# executeTransaction

Executes a single transaction step with the provided transaction data. This method is used internally by `executeQuote` but can also be used directly for more granular control over transaction execution.

<pre class="language-typescript"><code class="lang-typescript"><strong>interface ExecuteTransactionParams {
</strong>  txDataResponse: TransactionDataResponse;
  quote: Quote;
  step: TransactionStep;
  provider: BrowserProvider | WalletClient;
  walletAdapter: WalletAdapter;
  cosmosClient: SigningStargateClient | SigningCosmWasmClient;
  solanaRpcUrl?: string;
}

// Example implementation
const txnResponse = await getNextTxn({
  routeId: quote.routeId,
  stepId: step.stepId,
});
// Example implementation
const txHash = await executeTransaction({
  txDataResponse,
  quote,
  step,
  provider: ethersProvider,
  walletAdapter: solanaWallet,
  cosmosClient: cosmosClient,
  solanaRpcUrl: "https://api.mainnet-beta.solana.com",
});
</code></pre>

The method handles different transaction types based on the network:

* For EVM chains: Supports regular transactions for on chain transactions, EIP-712 typed data signing for gasless transactions, and untyped message signing for meson finance transactions.
* For Solana: Handles Solana-specific transaction formats
* For Cosmos: Manages Cosmos SDK transaction types

The `provider`, `walletAdapter`, and `cosmosClient` parameters are mutually exclusive - you should provide the appropriate one based on the chain you're interacting with:

* For EVM chains: provide the `provider` parameter
* For Solana: provide the `walletAdapter` parameter
* For Cosmos chains: provide the `cosmosClient` parameter

The method returns the transaction hash of the executed transaction, which can be used to monitor its status using `checkStatus` or `pollTransactionStatus`.
