---
description: >-
  This page addresses the possible errors while setting up the widget in Next js
  applications
---

# Next js issues

## 1. Self is not defined or HTMLElement not defined

You may encounter Server Error... ReferenceError: self is not defined in your Next JS app, this is because blockend requires web apis to work and the web apis are not available on the server side when next js renders a page, in order to avoid this you can start by importing the Blockend Widget like below

<pre><code><strong>const Blockend = dynamic(() => import("blockend"), {
</strong>  ssr: false,
});
import "blockend/dist/style.css";
</code></pre>

