---
icon: list-tree
---

# Create Transaction

Once user selects a quote fetched from `/quotes` api, start the transaction by sending a request to `/createTx` api with the `routeId` of the selected quote. Response will contain updated steps for the transaction. These updated steps include any additional steps required to complete the transaction such as, but not limited to, ERC20 approvals.

### Endpoint: `GET /createTx`&#x20;

### Query params

```
/createTx?
    routeId=
```

### Response

```typescript
{
    "steps": Steps[];
}
```

### Example <a href="#exmaple" id="exmaple"></a>

Continuing example from `/quotes` api, let's start the transaction by sending a request to `/createTx` api with the `routeId` of the selected quote.

**Request:**

```
https://api2.bloclend.com/v1/createTx
    ?routeId=01J2WB2ZTWAWXN9K48899CSSVN
```

**Response:** The response for the selected quote now contains 2 steps. Additonal step being the approval step for USDC on Polygon chain.

```json
{
    "status": "success",
    "data": {
        "routeId": "01J2WB2ZTWAWXN9K48899CSSVN",
        "steps": [
            {
                "stepId": "01J2WB3JEB34B0A1SXHT1E3B63",
                "protocolsUsed": [
                    "Blockend"
                ],
                "stepType": "approval",
                "from": {
                    "symbol": "USDC",
                    "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                    "priceId": "usd-coin",
                    "blockchain": "Polygon",
                    "decimals": 6,
                    "address": "0x3c499c542cef5e3811e1192ce70d8cc03d5c3359",
                    "networkType": "evm",
                    "isNative": false,
                    "isPopular": false,
                    "chainId": "137",
                    "name": "USDC",
                    "lastPrice": 1.002
                },
                "to": {
                    "symbol": "USDC",
                    "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                    "priceId": "usd-coin",
                    "blockchain": "Polygon",
                    "decimals": 6,
                    "address": "0x3c499c542cef5e3811e1192ce70d8cc03d5c3359",
                    "networkType": "evm",
                    "isNative": false,
                    "isPopular": false,
                    "chainId": "137",
                    "name": "USDC",
                    "lastPrice": 1.002
                },
                "inputAmount": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
                "outputAmount": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
                "fee": [
                    {
                        "type": "network",
                        "token": {
                            "networkType": "evm",
                            "chainId": "137",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Matic",
                            "symbol": "MATIC",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
                            "priceId": "matic-network",
                            "blockchain": "Polygon",
                            "lastPrice": 0.546532
                        },
                        "amount": "1800000001440000",
                        "amountInEther": "1800000001440000",
                        "amountInUSD": "0.0009837576007870061"
                    }
                ]
            },
            {
                "stepId": "01J2WB2ZTWXW6D39KJN7AAB3VV",
                "stepType": "bridge",
                "protocolsUsed": [
                    "deBridge"
                ],
                "provider": "dln",
                "from": {
                    "symbol": "USDC",
                    "image": "https://assets.coingecko.com/coins/images/6319/small/usdc.png",
                    "priceId": "usd-coin",
                    "blockchain": "Polygon",
                    "decimals": 6,
                    "address": "0x3c499c542cef5e3811e1192ce70d8cc03d5c3359",
                    "networkType": "evm",
                    "isNative": false,
                    "isPopular": false,
                    "chainId": "137",
                    "name": "USDC",
                    "lastPrice": 1.002
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
                            "chainId": "137",
                            "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
                            "decimals": 18,
                            "name": "Matic",
                            "symbol": "MATIC",
                            "isNative": true,
                            "isPopular": true,
                            "image": "https://assets.coingecko.com/coins/images/4713/standard/polygon.png?1698233745",
                            "priceId": "matic-network",
                            "blockchain": "Polygon",
                            "lastPrice": 0.546483
                        },
                        "amount": "518000000014400000",
                        "amountInEther": "518000000014400000",
                        "amountInUSD": "0.28307819400786943"
                    }
                ],
                "inputAmount": "3000000",
                "outputAmount": "1523800",
                "estimatedTimeInSeconds": 1
            }
        ]
    }
}
```
