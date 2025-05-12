---
description: >-
  This page addresses the possible errors while setting up the widget in Next js
  applications
---

# Next js issues

## 1. Next.js minification issue

When using the widget with Next.js, you may encounter a variable naming conflict error ("n is already declared") if you have `swcMinify` enabled. This is due to a minification conflict between Next.js's SWC compiler and the widget's bundled code.

To resolve this issue, disable SWC minification in your `next.config.js`:

```
/** @type {import('next').NextConfig} */
const nextConfig = {
  swcMinify: false, // Temporarily disable SWC minification
  // ... other configs
};

export default nextConfig;
```

> **Note**: This is a temporary workaround. We are actively working on resolving this minification conflict in future versions of the widget to ensure full compatibility with Next.js's default settings.

## 2. Self is not defined or HTMLElement not defined

You may encounter Server Error... ReferenceError: self is not defined in your Next JS app, this is because blockend requires web apis to work and the web apis are not available on the server side when next js renders a page, in order to avoid this you can start by importing the Blockend Widget like below

```
onst Blockend = dynamic(() => import("blockend"), {
  ssr: false,
});
import "blockend/dist/style.css";
```

