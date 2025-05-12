# Tokens and Chains

Get a list of supported tokens, optionally filtered by chain ID.

```typescript
interface TokensParams {
  chainId?: string;
}

//Example implementation
const tokens = await getTokens();// to fetch all tokens from all chains
const tokens = await getTokens("1");// to fetch all tokens from chain 1

//Example response
{
  "status": "success",
  "data":{
    "1":[
      {
    "networkType": "evm",
    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
    "chainId": "1",
    "blockchain": "Ethereum",
    "decimals": 18,
    "name": "Ethereum",
    "symbol": "ETH",
    "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
    "lastPrice": 2703.62,
    "isEnabled": true,
    "isFlagged": false,
    "isNative": true,
    "isPopular": true
},
//other coins on chain 1
    ]
  }
}
```

**`getChains()`** \


Get a list of supported blockchain networks.

```typescript
interface Chain {
  id: string;
  name: string;
  networkType: "evm" | "sol" | "cosmos";
  nativeCurrency: {
    name: string;
    symbol: string;
    decimals: number;
  };
  blockExplorer: string;
  rpcUrls: string[];
}
//Example implementation
const chains = await getChains(); // to fetch all chains

//Example response
{
  "status": "success",
  "data": [
    {
  "chainId": "sol",
  "symbol": "sol",
  "name": "Solana",
  "networkType": "sol",
  "image": "https://assets.coingecko.com/coins/images/4128/large/solana.png?1696504756",
  "explorer": {
    "address": "https://solscan.io/account/{address}",
    "token": "https://solscan.io/token/{tokenAddress}",
    "txn": "https://solscan.io/tx/{txnHash}"
  },
  "isPopular": true,
  "tokenCount": 946,
  "isEnabled": true
},
//other supported chains
  ]
}
```
