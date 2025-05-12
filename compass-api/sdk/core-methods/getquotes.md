# getQuotes

Gets quotes for cross-chain token swaps or transfers by aggregating liquidity from multiple sources including DEXs, Bridges, RFQs, and Intent Protocols.

```typescript
interface QuoteParams {
  // Chain ID of the source blockchain (e.g., "ethereum", "solana", "bsc")
  fromChainId: string;

  // Token contract address on source chain (use native token address for chain's native currency)
  fromAssetAddress: string;

  // Chain ID of the destination blockchain
  toChainId: string;

  // Token contract address on destination chain
  toAssetAddress: string;

  // Amount in smallest unit (wei, lamports, etc.)
  inputAmount: string;

  // Human readable amount (e.g., "1.0" ETH)
  inputAmountDisplay: string;

  // Source wallet address that will initiate the transaction
  userWalletAddress: string;

  // Optional: Destination wallet address (defaults to userWalletAddress if not specified)
  recipient?: string;

  // Optional: Solana priority fee in lamports or predefined level ("LOW" | "MEDIUM" | "HIGH")
  solanaPriorityFee?: number | PriorityLevel;

  // Optional: Solana Jito MEV tip in lamports or predefined level
  solanaJitoTip?: number | PriorityLevel;

  // Optional: EVM priority fee in gwei or predefined level
  evmPriorityFee?: number | PriorityLevel;

  // Optional: Maximum allowed slippage in BPS (default: 50)
  slippage?: number;

  // Optional: Skip certain validation checks for faster response
  skipChecks?: boolean;

  // Optional: Comma-separated list of preferred liquidity sources
  include?: string;

  // Optional: Comma-separated list of liquidity sources to exclude
  exclude?: string;

  // Optional: Use recommended liquidity provider
  recommendedProvider?: boolean;

  // Optional: To get gasless (approval + swap) transaction quotes.
  gasless?:boolean;

  // Optional: To  get  gasless transaction for swaps, but approvals may or may not require gas.
  gaslessSwap:boolean;

  // Optional: To set fee in BPS
  feeBps: number | string;
}

//Example implementation
const quotes = await getQuotes({
  fromChainId: "137",
  fromAssetAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
  toChainId: "137",
  toAssetAddress: "0xc2132d05d31c914a87c6611c10748aeb04b58e8f.",
  inputAmountDisplay: "1.0",
  userWalletAddress: "0x...",
  recipient: "0x...",
});

// Example Response (shortened)
{
  "status": "success",
  "data": {
    "quotes": [{
      "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
      "from": {
        "chainId": "1",
        "symbol": "ETH",
        // ... other token details
      },
      "to": {
        "chainId": "sol",
        "symbol": "USDC",
        // ... other token details
      },
      "outputAmountDisplay": "1459.244847",
      "estimatedTimeInSeconds": 900
    }]
  }
}
```

[View full quotes response example](https://docs.blockend.com/compass-api/api-reference/fetching-quotes)
