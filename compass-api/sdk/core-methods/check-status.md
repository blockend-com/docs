# Check Status

Monitors the status of a transaction step, providing detailed information about its progress.

```typescript
interface StatusCheckParams {
  // Route ID of the transaction
  routeId: string;

  // Step ID being checked
  stepId: string;

  // Transaction hash from the blockchain
  txnHash: string;
}

//Example implementation
const status = await checkStatus({
  routeId: transaction.routeId,
  stepId: transaction.steps[0].id,
  txnHash: result.hash,
});

// Example Response (shortened)
{
  "status": "success",
  "data": {
    "status": "success",
    "outputAmount": "1459244847",
    "outputAmountDisplay": "1459.244847"
  }
}
type TransactionStatus = "not-started" | "in-progress" | "success" | "failed";
```

See checkStatus example implementation using while loop below.

[View full checkStatus response example](https://docs.blockend.com/compass-api/api-reference/check-transaction-status)

Here's a simpler example to poll the transaction status:

```typescript
async function pollWithWhileLoop(
  routeId: string,
  stepId: string,
  txnHash: string
) {
  const POLLING_INTERVAL = 3000; // 3 seconds
  const MAX_ATTEMPTS = 200; // Maximum number of attempts (10 minutes with 3s interval)
  let attempts = 0;

  while (attempts < MAX_ATTEMPTS) {
    try {
      const response = await checkStatus({ routeId, stepId, txnHash });
      const status = response.data.status;

      console.log(`Current status: ${status}`);

      if (["success", "failed", "partial-success"].includes(status)) {
        return response.data;
      }

      // Wait for the polling interval
      await new Promise((resolve) => setTimeout(resolve, POLLING_INTERVAL));
      attempts++;
    } catch (error) {
      console.error("Error checking status:", error);
      throw error;
    }
  }

  throw new Error("Polling timeout: Maximum attempts reached");
}

// Usage example:
try {
  const result = await pollWithWhileLoop(
    "your-route-id",
    "your-step-id",
    "your-transaction-hash"
  );
  console.log("Final result:", result);
} catch (error) {
  console.error("Polling failed:", error);
}
```
