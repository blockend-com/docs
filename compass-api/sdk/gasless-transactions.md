# Gasless Transactions

The SDK supports gasless transactions through two options in the `QuoteParams`:

```typescript
interface QuoteParams {
  // ... other params ...

  // Optional: Get fully gasless transactions (both approval and swap)
  gasless?: boolean;

  // Optional: Get gasless swap transactions (approvals may still require gas)
  gaslessSwap?: boolean;
}
```

Example of requesting a gasless quote:

```typescript
const gaslessQuote = await getQuotes({
  fromChainId: "137",
  fromAssetAddress: "0x...",
  toChainId: "1",
  toAssetAddress: "0x...",
  inputAmount: "1000000000000000000",
  inputAmountDisplay: "1.0",
  userWalletAddress: "0x...",
  gasless: true, // Enable gasless(approval+swap) transactions
  gaslessSwap: true, // Enable gasless atleast for swaps and approval may or not be gasless based on the tokens selected
});
```

Example response for a gasless quote:

```typescript
{
  "status": "success",
  "data": {
    "quotes": [{
      "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
      "from": {
        "chainId": "137",
        "symbol": "MATIC",
        "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
        "decimals": 18,
        "name": "Polygon",
        "blockchain": "Polygon",
        "isNative": true
      },
      "to": {
        "chainId": "1",
        "symbol": "USDC",
        "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
        "decimals": 6,
        "name": "USD Coin",
        "blockchain": "Ethereum"
      },
      "steps": [{
        "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
        "stepType": "swap",
        "protocolsUsed": ["uniswap-v3"],
        "provider": "uniswap",
        "gasless": true, // Indicates this step is gasless
        "from": {
          // Token details same as above
        },
        "to": {
          // Token details same as above
        },
        "fee": [{
          "name": "Protocol Fee",
          "value": "0.3",
          "token": "MATIC"
        }],
        "inputAmount": "1000000000000000000",
        "outputAmount": "1459244847",
        "estimatedTimeInSeconds": 300
      }],
      "outputAmountDisplay": "1.459244847",
      "outputAmount": "1459244847",
      "estimatedTimeInSeconds": 300,
      "fee": [{
        "name": "Protocol Fee",
        "value": "0.3",
        "token": "MATIC"
      }],
      "gasless": true // Indicates the entire route is gasless
    }]
  }
}
```

Example response for a gasless create transaction:

```typescript
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "steps": [{
      "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
      "stepType": "approval",
      "protocolsUsed": ["permit2"],
      "provider": "permit2",
      "gasless": true,
      "chainId": "137"
    },
    {
      "stepId": "01J2WB1NY64MKVCM8FM994WMZW",
      "stepType": "swap",
      "protocolsUsed": ["uniswap-v3"],
      "provider": "uniswap",
      "gasless": true,
      "chainId": "137"
    }]
  }
}
```

Example response for a gasless transaction step (swap):

```typescript
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "stepId": "01J2WB1NY64MKVCM8FM994WMZW",
    "networkType": "evm",
    "txnData": {
      "txnType": "sign-typed",
      "txnEvm": {
        "domain": {
          "name": "Gasless Swap",
          "version": "1",
          "chainId": 137,
          "verifyingContract": "0x9876543210987654321098765432109876543210"
        },
        "types": {
          "MetaTransaction": [
            { "name": "from", "type": "address" },
            { "name": "to", "type": "address" },
            { "name": "value", "type": "uint256" },
            { "name": "nonce", "type": "uint256" },
            { "name": "deadline", "type": "uint256" }
          ]
        },
        "message": {
          "from": "0x17e7c3DD600529F34eFA1310f00996709FfA8d5c",
          "to": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
          "value": "1000000000000000000",
          "nonce": "1",
          "deadline": "1747836789"
        },
        "primaryType": "MetaTransaction"
      }
    },
    "gasless": true
  }
}
```

Example code to execute a gasless transaction :

```typescript
// wagami / viem
import { getWalletClient } from "wagmi";
import { config } from "./config";

const client = getWalletClient(config);
const signature = await client.signTypedData({
  account, // userWalletAddress
  domain,
  types,
  primaryType,
  message,
});

// Ethers
import { BrowserProvider } from "ethers";
const provider = new BrowserProvider(window.ethereum);
//make sure the wallet is connected before executing this step
delete types.EIP712Domain;
const signer = await provider.getSigner();
const signature = await signer.signTypedData(
  domain,
  {
    [primaryType]: types[primaryType],
    ...types,
  },
  message
);
```
