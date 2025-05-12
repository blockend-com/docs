---
icon: square-code
---

# Get Raw Transaction To Execute

The TxnData type combines transaction metadata with network-specific transaction details for different blockchain networks (EVM, Solana, Cosmos, Tron)

Call this api with `routeId` and `stepId` of individual steps to get the transaction data to execute. Once the transaction is executed, check the status of the transaction using `/status` api and proceed to the next step of the transaction.

> Note: you can ignore status check and skip to next step of the txn if `skipTxn` field is set to `true` in the response. Also, when `skipTxn` is set to `true`, `txnData` will be `null`.

### Endpoint: `GET /nextTx`&#x20;

### Query params

```
/nextTx?
    routeId=
    &stepId=
```

### Response

```typescript
{
    routeId: string;
    stepId: string;
    txnData: TxnData | null;
    skipTxn?: boolean;
}
```

<table><thead><tr><th width="184">Field</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td>requestId</td><td>string</td><td>Associated request identifier</td></tr><tr><td>routeId</td><td>string</td><td>Associated route identifier</td></tr><tr><td>stepId</td><td>string</td><td>Current step identifier</td></tr><tr><td>networkType</td><td>NetworkType</td><td>Blockchain network type</td></tr><tr><td>deadline</td><td>number</td><td>Transaction expiration timestamp</td></tr><tr><td>skipTxn</td><td>boolean</td><td>This step's associated txn can be skipped</td></tr><tr><td>txnEvm</td><td>TxnEvm | null</td><td>Either of one is present depending on network type</td></tr><tr><td>txnSol</td><td>TxnSol | null</td><td></td></tr><tr><td>txnTron</td><td>TxnTron | null</td><td></td></tr><tr><td>txnCosmos</td><td>TxnCosmos | null</td><td></td></tr></tbody></table>

Network-Specific Transaction Data:

EVM Transaction (TxnEvm)

| Field    | Type   | Required | Description                  |
| -------- | ------ | -------- | ---------------------------- |
| from     | string | No       | Sender address               |
| to       | string | Yes      | Recipient contract/address   |
| value    | string | No       | Native token amount (in wei) |
| data     | string | No       | Transaction calldata         |
| gasPrice | string | No       | Gas price in wei             |
| gasLimit | string | No       | Maximum gas limit            |

Solana Transaction (TxnSol)

| Field | Type   | Required | Description              |
| ----- | ------ | -------- | ------------------------ |
| data  | string | Yes      | Encoded transaction data |

Cosmos Transaction (TxnCosmos)

| Field        | Type   | Required | Description              |
| ------------ | ------ | -------- | ------------------------ |
| data         | string | Yes      | Transaction data         |
| value        | string | Yes      | Transaction value        |
| gasLimit     | string | Yes      | Gas limit                |
| gasPrice     | string | Yes      | Gas price                |
| maxFeePerGas | string | Yes      | Maximum fee per gas unit |

Tron Transaction (txnTron)

| Field          | Type    | Required | Description            |
| -------------- | ------- | -------- | ---------------------- |
| raw\_data      | any     | No       | Raw transaction data   |
| raw\_data\_dex | string  | No       | DEX-specific data      |
| txID           | string  | Yes      | Transaction ID         |
| visible        | boolean | Yes      | Transaction visibility |

### Example <a href="#example" id="example"></a>

Lets now fetch the transaction data for the first step of the transaction we created in `/createTx` api.

**Request:**

```
https://api2.blockend.com/v1/nextTx
    ?routeId=01J2WB2ZTWAWXN9K48899CSSVN
    &stepId=01J2WB3JEB34B0A1SXHT1E3B63
```

**Response:** As the

```json
{
    "status": "success",
    "data": {
        "routeId": "01J2WB2ZTWAWXN9K48899CSSVN",
        "stepId": "01J2WB3JEB34B0A1SXHT1E3B63",
        "txnData": {
            "id": "01J2WD2YRTT6AN4450NKX9H1WB",
            "routeId": "01J2WB2ZTWAWXN9K48899CSSVN",
            "stepId": "01J2WB3JEB34B0A1SXHT1E3B63",
            "isCompleted": false,
            "networkType": "evm",
            "txnEvm": {
                "from": "0x17e7c3DD600529F34eFA1310f00996709FfA8d5c",
                "to": "0x3c499c542cef5e3811e1192ce70d8cc03d5c3359",
                "data": "0x095ea7b3000000000000000000000000ef4fb24ad0916217251f553c0596f8edc630eb66ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                "gasPrice": "30000000030",
                "gasLimit": 56167
            },
            "createdAt": 1721087654682,
            "status": "not-started",
            "fetchedAt": 1721087654682,
            "requestId": "01J2WB2ZBWNW0M0CJEYV415HPZ"
        }
    }
}
```
