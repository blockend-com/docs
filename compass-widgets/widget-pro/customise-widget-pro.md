---
icon: transporter-6
---

# Customise Widget Pro

## Widget Customization Guide

### Customizing gradient colors

You can customize the gradient colors of the widget by passing `gradientStyle` object in configuration.

```jsx
const configuration = {
  gradientStyle: {
    background: "linear-gradient(#E66465, #9198E5)",
    spinnerColor: "#E66465",
    stopColor: "#9198E5",
  },
  ...
};
```

You can customize the theme of the widget by passing `customTheme` object in configuration.

```jsx
const configuration = {
  // containerStyle will override the styles written for the widget container, containerStyle accepts all the inline style properties.
  containerStyle:{
    background:"#000000", // changes the background color of the widget to black
    border:"1px solid #fff", //adds border to the widget container
    boxShadow:"1px 1px 7px 5px rgb(255,255,255,0.1)" // for adding desired shadow effect to the container.
  },
  theme:"light",  // light or dark, if custom theme is applied then custom theme will override light/dark theme
  customTheme: {
    text: {
      primary: "#808080", // primary color of the theme, this applies to headings, main text, svgs, etc..,
      secondary: "rgba(128, 128, 128, 0.75)",  //secondary color of the theme, this applies to secondary headings, network names, route info ,etc..,
      placeholder: "#cccccc",// to update  placeholder colors like input placeholder,date picker heading etc.,
      success: "#49AD71", // view all routes Higher output color
      error: "#FD5868", // error messages and lower output.
    },
    background: {
      container: "#FFFFFF", // can be used to update the bg color of the widget container, input amount container and routes container.
      secondary: "#E9E9E9", // can be used to update the table cell background on portfolio page.
      card:"#FFFFFF", //can be used to update the card color of the widget
      networkCard: "#F6F6F6", // used as background color for top main networks cards and transaction hash container in tokens section.
      loaderbar: "#E9E9E9", // loader bar color accross the widget.
      coin:"#E0E0E0", // used to update th default background color of the coin and chain icons.
      rewards:"#eaeaeb33" // can be used to update the rewards section background color on history page
    },
    border: {
      primary: "#E0E0E0",// primary border color of the widget, can be used to update the container border,border of coin and chain icons, cards border of the widget.
      inputHighlight: "#9FC966", //inputHighlight is used to update the border color of search tokens input field on select chains page when there are more than 0 results.
    },
    fontFamily:'"micro 5 charted"', sans-serif, lato; // can be used to update the font family of the widget to match the parent site, add the fonts to your site and just pass the font name to the widget.
    shadow: {
      boxShadow: "1px 1px 7px 5px rgb(255,255,255,0.1)", // to add shadow effect to the container and cards.
    },
  },
};
```

### Setting Default Chains and Tokens

Widget gives you the option to set default chains and tokens to be shown to the user. This can be done by passing `defaultChains` and `defaultTokens` in configuration when initializing the widget.\
See list of supported chains and tokens below.

{% content-ref url="../../compass-api/supported-chains.md" %}
[supported-chains.md](../../compass-api/supported-chains.md)
{% endcontent-ref %}

{% content-ref url="../../compass-api/api-reference/get-supported-chains-and-tokens.md" %}
[get-supported-chains-and-tokens.md](../../compass-api/api-reference/get-supported-chains-and-tokens.md)
{% endcontent-ref %}

```jsx
const configuration = {
  defaultChains: {
    from: { chainId: "10" }, // optimism
    to: { chainId: "sol" }, // solana
  },
  defaultTokens: {
    from: { tokenAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee" }, // eth on optimism
    to: { tokenAddress: "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v" }, // usdc on solana
  },
  ...
};
```

Note: token address for native tokens is set to 0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee

### Add custom widget heading

There is an option to customise the widget heading, here is how to do it,

```javascript
const configuration={
   headingText: "", // Text to display as the widget heading - pass the heading as string to display your own heading, default text is LAZY.exchange.
      headingContainerStyles: {// Styles to customize the container that wraps the heading,
        transform: "skewX(0deg)", // Supports all valid CSS style properties.
        left: "0px",
        top: "0px",
      },
      // Styles to customize the heading text itself,
      // Supports all valid CSS style properties.
      headingStyles: {
        transform: "skewX(0deg)",
      },
}
```
