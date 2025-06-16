---
icon: rocket-launch
---

# Quick Swap API

The **Quick Swap API** allows you to perform cross-chain or same-chain token swaps in a **single call**, returning both the best route and all the transactions required to complete it. This API is perfect for dApps, wallets, checkouts, or DeFi flows where simplicity and speed are essential.

### Endpoint: `GET /quick-swap`

```url
/quick-swap
    ?fromChainId=
    &fromAssetAddress=
    &toChainId=
    &toAssetAddress=
    &inputAmountDisplay=
    &userWalletAddress=
    &recipient=
```

### Query Parameters

<table><thead><tr><th width="200.18359375">Field</th><th width="199.75390625">Type</th><th>Description</th></tr></thead><tbody><tr><td>fromChainId*</td><td>string</td><td>Source blockchain identifier</td></tr><tr><td>fromAssetAddress*</td><td>string</td><td>Token address on source chain</td></tr><tr><td>toChainId*</td><td>string</td><td>Destination blockchain identifier</td></tr><tr><td>toAssetAddress*</td><td>string</td><td>Token address on destination chain</td></tr><tr><td>inputAmountDisplay*</td><td>string</td><td>Human-readable input amount (e.g., "1.5") <strong>Note:</strong> Either inputAmountDisplay or inputAmount should be passed</td></tr><tr><td>inputAmount*</td><td>string</td><td>Input amount in decimal units <strong>Note:</strong> Either inputAmountDisplay or inputAmount should be passed</td></tr><tr><td>userWalletAddress*</td><td>string</td><td>User's wallet address</td></tr><tr><td>recipient</td><td>string</td><td>Final recipient of the tokens. If not provided userWalletAddress is used by default</td></tr><tr><td>slippage</td><td>number</td><td>Slippage tolerance in basis points (100 = 1%)</td></tr><tr><td>solanaOptions</td><td>SolanaOptions</td><td>Solana-specific parameters</td></tr><tr><td>evmOptions</td><td>EvmOptions</td><td>EVM-specific parameters</td></tr><tr><td>skipChecks</td><td>boolean</td><td>Skip validation checks</td></tr><tr><td>include</td><td>string</td><td>Comma-separated list of providers to include</td></tr><tr><td>exclude</td><td>string</td><td>Comma-separated list of providers to exclude</td></tr></tbody></table>

**Solana Options**

<table><thead><tr><th width="199.953125">Field</th><th width="199.90234375">Type</th><th>Description</th></tr></thead><tbody><tr><td>solanaPriorityFee</td><td>number | PriorityLevel</td><td>Priority fee in micro lamports or priority level</td></tr><tr><td>solanaJitoTip</td><td>number | PriorityLevel</td><td>Jito MEV tip in lamports or priority level</td></tr></tbody></table>

**EVM Options**

<table><thead><tr><th width="200.02734375">Field</th><th width="200.1640625">Type</th><th>Description</th></tr></thead><tbody><tr><td>evmPriorityFee</td><td>number | PriorityLevel</td><td>Priority fee in wei or priority level</td></tr></tbody></table>

```typescript
PriorityLevel = 'low' | 'medium' | 'high' | 'ultra' | 'degen'
```

### Response

The Quick Swap endpoint returns a simplified response containing both the selected route and ready-to-execute transaction data.

<table><thead><tr><th width="200.0546875">Field</th><th width="199.99609375">Type</th><th>Description</th></tr></thead><tbody><tr><td>route</td><td>Route</td><td>The selected route for the swap (null if no suitable route)</td></tr><tr><td>txn</td><td>TxnData[]</td><td>Array of transaction data ready for execution (null if error)</td></tr><tr><td>error</td><td>string</td><td>Error message if the swap cannot be completed</td></tr></tbody></table>

#### Route Object

The route object contains the same structure as returned by the `/quotes` endpoint, including:

* **from/to**: Source and destination token details
* **inputAmount/outputAmount**: Input and output amounts in wei/native units
* **inputAmountDisplay/outputAmountDisplay**: Human-readable amounts
* **provider**: Liquidity provider used
* **estimatedTimeInSeconds**: Estimated execution time
* **steps**: Transaction steps (limited to simple swaps only)
* **fee**: Fee breakdown

#### Transaction Data Array

The `txn` array contains 1-2 transaction objects ready for execution:

1. **Approval Transaction** (if required): ERC20 token approval for non-native tokens
2. **Swap Transaction**: The actual swap transaction

Each transaction object includes:

* **txnEvm**: EVM transaction data (from, to, data, value, gasPrice, gasLimit)
* **txnSol**: Solana transaction data (base64 encoded transaction)
* **networkType**: "evm" or "sol"
* **routeId/stepId**: Identifiers for tracking

### Limitations

The Quick Swap endpoint is designed for simple swaps only and has the following restrictions:

* Only supports single-step swaps (no complex multi-hop routes)
* Automatically uses recommended providers only
* Routes with more than 2 steps (approval + swap) will be rejected
* Cross-chain swaps are supported but must be single-step

### Example

**Request:**

```url
https://api2.blockend.com/v1/quick-swap
?fromChainId=1
&fromAssetAddress=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
&toChainId=1
&toAssetAddress=0xA0b86a33E6417aE4bbBd449c8E87C2E2e20E84DE
&inputAmountDisplay=0.1
&userWalletAddress=0x17e7c3DD600529F34eFA1310f00996709FfA8d5c
&slippage=100
```

**Response:**

```json
{
  "status": "success",
  "data": {
    "route": {
      "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
      "from": {
        "networkType": "evm",
        "chainId": "1",
        "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
        "decimals": 18,
        "name": "Ethereum",
        "symbol": "ETH",
        "isNative": true,
        "lastPrice": 3480.62
      },
      "to": {
        "networkType": "evm",
        "chainId": "1",
        "address": "0xA0b86a33E6417aE4bbBd449c8E87C2E2e20E84DE",
        "decimals": 18,
        "name": "USDC",
        "symbol": "USDC",
        "isNative": false,
        "lastPrice": 1.002
      },
      "inputAmount": "100000000000000000",
      "inputAmountDisplay": "0.1",
      "outputAmount": "348062000000",
      "outputAmountDisplay": "348.062",
      "provider": "1inch",
      "estimatedTimeInSeconds": 30,
      "steps": [{
        "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
        "stepType": "swap",
        "from": { /* token details */ },
        "to": { /* token details */ },
        "inputAmount": "100000000000000000",
        "outputAmount": "348062000000"
      }]
    },
    "txn": [{
      "id": "txn_123",
      "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
      "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
      "networkType": "evm",
      "txnType": "on-chain",
      "txnEvm": {
        "from": "0x17e7c3DD600529F34eFA1310f00996709FfA8d5c",
        "to": "0x1111111254eeb25477b68fb85ed929f73a960582",
        "value": "100000000000000000",
        "data": "0x7c025200000000000000000000000000...",
        "gasPrice": "20000000000",
        "gasLimit": "150000"
      }
    }]
  }
}
```

### Error Responses

The endpoint may return the following error scenarios:

* **No routes found**: When no suitable swap routes are available
* **Route too complex**: When the optimal route requires multiple steps
* **Failed to compute transaction data**: When transaction data cannot be generated

```json
{
  "status": "success",
  "data": {
    "route": null,
    "txn": null,
    "error": "No routes found"
  }
}
```

This endpoint is perfect for applications that need simple, one-click swap functionality without the complexity of managing separate quote and transaction creation flows.
