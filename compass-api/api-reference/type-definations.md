---
icon: codepen
---

# Type Definations

### Basic Types

```typescript
type NetworkType = "evm" | "sol" | "cosmos" | "tron";

type Asset = {
  networkType?: NetworkType;
  chainId: string;
  address: string;
  decimals: number;

  symbol: string;
  name?: string;
  isNative?: boolean;
  isPopular?: boolean;
  image?: string;

  blockchain: string;
  lastPrice: number;
  marketCap?: number;
};

type Chain = {
  chainId: string;
  symbol: string;
  name: string;
  networkType: NetworkType;

  image: string;
  isPopular?: boolean;
  isEnabled: boolean;

  explorer: {
    token: string; // https://polygonscan.com/token/{tokenAddress}
    txn: string; // https://polygonscan.com/tx/{txnHash}
    address?: string; // https://polygonscan.com/address/{address}
  };
  rpcUrls: string[];
  tokenCount: number;
};

enum FeeType {
  NETWORK = "NETWORK", // gas fee
  PROVIDER = "PROVIDER", // fee charged by the provider
  BLOCKEND = "BLOCKEND", // fee charged by blockend
  INTEGRATOR = "INTEGRATOR", // custom fee someone want to charge via blockend integration
};

enum FeeSource {
  FROM_SOURCE_WALLET = "FROM_SOURCE_WALLET",
  FROM_OUTPUT_AMOUNT = "FROM_OUTPUT_AMOUNT",
  FROM_INPUT_AMOUNT = "FROM_INPUT_AMOUNT",
};

type Fee = {
  type: FeeType;
  token: Asset;
  source: FeeSource;
  amountInToken: string;
  amountInUSD: number | string;
};

type ProviderDetails = {
  name: string;
  logoUrl: string;
};

type StepType = "approval" | "swap" | "bridge" | "sign" | "claim";
type TxnStatus = "not-started" | "in-progress" | "success" | "failed" | "cancelled";
```

### Quote Request

```typescript
type QuoteRequest = {
  fromChainId: string; // chainId of the asset to swap from
  fromAssetAddress: string; // address of the asset to swap from

  toChainId: string; // chainId of the asset to swap to
  toAssetAddress: string; // address of the asset to swap to

  // send either inputAmountDisplay or inputAmount
  inputAmountDisplay: string; // 3.2
  inputAmount: string; // 3.2*10^18

  userWalletAddress: string; // address of the user who will perform the swap
  recipient?: string; // address of the recipient (in case recipient is different)
  
  slippage: number; // in bps, 100bps = 1%
  solanaOptions?: SolanaOptions;
  evmOptions?: EvmOptions;
  skipChecks?: boolean;
};

type QuoteResponse = {
  quotes: Routes[];
};

export type Route = {
  requestId?: string;
  routeId: string;

  from: Asset;
  to: Asset;
  steps: Steps[];
  fee: Fee[];

  provider: Providers;
  providerDetails: ProviderDetails;
  protocolsUsed: string[];

  inputAmount: string; // 3.2*10^18
  inputAmountDisplay: string; // 3.2

  outputAmount: string; // 4.56*10^18
  outputAmountDisplay: string; // 4.56
  minOutputAmount: string; // 4.32*10^18
  minOutputAmountDisplay?: string; // 4.32
  slippage: number;

  userWalletAddress: string;
  recipient?: string;

  createdAt: number; // time when quote is created
  deadline: number; // deadline (in seconds) for quote to be used
  estimatedTimeInSeconds: number;
  tags?: string[];
};

type Steps = {
  stepId: string;
  stepType: StepType;
  protocolsUsed: string[];
  provider?: Providers;

  from: Asset;
  to: Asset;
  txnHash?: string;
  status?: TxnStatus;

  inputAmount: string;
  outputAmount: string;
  fee: Fee[];
  estimatedTimeInSeconds?: number;
};
```

### Create a transaction

```typescript
type CreateTxnRequest = {
  routeId: string;
};

type CreateTxnResponse = {
  routeId: string;
  steps: Steps[];
};
```

### Transaction Data

```typescript
type NextTxnRequest = {
  routeId: string;
  stepId: string;
};

type NextTxnResponse = {
  routeId: string;
  stepId: string;
  txnData: TxnData | null;
  skipTxn?: boolean;
};

type TxnData = TxnMetaData & {
  txnEvm?: TxnEvm;
  txnSol?: TxnSol;
  txnTron?: TxnTron;
  txnCosmos?: TxnCosmos;
};

type TxnEvm = {
  from: string | null;
  to: string;
  value?: string | null;
  data?: string | null;

  gasPrice?: string | null;
  gasLimit?: string | null;
};

type TxnSol = {
  data: string;
};

type TxnTron = {
  raw_data?: any | null;
  raw_data_dex?: string | null;
  txID: string;
  visible: boolean;
};

type TxnCosmos = {
  data: string; 
  value: string;
  gasLimit: string;
  gasPrice: string;
  maxFeePerGas: string;
};

type TxnMetaData = {
  requestId: string;
  routeId: string;
  stepId: string;
  networkType: NetworkType;

  deadline?: number;
  skipTxn?: boolean;
};
```

### Check status of a transaction

```typescript
type StatusCheckRequest = {
  routeId: string;
  stepId: string;
  txnHash: string;
};

type StatusCheckResponse = {
  routeId: string;
  stepId: string;
  status: TxnStatus;
  srcTxnHash?: string;
  srcTxnUrl?: string;
  destTxnHash?: string;
  destTxnUrl?: string;
  points?: number;
};
```
