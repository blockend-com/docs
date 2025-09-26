---
hidden: true
---

# Lite Mode

### Overview

Lite Mode is an advanced feature **exclusively available for Solana** that optimizes transactions by significantly reducing transaction size and providing developers with granular control over transaction construction. When enabled, the API returns raw transaction instructions instead of pre-serialized base64-encoded transaction data, allowing for more flexible transaction building and better success rates.

> **Note**: This feature is currently only supported for Solana blockchain swaps. Other chains will use the standard transaction mode.

### Key Features

* **Reduced Transaction Size**: Minimizes the number of accounts included in transactions
* **Instruction-Based**: Returns raw Solana instructions for custom transaction building
* **Provider Optimization**: Uses only the most efficient providers for Solana swaps
* **Automatic Account Management**: Intelligently manages account limits based on mode
* **Direct Route Optimization**: Forces single-hop swaps for faster execution and lower complexity
* **Enhanced Success Rate**: Reduces transaction failures due to size constraints

### Supported Providers

* **Jupiter (JUPAG)** - Primary DEX aggregator with advanced routing (Solana only)
* **OKX** - Multi-chain DEX aggregator with Solana support (Solana only)

> **Chain Limitation**: These providers are configured for lite mode only when `fromChainId` and `toChainId` are both set to `"sol"`.

### How It Works

#### Quote API Behavior

When `lite=true` is enabled in the quote request **for Solana swaps**:

1. **Provider Selection**: Only Jupiter and OKX providers are utilized for Solana swaps
2. **Endpoint Optimization**:
   * Jupiter switches to `/swap-instructions` endpoint
   * OKX switches to `/swap-instruction` endpoint
3. **Account Management**:
   * `maxAccounts` is automatically set to 20 (vs default 64 in normal mode)
   * This reduces transaction complexity and size
4. **Direct Route Optimization**:
   * Jupiter: `onlyDirectRoutes: true` - Forces single-hop swaps only
   * OKX: `directRoute: true` - Enables direct routing mode
   * Eliminates multi-hop routing for faster execution and lower complexity

> **Important**: Lite mode is automatically disabled for non-Solana chains, and the request will fall back to standard transaction mode.

#### Next Transaction API Response

Instead of returning serialized transaction data, the API provides raw transaction instructions. The response format varies by provider:

**Jupiter Response Format**

```json
{
  "txnData": {
    "txnSol": {
      "data": "{\"addressLookupTableAccount\":[],\"instructionLists\":[...]}"
    }
  }
}
```

**Jupiter Instruction Structure:**

```json
{
  "tokenLedgerInstruction": {
    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
    "accounts": [
      {
        "pubkey": "UserWalletAddress",
        "isSigner": true,
        "isWritable": true
      }
    ],
    "data": "base64-encoded-instruction-data"
  },
  "computeBudgetInstructions": [
    {
      "programId": "ComputeBudget111111111111111111111111111111",
      "accounts": [],
      "data": "base64-encoded-instruction-data"
    }
  ],
  "setupInstructions": [
    {
      "programId": "ATokenGPvbdGVxr1b2hvZbsiqW5xWH25efTNsLJA8knL",
      "accounts": [
        {
          "pubkey": "AssociatedTokenAccount",
          "isSigner": false,
          "isWritable": true
        }
      ],
      "data": "base64-encoded-instruction-data"
    }
  ],
  "swapInstruction": {
    "programId": "JUP4Fb2cqiRUcaTHdrPC8h2gNsA2ETXiPDD33WcGuJB",
    "accounts": [
      {
        "pubkey": "UserWalletAddress",
        "isSigner": true,
        "isWritable": true
      },
      {
        "pubkey": "SourceTokenAccount",
        "isSigner": false,
        "isWritable": true
      }
    ],
    "data": "base64-encoded-instruction-data"
  },
  "cleanupInstruction": {
    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
    "accounts": [
      {
        "pubkey": "WSOLAccount",
        "isSigner": false,
        "isWritable": true
      }
    ],
    "data": "base64-encoded-instruction-data"
  },
  "addressLookupTableAddresses": [
    "AddressLookupTableAccount1",
    "AddressLookupTableAccount2"
  ]
}
```

**OKX Response Format**

```json
{
  "txnData": {
    "txnSol": {
      "data": "{\"addressLookupTableAccount\":[],\"instructionLists\":[...]}"
    }
  }
}
```

**OKX Instruction Structure:**

```json
{
  "addressLookupTableAccount": [
    "AddressLookupTableAccount1"
  ],
  "instructionLists": [
    {
      "programId": "OKXProgramId",
      "accounts": [
        {
          "pubkey": "UserWalletAddress",
          "isSigner": true,
          "isWritable": true
        },
        {
          "pubkey": "SourceTokenAccount",
          "isSigner": false,
          "isWritable": true
        }
      ],
      "data": "base64-encoded-instruction-data"
    }
  ]
}
```

**Response Components:**

**Jupiter Components:**

* `tokenLedgerInstruction`: Token ledger instruction (if using token ledger)
* `computeBudgetInstructions`: Array of compute budget instructions for gas optimization
* `setupInstructions`: Array of setup instructions for creating missing accounts
* `swapInstruction`: The main swap instruction
* `cleanupInstruction`: Cleanup instruction for unwrapping SOL (if needed)
* `addressLookupTableAddresses`: Array of address lookup table addresses

**OKX Components:**

* `addressLookupTableAccount`: Array of address lookup table accounts for transaction optimization
* `instructionLists`: Array of Solana transaction instructions to be executed
* Each instruction contains `programId`, `accounts`, and `data` fields

### API Integration

#### Quote Request Example

```json
{
  "fromChainId": "sol",
  "toChainId": "sol", 
  "fromAssetAddress": "So11111111111111111111111111111111111111112",
  "toAssetAddress": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
  "inputAmountDisplay": "1.0",
  "userWalletAddress": "7zSa114U45nJ8b8ALsuhszS2Kxto8grgedh65Q7xJYii",
  "lite": true,
  "slippage": 50
}
```

> **Chain Requirement**: Both `fromChainId` and `toChainId` must be set to `"sol"` for lite mode to be activated.

#### Transaction Building Process

1. **Quote Request**: Include `lite: true` parameter
2. **Instruction Retrieval**: Use the returned instructions from Next Transaction API
3. **Transaction Construction**: Build your Solana transaction using the provided instructions
4. **Address Lookup Tables**: Utilize the `addressLookupTableAccount` data for optimization
5. **Transaction Submission**: Send the constructed transaction to Solana network

### Account Management

#### Automatic maxAccounts Setting

* **Lite Mode (`lite: true`)**: `maxAccounts` is automatically set to **20**
* **Normal Mode (`lite: false`)**: `maxAccounts` is automatically set to **64**
* **Manual Override**: You can still specify `maxAccounts` in the request to override the automatic setting

#### Why Account Limits Matter

* **Transaction Size**: Solana transactions have size limits (1232 bytes)
* **Account Limits**: Each account reference consumes space
* **Success Rate**: Fewer accounts = higher transaction success rate
* **Gas Efficiency**: Smaller transactions are more cost-effective

### Benefits

#### Performance Improvements

* **Faster Execution**: Direct routes eliminate multi-hop complexity
* **Higher Success Rate**: Smaller transactions are less likely to fail
* **Reduced Gas Costs**: Optimized transaction size leads to lower fees
* **Simplified Routing**: Single-hop swaps reduce MEV exposure and slippage

#### Developer Experience

* **Flexible Integration**: Raw instructions allow custom transaction building
* **Better Control**: Fine-grained control over transaction construction
* **Simplified Debugging**: Easier to troubleshoot instruction-level issues

#### Network Efficiency

* **Reduced Congestion**: Smaller transactions contribute to network efficiency
* **Better Batching**: Instructions can be batched with other operations
* **Optimized Routing**: Direct paths reduce unnecessary hops

### Use Cases

#### High-Frequency Trading

* Minimal transaction size for rapid execution
* Reduced slippage due to faster processing and direct routes
* Better price discovery through single-hop swaps
* Lower MEV exposure with simplified routing

#### DeFi Applications

* Integration with complex DeFi protocols
* Custom transaction building for specific needs
* Batch operations with other Solana instructions

#### Mobile Applications

* Reduced data usage for mobile users
* Faster transaction confirmation
* Better user experience on slower connections

### Implementation Guide

#### Step 1: Enable Lite Mode

```javascript
const quoteRequest = {
  fromChainId: "sol",
  toChainId: "sol",
  fromAssetAddress: "So11111111111111111111111111111111111111112",
  toAssetAddress: "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
  inputAmountDisplay: "1.0",
  userWalletAddress: "7zSa114U45nJ8b8ALsuhszS2Kxto8grgedh65Q7xJYii",
  lite: true
};
```

#### Step 2: Process Quote Response

```javascript
const quoteResponse = await fetch('/api/quote', {
  method: 'POST',
  body: JSON.stringify(quoteRequest)
});
```

#### Step 3: Get Transaction Instructions

```javascript
const nextTxnResponse = await fetch('/api/nextTx', {
  method: 'POST',
  body: JSON.stringify({
    routeId: quoteResponse.routeId,
    stepId: quoteResponse.steps[0].stepId
  })
});

const instructions = JSON.parse(nextTxnResponse.txnData.txnSol.data);
```

#### Step 4: Build and Submit Transaction

**For Jupiter Instructions**

```javascript
import { TransactionInstruction, PublicKey } from '@solana/web3.js';

// Parse Jupiter instructions
const { 
  tokenLedgerInstruction,
  computeBudgetInstructions,
  setupInstructions,
  swapInstruction,
  cleanupInstruction,
  addressLookupTableAddresses 
} = instructions;

// Create transaction instructions array
const txInstructions = [];

// Add compute budget instructions first
if (computeBudgetInstructions) {
  txInstructions.push(...computeBudgetInstructions.map(instruction => 
    new TransactionInstruction({
      programId: new PublicKey(instruction.programId),
      keys: instruction.accounts.map(account => ({
        pubkey: new PublicKey(account.pubkey),
        isSigner: account.isSigner,
        isWritable: account.isWritable,
      })),
      data: Buffer.from(instruction.data, 'base64'),
    })
  ));
}

// Add setup instructions
if (setupInstructions) {
  txInstructions.push(...setupInstructions.map(instruction => 
    new TransactionInstruction({
      programId: new PublicKey(instruction.programId),
      keys: instruction.accounts.map(account => ({
        pubkey: new PublicKey(account.pubkey),
        isSigner: account.isSigner,
        isWritable: account.isWritable,
      })),
      data: Buffer.from(instruction.data, 'base64'),
    })
  ));
}

// Add main swap instruction
if (swapInstruction) {
  txInstructions.push(new TransactionInstruction({
    programId: new PublicKey(swapInstruction.programId),
    keys: swapInstruction.accounts.map(account => ({
      pubkey: new PublicKey(account.pubkey),
      isSigner: account.isSigner,
      isWritable: account.isWritable,
    })),
    data: Buffer.from(swapInstruction.data, 'base64'),
  }));
}

// Add cleanup instruction if needed
if (cleanupInstruction) {
  txInstructions.push(new TransactionInstruction({
    programId: new PublicKey(cleanupInstruction.programId),
    keys: cleanupInstruction.accounts.map(account => ({
      pubkey: new PublicKey(account.pubkey),
      isSigner: account.isSigner,
      isWritable: account.isWritable,
    })),
    data: Buffer.from(cleanupInstruction.data, 'base64'),
  }));
}

// Build and send transaction with address lookup tables
// See Jupiter docs for complete implementation
```

**For OKX Instructions**

```javascript
import { TransactionInstruction, PublicKey } from '@solana/web3.js';

// Parse OKX instructions
const { addressLookupTableAccount, instructionLists } = instructions;

// Create transaction instructions
const txInstructions = instructionLists.map(instruction => 
  new TransactionInstruction({
    programId: new PublicKey(instruction.programId),
    keys: instruction.accounts.map(account => ({
      pubkey: new PublicKey(account.pubkey),
      isSigner: account.isSigner,
      isWritable: account.isWritable,
    })),
    data: Buffer.from(instruction.data, 'base64'),
  })
);

// Build and send transaction with address lookup tables
// See OKX docs for complete implementation
```

### Best Practices

1. **Handle Instructions Properly** - Parse and validate instruction data
2. **Utilize Address Lookup Tables** - They significantly reduce transaction size
3. **Monitor Success Rates** - Lite mode should improve transaction success

### Troubleshooting

#### Common Issues

* **Instruction Parsing Errors**: Ensure proper JSON parsing of instruction data
* **Provider Availability**: Only Jupiter and OKX support lite mode
* **Chain Compatibility**: Lite mode only works with Solana (`fromChainId: "sol"` and `toChainId: "sol"`)

#### Debug Tips

* Check instruction structure before building transactions
* Verify address lookup table accounts are properly included
* Monitor transaction size to ensure it stays within limits

### References

#### Provider Documentation

* [Jupiter Swap Instructions API](https://dev.jup.ag/docs/swap-api/build-swap-transaction) - Complete guide for building transactions with Jupiter instructions
* [Jupiter Swap Instructions Example](https://dev.jup.ag/docs/swap-api/build-swap-transaction#build-your-own-transaction-with-instructions) - Code examples for instruction handling
* [OKX Swap Instruction API](https://web3.okx.com/build/dev-docs/dex-api/dex-solana-swap-instruction) - OKX's swap instruction documentation
* [OKX Solana Integration Guide](https://web3.okx.com/build/dev-docs/dex-api/dex-solana-swap-instruction) - Complete implementation examples

#### Solana Documentation

* [Solana Transaction Format](https://docs.solana.com/developing/programming-model/transactions) - Understanding Solana transactions
* [Address Lookup Tables](https://docs.solana.com/developing/lookup-tables) - Optimizing transaction size with ALTs
* [Transaction Instructions](https://docs.solana.com/developing/programming-model/transactions#instruction) - Working with Solana instructions
