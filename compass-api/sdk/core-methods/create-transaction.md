# Create Transaction

Creates a transaction from a selected quote, preparing all necessary steps for the cross-chain transfer.

```typescript
interface CreateTransactionParams {
  // Route ID obtained from getQuotes response
  routeId: string;
}

//Example implementation
const transaction = await createTransaction({
  routeId: quotes.data.quotes[0].routeId,
});

// Example Response (shortened)
{
  "status": "success",
  "data": {
    "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
    "steps": [{
      "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
      "stepType": "bridge",
      "chainId": "1"
    }]
  }
}
```

[View full createTransaction response example](https://docs.blockend.com/compass-api/api-reference/create-transaction)
