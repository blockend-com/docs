# Embed Widget

The iframe integration allows you to embed the Blockend widget using the URL `https://www.blockend.com/embedwidget` with optional URL parameters and postMessage API for dynamic configuration.

**Key Features:**

* URL parameters for initial configuration
* PostMessage API for dynamic updates
* Automatic height adjustment
* Theme customization
* Default chain and token selection

**💡 Need help with configuration?** Use the [Widget Studio](https://studio.lazy.exchange/) to generate your widget configuration.

### Quick Start

#### Basic Implementation

```html
<iframe src="https://www.blockend.com/embedwidget?integratorId=YOUR_INTEGRATOR_ID" width="100%" height="600px" frameborder="0"> </iframe>
```

Note:  integratorId is required

#### React Implementation

Dynamically update height if you want the iframe to match the height of the widget.

```jsx
import React, { useEffect, useState, useRef } from "react";

function BlockendWidget() {
  const [height, setHeight] = useState(0);
  const iframeRef = useRef(null);

  useEffect(() => {
    const handleMessage = (event) => {
      if (event.data.type === "BLOCKEND_HEIGHT_UPDATE") {
        setHeight(event.data.height);
      }
    };

    window.addEventListener("message", handleMessage);
    return () => window.removeEventListener("message", handleMessage);
  }, []);
  
useEffect(() => {
    // Request for the widget height
    window.postMessage(
      {
        type: "BLOCKEND_REQUEST_HEIGHT",
      },
      "*"
    );
  }, []);
  
  return <iframe ref={iframeRef} src="https://www.blockend.com/embedwidget?integratorId=YOUR_INTEGRATOR_ID" width="100%" height={height} style={{ border: "none" }} />;
}
```

### URL Parameters

URL parameters provide initial configuration for the widget. These parameters have higher precedence than postMessage configurations.

#### Available Parameters

| Parameter      | Type   | Description                              | Example                                               |
| -------------- | ------ | ---------------------------------------- | ----------------------------------------------------- |
| `integratorId` | string | **Required** - Your unique integrator ID | `integratorId=`YOUR\_INTEGRATOR\_ID                   |
| `theme`        | string | Widget theme (`light` or `dark`)         | `theme=dark`                                          |
| `headingText`  | string | Custom widget heading                    | `headingText=Lazy%20Exchange`                         |
| `fromChain`    | string | Default source chain ID                  | `fromChain=137`                                       |
| `toChain`      | string | Default destination chain ID             | `toChain=1`                                           |
| `fromCoin`     | string | Default source token address             | `fromCoin=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee` |
| `toCoin`       | string | Default destination token address        | `toCoin=0xA0b86a33E6441b8C4C8C8C8C8C8C8C8C8C8C8C8C`   |

#### URL Example

```
https://www.blockend.com/embedwidget?integratorId=YOUR_INTEGRATOR_ID&theme=dark&fromChain=137&headingText=Lazy%20Exchange&fromCoin=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
```

### PostMessage API

The postMessage API enables dynamic configuration and communication between your application and the embedded widget.

#### Message Types

**1. Configuration Message**

Send configuration updates to the widget:

```javascript
// Send configuration to widget
iframe.contentWindow.postMessage(
  {
    type: "BLOCKEND_CONFIG",
    config: {
      theme: "dark",
      defaultChains: {
        from: { chainId: "137" }, // Polygon
        to: { chainId: "1" }, // Ethereum
      },
      defaultTokens: {
        from: { tokenAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee" }, // Native token
        to: { tokenAddress: "0xA0b86a33E6441b8C4C8C8C8C8C8C8C8C8C8C8C8C" }, // USDC
      },
      headingText: "Custom Exchange",
      gradientStyle: {
        background: "linear-gradient(#E66465, #9198E5)",
        spinnerColor: "#E66465",
        stopColor: "#9198E5",
      },
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
          networkCard: "#F6F6F6",
        },
        border: {
          primary: "#E0E0E0",
        },
        fontFamily: '"Arial", sans-serif',
        shadow: {
          boxShadow: "1px 1px 7px 5px rgb(255,255,255,0.1)",
        },
      },
    },
  },
  "*"
);
```

**2. Height Request Message**

Request current widget height:

```javascript
// Request height from widget
iframe.contentWindow.postMessage(
  {
    type: "BLOCKEND_REQUEST_HEIGHT",
  },
  "*"
);
```

#### Received Messages

**1. Configuration Request**

The widget requests configuration on load:

```javascript
window.addEventListener("message", (event) => {
  if (event.data.type === "BLOCKEND_REQUEST_CONFIG") {
    // Send configuration back to widget
    iframe.contentWindow.postMessage(
      {
        type: "BLOCKEND_CONFIG",
        config: {
          // Your configuration here
        },
      },
      "*"
    );
  }
});
```

**2. Configuration Confirmation**

Confirmation that configuration was received:

```javascript
window.addEventListener("message", (event) => {
  if (event.data.type === "BLOCKEND_CONFIG_RECEIVED") {
    console.log("Configuration received by widget:", event.data.success);
  }
});
```

**3. Height Updates**

Automatic height updates from the widget:

```javascript
window.addEventListener("message", (event) => {
  if (event.data.type === "BLOCKEND_HEIGHT_UPDATE") {
    const newHeight = event.data.height;
    // Update iframe height using react state
    setHeight(newHeight);
  }
});
```

### Complete Implementation Example

#### React Component with Full Features

<pre class="language-jsx"><code class="lang-jsx">import React, { useEffect, useState, useRef } from "react";

function BlockendWidget() {
  const [height, setHeight] = useState(600);
  const [isConfigured, setIsConfigured] = useState(false);
  const iframeRef = useRef(null);

  useEffect(() => {
    const handleMessage = (event) => {
      switch (event.data.type) {
        case "BLOCKEND_REQUEST_CONFIG":
          // Send configuration to widget
<strong>          iframeRef.current?.contentWindow?.postMessage(
</strong>            {
              type: "BLOCKEND_CONFIG",
              config: {
                theme: "dark",
                defaultChains: {
                  from: { chainId: "137" },
                  to: { chainId: "1" },
                },
                defaultTokens: {
                  from: { tokenAddress: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee" },
                  to: { tokenAddress: "0xA0b86a33E6441b8C4C8C8C8C8C8C8C8C8C8C8C8C" },
                },
                headingText: "My Exchange",
                gradientStyle: {
                  background: "linear-gradient(#E66465, #9198E5)",
                  spinnerColor: "#E66465",
                  stopColor: "#9198E5",
                },
              },
            },
            "*"
          );
          break;

        case "BLOCKEND_CONFIG_RECEIVED":
          setIsConfigured(event.data.success);
          break;

        case "BLOCKEND_HEIGHT_UPDATE":
          // Widget responds with the height
          setHeight(event.data.height);
          break;
      }
    };

    window.addEventListener("message", handleMessage);
    return () => window.removeEventListener("message", handleMessage);
  }, []);

  useEffect(() => {
    // Request for the widget height
    window.postMessage(
      {
        type: "BLOCKEND_REQUEST_HEIGHT",
      },
      "*"
    );
  }, []);

  return (
    &#x3C;div className="widget-container">
      &#x3C;iframe
        ref={iframeRef}
        src="https://blockend.com/embedwidget?integratorId=YOUR_INTEGRATOR_ID&#x26;theme=dark"
        width="100%"
        height={height}
        style={{
          border: "none",
          borderRadius: "12px",
          boxShadow: "0 4px 6px rgba(0, 0, 0, 0.1)",
        }}
        title="Blockend Widget"
      />
    &#x3C;/div>
  );
}

export default BlockendWidget;
</code></pre>

For easy configuration generation, use the [Widget Studio](https://studio.lazy.exchange/) to:

* Customize widget themes and styling
* Set default chains and tokens
* Preview your widget configuration
* Get ready-to-use configuration code

#### Vanilla JavaScript Implementation

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blockend Widget Integration</title>
    <style>
      .widget-container {
        max-width: 500px;
        margin: 20px auto;
        padding: 20px;
      }
      .blockend-widget {
        border: none;
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        width: 100%;
      }
    </style>
  </head>
  <body>
    <div class="widget-container">
      <iframe id="blockend-widget" class="widget-iframe" src="https://blockend.com/embedwidget?integratorId=YOUR_INTEGRATOR_ID&theme=dark" height="600"> </iframe>
    </div>

    <script>
      const iframe = document.getElementById("blockend-widget");
      let isConfigured = false;

      window.addEventListener("message", (event) => {
        switch (event.data.type) {
          case "BLOCKEND_REQUEST_CONFIG":
            iframe.contentWindow.postMessage(
              {
                type: "BLOCKEND_CONFIG",
                config: {
                  theme: "dark",
                  defaultChains: {
                    from: { chainId: "137" },
                    to: { chainId: "1" },
                  },
                  headingText: "My Exchange",
                },
              },
              "*"
            );
            break;

          case "BLOCKEND_CONFIG_RECEIVED":
            isConfigured = event.data.success;
            console.log("Widget configured:", isConfigured);
            break;

          case "BLOCKEND_HEIGHT_UPDATE":
            iframe.style.height = `${event.data.height}px`;
            break;
        }
      });

      // Request initial height
      setTimeout(() => {
        window.postMessage(
          {
            type: "BLOCKEND_REQUEST_HEIGHT",
          },
          "*"
        );
      }, 1000);
    </script>
  </body>
</html>
```

For easy configuration generation, use the [Widget Studio](https://studio.lazy.exchange/) to:

* Customize widget themes and styling
* Set default chains and tokens
* Preview your widget configuration
* Get ready-to-use configuration code

### Best Practices

#### 1. Security Considerations

* Always validate message origins in production
* Use specific origins instead of `'*'` when possible
* Sanitize configuration data before sending

```javascript
// Production-ready message handler
window.addEventListener("message", (event) => {
  // Validate origin
  if (event.origin !== "https://www.blockend.com") {
    return;
  }

  // Handle messages
  // ...
});
```

#### 2. Performance Optimization

* Debounce height updates to prevent excessive reflows
* Use `requestAnimationFrame` for smooth height transitions
* Implement proper cleanup in React components

#### 3. Error Handling

```javascript
window.addEventListener("message", (event) => {
  try {
    if (event.data.type === "BLOCKEND_HEIGHT_UPDATE") {
      const height = event.data.height;
      if (typeof height === "number" && height > 0) {
        iframe.style.height = `${height}px`;
      }
    }
  } catch (error) {
    console.error("Error handling widget message:", error);
  }
});
```
