# Widget Studio

Widget Studio is an interactive configuration tool that helps you customize your Blockend Widget visually. Instead of manually writing JSON configuration objects, you can use the intuitive interface to:

* ✨ Customize themes and colors
* 🎨 Style container and gradients
* ⚙️ Set default chains and tokens
* 📝 Generate configuration code
* 🔄 Import/export configurations

#### Access Widget Studio

Visit our Widget Studio

{% embed url="https://studio.lazy.exchange/" %}

#### Key Features

**Visual Theme Editor**

* Switch between light/dark themes
* Create custom color schemes
* Customize text, background, and border colors
* Real-time preview of changes

**Styling Controls**

* Container border and shadow customization
* Gradient background configuration
* Typography and font settings
* Heading text and positioning

**Configuration Management**

* **Copy Configuration**: Export your settings as JSON
* **Paste Your Config**: Import existing configurations
* **Reset**: Return to default settings
* **Live Preview**: See changes instantly

**Default Chain & Token Setup**

* Set default source and destination chains
* Configure default token addresses
* Prevent same-chain selection errors

#### How to Use

1. **Open Widget Studio** and start with the default configuration
2. **Customize Settings** using the Design tab controls
3. **Preview Changes** in real-time on the widget
4. **Copy Configuration** from the Code tab
5. **Integrate** the configuration into your app

#### Integration Example

After customizing in Widget Studio, copy the generated configuration:

```javascript
import Blockend from "@blockend/widget";
import "@blockend/widget/dist/style.css";

// Configuration generated from Widget Studio
const configuration = {
  integratorId: "YOUR_INTEGRATOR_ID",
  theme: "dark",
  gradientStyle: {
    background: "linear-gradient(135deg, #2CFFE4, #A45EFF)",
    spinnerColor: "#2CFFE4",
    stopColor: "#A45EFF"
  },
  containerStyle: {
    border: "1px solid #353535",
    borderRadius: "12px"
  },
  defaultChains: {
    from: { chainId: "1" },
    to: { chainId: "10" }
  }
  // ... other customizations
};

export const Home = () => {
  return <Blockend configuration={configuration} />;
};
```

#### Import/Export Configurations

**Exporting Configuration**

1. Customize your widget in the Design tab
2. Navigate to the Code tab
3. Click the copy icon to copy the configuration JSON
4. Save it for future use or sharing

**Importing Configuration**

1. Navigate to the Code tab
2. Scroll to "Paste Your Config" section
3. Paste your JSON configuration
4. Click "Paste Your Config" to apply
5. Widget updates instantly with your settings

