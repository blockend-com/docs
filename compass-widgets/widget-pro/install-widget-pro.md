---
icon: arrow-down-to-line
---

# Install Widget Pro

## Getting started with Blockend Widget

Blockend Widget is available as an [npm package](https://www.npmjs.com/package/blockend) along with several required dependencies for full functionality.

**Package Dependencies Overview**

The widget requires the following dependency packages:\


* **@dynamic-labs**: Provides wallet connection for Solana blockchain.
* **@cosmjs**: Enables interaction with Cosmos-based blockchains
* graz: Provides react hooks for Cosmos wallet interaction.
* wagmi,viem: Enables interaction with EVM blockchain

**Installation Commands**

Choose your preferred package manager:

**npm:**

```sh
npm install @blockend/widget wagmi viem @tanstack/react-query @dynamic-labs/sdk-react-core @dynamic-labs/solana graz graz-sh/types @cosmjs/cosmwasm-stargate @cosmjs/proto-signing @cosmjs/stargate
```

**yarn:**

```sh
yarn add @blockend/widget wagmi viem @tanstack/react-query @dynamic-labs/sdk-react-core @dynamic-labs/solana graz graz-sh/types @cosmjs/cosmwasm-stargate  @cosmjs/proto-signing @cosmjs/stargate 
```

> **Note**: All dependencies are required for full functionality of the widget across different blockchain networks and wallet types.

Integrating widget to your dapp or webiste is very easy. It takes a full 3 line of code to integrate and start using Blockend Widget, now that's a lot of work for a human dev.

### 1. Import Widget dependencies

In your react app, start by importing the Blockend Widget and its styles

```jsx
import Blockend from "@blockend/widget";
import "@blockend/widget/dist/style.css";
```

You may encounter Server Error... ReferenceError: self is not defined in your Next JS app, this is because blockend requires web apis to work and the web apis are not available on the server side when next js renders a page, in order to avoid this you can start by importing the Blockend Widget like below

```jsx
import dynamic from "next/dynamic";
const Blockend = dynamic(() => import("@blockend/widget"), {
  ssr: false,
});
import "blockend/dist/style.css";
```

### 2. Initialize the Widget

Add the widget component to your app

```jsx
<Blockend />
```

### Integrator Id (Required)

Unique identifier assigned to each integration partner. It is used to track and manage various integrations within our system.Error will be thrown if this field is empty.

```jsx
const configuration = {
  integratorId:""
  ...
};
<Blockend  configuration={configuration} />
```

This id will be added in the request header of api calls that is made by the widget.

And that is it, you have successfully integrated the Blockend Widget.

### 3. (optional) Customizing the Widget

As an optional step, you can also customize the look and feel of the widget. This can be done by passing a configuration object as prop when initializing the widget.

```jsx
const configuration = {
    gradientStyle: {
    background: "linear-gradient(#E66465, #9198E5)",
    spinnerColor: "#E66465",
    stopColor: "#9198E5",
  },
  containerStyle:{
    background:"#000000",
    border:"1px solid #fff",
    boxShadow:"1px 1px 7px 5px rgb(255,255,255,0.1)" ,
  },
  theme:"light",
  customTheme: {
    text: {
      primary: "#808080",
      secondary: "rgba(128, 128, 128, 0.75)",
      placeholder: "#cccccc",
      success: "#49AD71",
      error: "#FD5868",
    },
    background: {
      container: "#FFFFFF",
      secondary: "#E9E9E9",
      card:"#FFFFFF",
      networkCard: "#F6F6F6",
      loaderbar: "#E9E9E9",
      coin:"#E0E0E0",
      rewards:"#eaeaeb33"
    },
    border: {
      primary: "#E0E0E0",
      inputHighlight: "#9FC966",
    },
    fontFamily:'"micro 5 charted"', sans-serif, lato;
    shadow: {
      boxShadow: "1px 1px 7px 5px rgb(255,255,255,0.1)",
    },
  },
};

<Blockend configuration={configuration} />;
```

Full list of configuration options can be found here:

{% content-ref url="customise-widget-pro.md" %}
[customise-widget-pro.md](customise-widget-pro.md)
{% endcontent-ref %}

### Code Example

Here is what a integration of widget on your frontend might look like:

```jsx
import Blockend from "@blockend/widget";
import "@blockend/widget/dist/style.css";

const configuration = {
  gradientStyle: {
    background: "linear-gradient(#e66465, #9198e5)",
    spinnerColor: "#e66465",
    stopColor: "#9198e5",
  },
  defaultChains: {
    from: { chainId: "10" }, // optimism
    to: { chainId: "sol" }, // solana
  },
  defaultTokens: {
    from: { tokenAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee" }, // eth on optimism
    to: { tokenAddress: "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v" }, // usdc on solana
  },
};

export const Home = () => {
  return <Blockend configuration={configuration} />;
};
```

### Supported Tokens

Widget supports a whole array of token which would be difficult to list here, so we have built a handy API just for that

```bash
curl -X GET "https://api2.blockend.com/v1/tokens" \
  -H "accept: application/json"
```

\
You can also pass `chainId` as query parameter to get tokens for a specific chain

```bash
curl -X GET "https://api2.blockend.com/v1/tokens?chainId=sol" \
  -H "accept: application/json"
```

\
You can also get list of supported chains by calling the following endpoint

```bash
curl -X GET "https://api2.blockend.com/v1/chains" \
  -H "accept: application/json"
```
