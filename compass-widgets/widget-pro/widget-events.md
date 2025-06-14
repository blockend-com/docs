# Widget Events

This guide explains how to integrate and use these events in your application.

```javascript
import Blockend, { blockendEventsConstants, useBlockendEvents } from "@blockend/widget";
```

### Event System Overview

The widget uses [mitt](https://github.com/developit/mitt) as the underlying event emitter, providing a lightweight and efficient event system. The `useBlockendEvents` hook returns a mitt instance that you can use to listen to widget events.

### Setting Up Event Listeners

#### Basic Setup

```javascript
import React, { useEffect } from "react";
import { useBlockendEvents, blockendEventsConstants } from "@blockend/widget";

function MyApp() {
  const eventEmitter = useBlockendEvents();

  useEffect(() => {
    // Subscribe to events
    const handleTransactionStarted = (data) => {
      console.log("Transaction started:", data);
    };

    eventEmitter.on(blockendEventsConstants.TransactionStarted, handleTransactionStarted);

    // Cleanup on unmount
    return () => {
      eventEmitter.off(blockendEventsConstants.TransactionStarted, handleTransactionStarted);
    };
  }, [eventEmitter]);

  return (
    <div>
      <Blockend />
    </div>
  );
}
```

#### Complete Event Listener Setup

```javascript
import React, { useEffect } from "react";
import { useBlockendEvents, blockendEventsConstants } from "@blockend/widget";

function MyApp() {
  const eventEmitter = useBlockendEvents();

  useEffect(() => {
    // Transaction Started Event
    const handleTransactionStarted = (data) => {
      console.log("🚀 Transaction Started:", data);
      // Handle transaction initiation
    };

    // Transaction Steps Event
    const handleTransactionSteps = (data) => {
      console.log("📋 Transaction Steps:", data);
      // Handle step information
    };

    // Transaction Data Event
    const handleTransactionData = (data) => {
      console.log("📄 Transaction Data:", data);
      // Handle transaction data updates
    };

    // Transaction Status Event
    const handleTransactionStatus = (data) => {
      console.log("📊 Transaction Status:", data);
      // Handle status updates
    };

    // Submit Signed Transaction Event
    const handleSubmitSignedTxn = (data) => {
      console.log("✍️ Submit Signed Transaction:", data);
      // Handle transaction submission results
    };

    // Register all event listeners
    eventEmitter.on(blockendEventsConstants.TransactionStarted, handleTransactionStarted);
    eventEmitter.on(blockendEventsConstants.TransactionSteps, handleTransactionSteps);
    eventEmitter.on(blockendEventsConstants.TransactionData, handleTransactionData);
    eventEmitter.on(blockendEventsConstants.TransactionStatus, handleTransactionStatus);
    eventEmitter.on(blockendEventsConstants.SubmitSignedTxn, handleSubmitSignedTxn);

    // Cleanup function
    return () => {
      eventEmitter.all.clear();
    };
  }, [eventEmitter]);

  return (
    <div>
      <Blockend consfiguration={...}/>
    </div>
  );
}
```

### Available Events

#### 1. TransactionStarted

**Event Name:** `blockendEventsConstants.TransactionStarted`

**When Triggered:** When a user initiates a transaction through the widget

```javascript
{
  // Contains the complete route data including:
  routeId: "string",
  fee: [
    {
      amountInToken: "number",
      token: {
        address: "string",
        symbol: "string",
        decimals: number,
        chainId: "string",
        blockchain: "string",
        networkType: "string"
      },
      source: "string"
    }
  ],
  // ... other route properties
}
```

**Example Usage:**

```javascript
eventEmitter.on(blockendEventsConstants.TransactionStarted, (data) => {
  // Track transaction initiation
  analytics.track("Transaction Started", {
    routeId: data.routeId,
    fromToken: data.fromToken,
    toToken: data.toToken,
  });

  // Show loading state
  setTransactionInProgress(true);
});
```

#### 2. TransactionSteps

**Event Name:** `blockendEventsConstants.TransactionSteps`

**When Triggered:** When transaction steps are created or when step-related errors occur

**Data Structure:**

```javascript
// Success case
{
  routeId: "string",
  steps: [
    {
      stepId: "string",
      // ... step details
    }
  ],
  numOfSteps: number
}

// Error case
{
  error: "string",
  routeId: "string",
  status: "error"
}
```

**Example Usage:**

```javascript
eventEmitter.on(blockendEventsConstants.TransactionSteps, (data) => {
  if (data.error) {
    // Handle step creation error
    showNotification("Error creating transaction steps: " + data.error, "error");
    return;
  }

  // Update UI with step information
  setTransactionSteps(data.steps);
  setTotalSteps(data.numOfSteps);
});
```

#### 3. TransactionData

**Event Name:** `blockendEventsConstants.TransactionData`

**When Triggered:** When transaction data is fetched for each step or when data-related errors occur

**Data Structure:**

```javascript
// Success case
{
  data: {
    txnData: {
      txnEvm: "object", // EVM transaction data
      txnType: "string",
      gasless: boolean
    },
    // ... other transaction data
  },
  status: "success"
}

// Error case
{
  error: "string",
  routeId: "string",
  stepId: "string"
}
```

**Example Usage:**

```javascript
eventEmitter.on(blockendEventsConstants.TransactionData, (data) => {
  if (data.error) {
    // Handle transaction data error
    showNotification("Error fetching transaction data: " + data.error, "error");
    return;
  }

  // Process transaction data
  if (data.data?.txnData?.gasless) {
    showNotification("Gasless transaction detected!", "info");
  }

  setCurrentTransactionData(data);
});
```

#### 4. TransactionStatus

**Event Name:** `blockendEventsConstants.TransactionStatus`

**When Triggered:** When polling for transaction status updates

**Data Structure:**

```javascript
{
  data: {
    status: "pending" | "success" | "failed" | "partial-success" | "in-progress",
    // ... other status data
  },
  status: "pending" | "success" | "failed" | "partial-success" | "in-progress",
  isLastStep: boolean
}
```

**Example Usage:**

```javascript
eventEmitter.on(blockendEventsConstants.TransactionStatus, (data) => {
  const { status, isLastStep } = data;

  switch (status) {
    case "pending":
      showNotification("Transaction pending...", "info");
      break;
    case "success":
      if (isLastStep) {
        showNotification("Transaction completed successfully!", "success");
        setTransactionInProgress(false);
      } else {
        showNotification("Step completed, proceeding to next step...", "info");
      }
      break;
    case "failed":
      showNotification("Transaction failed", "error");
      setTransactionInProgress(false);
      break;
    case "partial-success":
      showNotification("Transaction partially completed", "warning");
      break;
    case "in-progress":
      showNotification("Transaction in progress...", "info");
      break;
  }

  // Update progress indicator
  updateTransactionProgress(status, isLastStep);
});
```

#### 5. SubmitSignedTxn

**Event Name:** `blockendEventsConstants.SubmitSignedTxn`

**When Triggered:** When a signed transaction is submitted to the blockchain

**Data Structure:**

```javascript
// Success case
{
  data: {
    // Submission response data
    txnHash: "string",
    // ... other response data
  }
}

// Error case
{
  error: "string",
  status: "Transaction submission failed!"
}
```

**Example Usage:**

```javascript
eventEmitter.on(blockendEventsConstants.SubmitSignedTxn, (data) => {
  if (data.error) {
    // Handle submission error
    showNotification(`Submission failed: ${data.error}`, "error");
    analytics.track("Transaction Submission Failed", { error: data.error });
    return;
  }

  // Handle successful submission
  showNotification("Transaction submitted successfully!", "success");
  analytics.track("Transaction Submitted", {
    txnHash: data.data?.txnHash,
  });

  // Store transaction hash for tracking
  setTransactionHash(data.data?.txnHash);
});
```

### Advanced Usage Patterns

#### Transaction Progress Tracking

```javascript
function TransactionTracker() {
  const eventEmitter = useBlockendEvents();
  const [progress, setProgress] = useState({
    status: "idle",
    currentStep: 0,
    totalSteps: 0,
    error: null,
  });

  useEffect(() => {
    const handleTransactionStarted = () => {
      setProgress((prev) => ({ ...prev, status: "started" }));
    };

    const handleTransactionSteps = (data) => {
      if (data.error) {
        setProgress((prev) => ({ ...prev, error: data.error, status: "error" }));
        return;
      }
      setProgress((prev) => ({ ...prev, totalSteps: data.numOfSteps }));
    };

    const handleTransactionStatus = (data) => {
      if (data.status === "success" && data.isLastStep) {
        setProgress((prev) => ({ ...prev, status: "completed" }));
      } else if (data.status === "failed") {
        setProgress((prev) => ({ ...prev, status: "failed" }));
      }
    };

    eventEmitter.on(blockendEventsConstants.TransactionStarted, handleTransactionStarted);
    eventEmitter.on(blockendEventsConstants.TransactionSteps, handleTransactionSteps);
    eventEmitter.on(blockendEventsConstants.TransactionStatus, handleTransactionStatus);

    return () => {
      eventEmitter.off(blockendEventsConstants.TransactionStarted, handleTransactionStarted);
      eventEmitter.off(blockendEventsConstants.TransactionSteps, handleTransactionSteps);
      eventEmitter.off(blockendEventsConstants.TransactionStatus, handleTransactionStatus);
    };
  }, [eventEmitter]);

  return (
    <div>
      <ProgressBar status={progress.status} currentStep={progress.currentStep} totalSteps={progress.totalSteps} />
      {progress.error && <ErrorMessage message={progress.error} />}
    </div>
  );
}
```

### Best Practices

1. **Always Clean Up Event Listeners**: Make sure to remove event listeners in the cleanup function to prevent memory leaks.
2. **Handle Error Cases**: Always check for error properties in event data and handle them appropriately.
3. **Use Event Constants**: Always use `blockendEventsConstants` instead of hardcoded strings to avoid typos.

### Troubleshooting

#### Events Not Firing

* Ensure you're importing the hook correctly: `import { useBlockendEvents } from '@blockend/widget'`
* Verify that the widget is properly mounted before setting up event listeners
* Check that you're using the correct event constants

#### Memory Leaks

* Always clean up event listeners in useEffect cleanup functions
* Avoid creating new handler functions on every render - use useCallback if necessary

#### TypeScript Errors

* Make sure you have the latest version of the widget package
* Import types properly: `import type { ... } from '@blockend/widget'`
