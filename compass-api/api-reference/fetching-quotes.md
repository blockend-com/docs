---
icon: server
---

# Fetching Quotes

Flow of a transaction starts with fetching quotes for it.

### Endpoint: `GET /quotes`&#x20;

```url
/quotes
    ?fromChainId=
    &fromAssetAddress=
    &toChainId=
    &toAssetAddress=
    &inputAmountDisplay=
    &userWalletAddress=
    &recipient=
```

### Query params <a href="#example" id="example"></a>

<table><thead><tr><th width="221">Field</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td>fromChainId*</td><td>string</td><td>Source blockchain identifier</td></tr><tr><td>fromAssetAddress*</td><td>string</td><td>Token address on source chain</td></tr><tr><td>toChainId*</td><td>string</td><td>Destination blockchain identifier</td></tr><tr><td>toAssetAddress*</td><td>string</td><td>Token address on destination chain</td></tr><tr><td>inputAmountDisplay*</td><td>string</td><td>Human-readable input amount (e.g., “1.5”) <br><strong>Note:</strong> Either inputAmountDisplay or inputAmount should be passed</td></tr><tr><td>inputAmount*</td><td>string</td><td>Input amount in decimals units<br><strong>Note:</strong> Either inputAmountDisplay or inputAmount should be passed</td></tr><tr><td>userWalletAddress*</td><td>string</td><td>User’s wallet address</td></tr><tr><td>recipient</td><td>string</td><td>Final recipient of the tokens. If not provided userWalletAddress is used by default </td></tr><tr><td>slippage</td><td>number</td><td>Slippage tolerance in basis points (100 = 1%)</td></tr><tr><td>solanaOptions</td><td>SolanaOptions</td><td>Solana-specific parameters</td></tr><tr><td>evmOptions</td><td>EvmOptions</td><td>EVM-specific parameters</td></tr><tr><td>skipChecks</td><td>boolean</td><td>Skip validation checks</td></tr><tr><td>include</td><td>string</td><td>Comma-separated list of providers to include</td></tr><tr><td>exclude</td><td>string</td><td>Comma-separated list of providers to exclude</td></tr><tr><td>recommendedProvider</td><td>boolean</td><td>Use only recommended providers</td></tr></tbody></table>

**Solana Options**

<table><thead><tr><th width="187">Field</th><th width="198">Type</th><th>Description</th></tr></thead><tbody><tr><td>solanaPriorityFee</td><td>number | PriorityLevel</td><td>Priority fee in micro lamports or priority level</td></tr><tr><td>solanaJitoTip</td><td>number | PriorityLevel</td><td>Jito MEV tip in lamports or priority level</td></tr></tbody></table>

**Evm Options**

<table><thead><tr><th width="185">Field</th><th width="202">Type</th><th>Description</th></tr></thead><tbody><tr><td>evmPriorityFee</td><td>number | PriorityLevel</td><td>Priority fee in wei or priority level</td></tr></tbody></table>

```typescript
PriorityLevel = 'low' | 'medium' | 'high' | 'ultra' | 'degen'
```

### Response <a href="#example" id="example"></a>

A Route represents a complete path for token transfer, including all necessary steps and fee information.

<table><thead><tr><th width="243">Field</th><th width="146">Type</th><th>Description</th></tr></thead><tbody><tr><td>requestId</td><td>string</td><td>Unique identifier for the quote request</td></tr><tr><td>routeId</td><td>string</td><td>Unique identifier for this specific route</td></tr><tr><td>from</td><td>Asset</td><td>Source token details including chain and token information</td></tr><tr><td>to</td><td>Asset</td><td>Destination token details</td></tr><tr><td>steps</td><td>Steps[]</td><td>Array of execution steps (approval, swap, bridge, etc.)</td></tr><tr><td>fee</td><td>Fee[]</td><td>Breakdown of all fees involved</td></tr><tr><td>provider</td><td>Providers</td><td>Liquidity provider identifier</td></tr><tr><td>providerDetails</td><td>ProviderDetails</td><td>Additional provider information</td></tr><tr><td>protocolsUsed</td><td>string[]</td><td>List of protocols used in this route</td></tr><tr><td>inputAmount</td><td>string</td><td>Input amount in wei/native units</td></tr><tr><td>inputAmountDisplay</td><td>string</td><td>Human-readable input amount</td></tr><tr><td>outputAmount</td><td>string</td><td>Expected output in wei/native units</td></tr><tr><td>outputAmountDisplay</td><td>string</td><td>Human-readable output amount</td></tr><tr><td>minOutputAmount</td><td>string</td><td>Minimum output amount after slippage</td></tr><tr><td>minOutputAmountDisplay</td><td>string</td><td>Human-readable minimum output</td></tr><tr><td>slippage</td><td>number</td><td>Applied slippage tolerance in basis points</td></tr><tr><td>userWalletAddress</td><td>string</td><td>User's wallet address</td></tr><tr><td>recipient</td><td>string</td><td>Final recipient address</td></tr><tr><td>createdAt</td><td>number</td><td>Unix timestamp of quote creation</td></tr><tr><td>deadline</td><td>number</td><td>Unix timestamp when quote expires</td></tr><tr><td>estimatedTimeInSeconds</td><td>number</td><td>Estimated execution time</td></tr><tr><td>tags</td><td>string[]</td><td>Route classification tags</td></tr></tbody></table>

### Example <a href="#example" id="example"></a>

The following example shows how to get quotes for a cross-chain swap transaction from Ethereum to Solana. We'll be fetching quotes for ETH on Ethereum to USDC on Solana.

**Request:**

```url
https://api2.blockend.com/v1/quotes
    ?fromChainId=1
    &fromAssetAddress=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
    &toChainId=sol
    &toAssetAddress=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v
    &inputAmountDisplay=0.420
    &userWalletAddress=0x17e7c3DD600529F34eFA1310f00996709FfA8d5c
    &recipient=7zSa114U45nJ8b8ALsuhszS2Kxto8grgedh65Q7xJYii
```

**Response:**

```json
{
    "status": "success",
    "data": {
        "quotes": [
            {
                "routeId": "01J2WB1NY6MD3F25CJTTB01D8F",
                "from": {
                    "networkType": "evm",
                    "chainId": "1",
                    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                    "decimals": 18,
                    "name": "Ethereum",
                    "symbol": "ETH",
                    "isNative": true,
                    "isPopular": true,
                    "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                    "priceId": "ethereum",
                    "blockchain": "Ethereum",
                    "lastPrice": 3480.62
                },
                "to": {
                    "symbol": "USDC",
                    "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                    "priceId": "usd-coin",
                    "blockchain": "Solana",
                    "decimals": 6,
                    "address": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                    "networkType": "sol",
                    "isNative": false,
                    "isPopular": false,
                    "chainId": "sol",
                    "name": "USDC",
                    "lastPrice": 1.002
                },
                "steps": [
                    {
                        "stepId": "01J2WB1NY64MKVCM8FM994WMZV",
                        "stepType": "bridge",
                        "protocolsUsed": [
                            "Auction"
                        ],
                        "provider": "mayan",
                        "from": {
                            "networkType": "evm",
                            "chainId": "1",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Ethereum",
                            "symbol": "ETH",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                            "priceId": "ethereum",
                            "blockchain": "Ethereum",
                            "lastPrice": 3480.62
                        },
                        "to": {
                            "symbol": "USDC",
                            "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                            "priceId": "usd-coin",
                            "blockchain": "Solana",
                            "decimals": 6,
                            "address": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                            "networkType": "sol",
                            "isNative": false,
                            "isPopular": false,
                            "chainId": "sol",
                            "name": "USDC",
                            "lastPrice": 1.002
                        },
                        "fee": [
                            {
                                "token": {
                                    "networkType": "evm",
                                    "chainId": "1",
                                    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                                    "decimals": 18,
                                    "name": "Ethereum",
                                    "symbol": "ETH",
                                    "isNative": true,
                                    "isPopular": true,
                                    "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                                    "priceId": "ethereum",
                                    "blockchain": "Ethereum",
                                    "lastPrice": 3480.62
                                },
                                "amount": "1380475027400000",
                                "amountInEther": "1380475027400000",
                                "amountInUSD": "4.804908989868988",
                                "type": "network"
                            }
                        ],
                        "inputAmount": "420000000000000000",
                        "outputAmount": "1459244847",
                        "estimatedTimeInSeconds": 900
                    }
                ],
                "fee": [
                    {
                        "token": {
                            "networkType": "evm",
                            "chainId": "1",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Ethereum",
                            "symbol": "ETH",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                            "priceId": "ethereum",
                            "blockchain": "Ethereum",
                            "lastPrice": 3480.62
                        },
                        "amount": "1380475027400000",
                        "amountInEther": "1380475027400000",
                        "amountInUSD": "4.804908989868988",
                        "type": "network"
                    }
                ],
                "provider": "mayan",
                "providerDetails": {
                    "id": "mayan",
                    "name": "Mayan",
                    "logoUrl": "https://blockend-widget.s3.ap-south-1.amazonaws.com/mayan.svg",
                    "websiteUrl": "https://mayan.finance/"
                },
                "protocolsUsed": [
                    "Auction"
                ],
                "inputAmount": "420000000000000000",
                "inputAmountDisplay": "0.420",
                "outputAmount": "1459244847",
                "outputAmountDisplay": "1459.244847",
                "minOutputAmount": "1459.098923",
                "minOutputAmountDisplay": "1459.098923",
                "slippage": 0,
                "recipient": "7zSa114U45nJ8b8ALsuhszS2Kxto8grgedh65Q7xJYii",
                "createdAt": 1721085515718,
                "deadline": 60,
                "estimatedTimeInSeconds": 900,
                "requestId": "01J2WB1K2D769PJRBMPNX1SKJT",
                "score": {
                    "outputScore": 1,
                    "speedScore": 0.0011111111111111111,
                    "feeScore": 1,
                    "slipparageScore": 0,
                    "stepScore": 1,
                    "outputDiffPercent": 0
                },
                "tags": [
                    "BEST_OUTPUT",
                    "CHEAP"
                ]
            },
            {
                "routeId": "01J2WB1MX7ZDHNB71GH3R52189",
                "from": {
                    "networkType": "evm",
                    "chainId": "1",
                    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                    "decimals": 18,
                    "name": "Ethereum",
                    "symbol": "ETH",
                    "isNative": true,
                    "isPopular": true,
                    "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                    "priceId": "ethereum",
                    "blockchain": "Ethereum",
                    "lastPrice": 3480.62
                },
                "to": {
                    "symbol": "USDC",
                    "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                    "priceId": "usd-coin",
                    "blockchain": "Solana",
                    "decimals": 6,
                    "address": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                    "networkType": "sol",
                    "isNative": false,
                    "isPopular": false,
                    "chainId": "sol",
                    "name": "USDC",
                    "lastPrice": 1.002
                },
                "steps": [
                    {
                        "stepId": "01J2WB1MX7BJA7ZSN8K4KXF1VA",
                        "stepType": "bridge",
                        "protocolsUsed": [
                            "deBridge"
                        ],
                        "provider": "dln",
                        "from": {
                            "networkType": "evm",
                            "chainId": "1",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Ethereum",
                            "symbol": "ETH",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                            "priceId": "ethereum",
                            "blockchain": "Ethereum",
                            "lastPrice": 3480.62
                        },
                        "to": {
                            "symbol": "USDC",
                            "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                            "priceId": "usd-coin",
                            "blockchain": "Solana",
                            "decimals": 6,
                            "address": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                            "networkType": "sol",
                            "isNative": false,
                            "isPopular": false,
                            "chainId": "sol",
                            "name": "USDC",
                            "lastPrice": 1.002
                        },
                        "fee": [
                            {
                                "type": "network",
                                "token": {
                                    "networkType": "evm",
                                    "chainId": "1",
                                    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                                    "decimals": 18,
                                    "name": "Ethereum",
                                    "symbol": "ETH",
                                    "isNative": true,
                                    "isPopular": true,
                                    "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                                    "priceId": "ethereum",
                                    "blockchain": "Ethereum",
                                    "lastPrice": 3480.62
                                },
                                "amount": "5141425082200000",
                                "amountInEther": "5141425082200000",
                                "amountInUSD": "17.895346969606965"
                            }
                        ],
                        "inputAmount": "420000000000000000",
                        "outputAmount": "1449243299",
                        "estimatedTimeInSeconds": 1
                    }
                ],
                "fee": [
                    {
                        "type": "network",
                        "token": {
                            "networkType": "evm",
                            "chainId": "1",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Ethereum",
                            "symbol": "ETH",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/279/standard/ethereum.png?1595348880",
                            "priceId": "ethereum",
                            "blockchain": "Ethereum",
                            "lastPrice": 3480.62
                        },
                        "amount": "5141425082200000",
                        "amountInEther": "5141425082200000",
                        "amountInUSD": "17.895346969606965"
                    }
                ],
                "provider": "dln",
                "providerDetails": {
                    "id": "dln",
                    "name": "DLN",
                    "logoUrl": "https://dln.trade/assets/images/favicon/apple-touch-icon.png",
                    "websiteUrl": "https://dln.trade/"
                },
                "protocolsUsed": [
                    "deBridge"
                ],
                "inputAmount": "420000000000000000",
                "inputAmountDisplay": "0.420",
                "outputAmount": "1449243299",
                "outputAmountDisplay": "1449.243299",
                "minOutputAmount": "1449.243299",
                "minOutputAmountDisplay": "1449.243299",
                "slippage": 0.3,
                "recipient": "7zSa114U45nJ8b8ALsuhszS2Kxto8grgedh65Q7xJYii",
                "createdAt": 1721085514664,
                "deadline": 30,
                "estimatedTimeInSeconds": 1,
                "requestId": "01J2WB1K2D769PJRBMPNX1SKJT",
                "score": {
                    "outputScore": 0.9932454038279075,
                    "speedScore": 1,
                    "feeScore": 0.26850046540195793,
                    "slipparageScore": 0,
                    "stepScore": 1,
                    "outputDiffPercent": 0.00677748576178394
                },
                "tags": [
                    "BEST",
                    "FAST"
                ]
            }
        ]
    }
}
```
