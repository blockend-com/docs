---
description: >-
  This page addresses the possible errors while setting up the widget in CRA or
  Vite applications
---

# React issues

## 1. RPC websocket

```
Class extends value /static/media/client.98866d6736d9f36d61b8.cjs is not a constructor or null
TypeError: Class extends value /static/media/client.98866d6736d9f36d61b8.cjs is not a constructor or null
```

If you encounter the above error, add this code to your package.json file

```
"resolutions": {
    "rpc-websockets": "7.10.0",
    "@solana/web3.js": "1.91.6"
}
```

for more details please refer [https://docs.dynamic.xyz/troubleshooting/react/cannot-resolve-rpc-websockets](https://docs.dynamic.xyz/troubleshooting/react/cannot-resolve-rpc-websockets)
