---
icon: list-check
---

# Check Transaction Status

After user signs and submits the transaction on chain, check the status of the transaction by passing in the signature hash of the transaction. You need to keep polling this api to get the `status` of the transaction. Once the status response is `success`, you can proceed to the next step of the transaction (if any).

### Endpoint: `GET /status`&#x20;

### Query params

```
/status?
    routeId=
    &stepId=
    &txnHash=
```

### Response:

```typescript
{
    routeId: string;
    stepId: string;
    status: TxnStatus;
    srcTxnHash?: string;
    srcTxnUrl?: string;
    destTxnHash?: string;
    destTxnUrl?: string;
    points?: number;
}
```

### Example <a href="#example" id="example"></a>

Now, let check the status of the transaction we fetched in the previous step.

**Request:**

```url
https://api2.blockend.com/v1/status
    ?routeId=01J2WB2ZTWAWXN9K48899CSSVN
    &stepId=01J2WB3JEB34B0A1SXHT1E3B63
    &txnHash=0x3d7bdcfac0062b3c9ae1e4045bc43600c2c55c560dde7eebd2a15afb5ba1c390
```

**Response:**

```json
{
    "status": "success",
    "data": {
        "routeId": "01J2WB2ZTWAWXN9K48899CSSVN",
        "stepId": "01J2WB3JEB34B0A1SXHT1E3B63",
        "status": "success",
        "srcTxnHash": "0x3d7bdcfac0062b3c9ae1e4045bc43600c2c55c560dde7eebd2a15afb5ba1c390",
        "srcTxnUrl": "https://polygonscan.com/tx/0x3d7bdcfac0062b3c9ae1e4045bc43600c2c55c560dde7eebd2a15afb5ba1c390"
    }
}
```
