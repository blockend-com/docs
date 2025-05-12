# getNextTxn

Retrieves the raw transaction data for the next pending step in the transaction sequence. The response format varies based on the blockchain network:

```typescript
interface TransactionDataParams {
  routeId: string; // Route ID from createTransaction
  stepId: string; // Step ID from the transaction steps array
}

//Example implementation
const nextTxn = await getNextTxn({
  routeId: transaction.routeId,
  stepId: transaction.steps[0].id,
});

// Example Response (shortened)
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
    "networkType": "evm",
    "txnData": {
      "txnType": "on-chain" | "sign-typed" | "sign-untyped"
      "txnEvm": {
        "to": "0x...",
        "value": "420000000000000000",
        // ... other transaction details
      }
    }
  }
}
```

**Transaction Types**

The SDK supports multiple transaction types that are automatically handled based on the protocol and requirements:

1.  **On-chain Transactions** (`txnType: "on-chain"`):

    * Regular blockchain transactions that require gas fees
    * Used for standard token transfers and swaps

    ```typescript
    // Example response for on-chain transaction
    {
      "txnType": "on-chain",
      "txnEvm": {
        "from": "0x17e7c3DD600529F34eFA1310f00996709FfA8d5c",
        "to": "0x1234567890abcdef1234567890abcdef12345678",
        "value": "420000000000000000",
        "data": "0x095ea7b3...",
        "gasPrice": "30000000000",
        "gasLimit": "250000"
      }
    }

    // Execute using wagmi/viem
    import { getWalletClient } from "wagmi";
    const client = await getWalletClient();
    const hash = await client.sendTransaction({
      to: txnEvm.to as `0x${string}`,
      data: txnEvm.data as `0x${string}`,
      value: BigInt(txnEvm.value),
      gas: BigInt(txnEvm.gasLimit),
      gasPrice: BigInt(txnEvm.gasPrice),
    });

    // Execute using ethers.js
    import { BrowserProvider } from "ethers";
    const provider = new BrowserProvider(window.ethereum);
    const signer = await provider.getSigner();
    const transaction = await signer.sendTransaction({
      from: txnEvm.from,
      to: txnEvm.to,
      data: txnEvm.data,
      gasLimit: txnEvm.gasLimit,
      gasPrice: txnEvm.gasPrice,
      value: txnEvm.value,
    });
    const hash = transaction.hash;
    ```
2.  **EIP-712 Typed Data Signing** (`txnType: "sign-typed"`):

    * Implements EIP-712 for structured data signing on EVM chains
    * Used for permit-style approvals and meta-transactions (for gasless)

    ```typescript
    // Example response for typed data signing
    {
      "txnType": "sign-typed",
      "txnEvm": {
        "domain": {
          "name": "Permit2",
          "version": "1",
          "chainId": 1,
          "verifyingContract": "0x000000000022D473030F116dDEE9F6B43aC78BA3"
        },
        "types": {
          "PermitSingle": [
            { "name": "details", "type": "PermitDetails" },
            { "name": "spender", "type": "address" },
            { "name": "sigDeadline", "type": "uint256" }
          ],
          "PermitDetails": [
            { "name": "token", "type": "address" },
            { "name": "amount", "type": "uint160" },
            { "name": "expiration", "type": "uint48" },
            { "name": "nonce", "type": "uint48" }
          ]
        },
        "message": {
          "details": {
            "token": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
            "amount": "1461501637330902918203684832716283019655932542975",
            "expiration": "1747836789",
            "nonce": "0"
          },
          "spender": "0x1234567890123456789012345678901234567890",
          "sigDeadline": "1747836789"
        },
        "primaryType": "PermitSingle"
      }
    }

    // Execute using wagmi/viem
    import { getWalletClient } from "wagmi";
    const client = await getWalletClient();
    const signature = await client.signTypedData({
      account, // userWalletAddress
      domain: txnEvm.domain,
      types: txnEvm.types,
      primaryType: txnEvm.primaryType,
      message: txnEvm.message,
    });

    // Execute using ethers.js
    import { BrowserProvider } from "ethers";
    const provider = new BrowserProvider(window.ethereum);
    const signer = await provider.getSigner();
    // Remove EIP712Domain from types before signing
    delete txnEvm.types.EIP712Domain;
    const signature = await signer.signTypedData(
      txnEvm.domain,
      {
        [txnEvm.primaryType]: txnEvm.types[txnEvm.primaryType],
        ...txnEvm.types,
      },
      txnEvm.message
    );
    ```
3.  **Untyped Message Signing** (`txnType: "sign-untyped"`):

    * Used for protocols like Meson.fi that require message signing
    * Supports cross-chain message verification

    ```typescript
    // Example response for untyped message signing
    {
      "txnType": "sign-untyped",
      "txnEvm": {
        "message": "0x1901c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20001f4fc3",
      }
    }

    // Execute using wagmi/viem
    import { getWalletClient } from "wagmi";
    const client = await getWalletClient();
    const signature = await client.request({
      method: "personal_sign",
      params: [txnEvm.message, account],
    });

    // Execute using ethers.js
    import { BrowserProvider } from "ethers";
    const provider = new BrowserProvider(window.ethereum);
    const signer = await provider.getSigner();
    const signature = await provider.send("personal_sign", [
      txnEvm.message,
      account,
    ]);
    ```

The transaction type is automatically determined based on the quote and protocol being used. The SDK handles all the necessary signing logic internally, including:

* Proper formatting of typed and untyped data
* Chain-specific signature requirements
* Protocol-specific message formatting
* Gasless transaction handling

**Transaction Data Response Examples**

Send txnEvm or txnSol or txnCosmos to the appropriate wallet/provider based on the networkType and get the transaction hash to monitor the status of the transaction.

**EVM Chain Response**

```typescript
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
    "networkType": "evm",
    "txnData": {
      "txnEvm": {
        "from": "0x17e7c3DD600529F34eFA1310f00996709FfA8d5c",
        "to": "0x1234567890abcdef1234567890abcdef12345678",
        "value": "420000000000000000",
        "data": "0x095ea7b3...",
        "gasPrice": "30000000000",
        "gasLimit": "250000"
      }
    }
  }
}
```

**Solana Chain Response**

```typescript
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
    "networkType": "sol",
    "txnData": {
      "txnSol": {
        "data": "base64EncodedTransactionData...",
      }
    }
  }
}
```

**Cosmos Chain Response**

```typescript
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
    "networkType": "cosmos",
    "txnData": {
      "txnCosmos": {
        "data": "{"typeUrl":"/ibc.applications.trans"}",
        "value": "0",
        "gasLimit": "250000",
        "gasPrice": "0.025",
      }
    }
  }
}
```

[View full getNextTxn response example](https://docs.blockend.com/compass-api/api-reference/get-raw-transaction-to-execute)
