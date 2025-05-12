# 🎯 Thesis

## Unifying Web3: The Future of Borderless Liquidity

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Source: <a href="https://thirdweb.com/chainlist">https://thirdweb.com/chainlist</a></p></figcaption></figure>

Today, launching a new chain is as easy as ordering food on a delivery app. Every day, new Layer 3s, Layer 4s, rollups, and app chains are cropping up. **The future of a million chains is inevitable.**

### _**A chain is not an island - neither is your dApp.**_ <a href="#block-1aead3a0895f80e4b281c595c8071d69" id="block-1aead3a0895f80e4b281c595c8071d69"></a>

We believe founders should leverage native blockchain ecosystems for their launch, go-to-market strategy, and the technology stack of a particular chain. At the same time, they must enable users from other ecosystems to access their dApps, maximizing their Total Addressable Market (TAM) and reaching a broader audience. Liquidity should be accessible across all ecosystems, regardless of the underlying infrastructure. **Users want the best apps, not a PhD in figuring out which tokens work where.**

Imagine if Gmail users couldn’t email Outlook users. That’s the reality of Web3 today.

* **Users Get Stuck at the Door** – Developers struggle to onboard users who lack the right assets on the right chain.
* **Liquidity is Trapped in Silos** – Assets can’t move freely, making cross-chain activity expensive and inefficient.
* **Expanding Across Chains is a Nightmare** – Cross-chain deployment is slow, costly, and distracts developers from innovation.

### **The Endgame? A Liquidity Marketplace Without Borders.** <a href="#block-1aead3a0895f804a9849fcfeb25638e1" id="block-1aead3a0895f804a9849fcfeb25638e1"></a>

A marketplace thrives by connecting supply and demand, eliminating inefficiencies. Consumers gain choice, competition, and better prices, while providers get instant access to a broader market without the overhead of customer acquisition. This model has dominated Web2, from Amazon to Uber. However, building a marketplace isn’t easy - most fail due to the classic **chicken-and-egg problem**: supply won’t come without demand, and demand won’t exist without supply. So, how do we build a thriving marketplace?

### **First: Bootstrap the supply side** <a href="#block-1aead3a0895f800a8061e900a6e41050" id="block-1aead3a0895f800a8061e900a6e41050"></a>

We build the supply side first by aggregating existing DEXs, bridges, and intent protocols into a unified liquidity layer, abstracting away their quirks. Instead of forcing developers to troubleshoot integrations for years, we make liquidity accessible in one place, instantly.

### **Second: Capture demand at the source** <a href="#block-1aead3a0895f805786a6c6c3d6a99d1c" id="block-1aead3a0895f805786a6c6c3d6a99d1c"></a>

Demand originates **at the application level**. Users don’t start with a liquidity problem. They start with an **application problem,** they want to use a dApp but need specific assets. Instead of forcing users to **search for external liquidity sources**, we **bring liquidity directly into the applications -** capturing **order flow at the source**, **exactly where it’s needed.**

### **Next: Make it convenient at scale** <a href="#block-1aead3a0895f8075a2c8c67c5b781da0" id="block-1aead3a0895f8075a2c8c67c5b781da0"></a>

_Just aggregation isn't cool, you know what's cool? **Convenience**_**.** Amazon scaled beyond its marketplace by making transactions effortless, one-click checkout, Prime delivery and seamless payments. In Web3, liquidity access must evolve the same way.

**Building for the future: Intents based protocols are the next evolution in liquidity movement.**

Instead of manually navigating steps, users simply express what they want. The protocol then broadcasts this intent to a network of solvers, who compete to find the best liquidity path and fulfill the request.

The winning solver fronts their own capital, delivers assets to the user instantly, and later claims reimbursement from the protocol after fulfillment.

_**If intent protocols are the future, why haven’t they solved everything yet?**_

1. **Infrastructure Overhead** – Protocols must deploy and audit smart contracts on every chain, while validators manage nodes, event monitoring, and messaging. This adds complexity and limits support for non-EVM chains.
2. **High Costs** – Frequent cross-chain smart contract calls result in high gas fees. Validator infrastructure and cross-chain messaging further drive up costs, reducing solver margins and increasing user fees.
3. **Locked Liquidity** – Settlement delays due to validation steps and messaging lock solver funds, reducing capital efficiency and restricting liquidity expansion across chains.

_With this foundational understanding, we establish the first layer of our liquidity stack:_

### **Layer 1: Compass Aggregator – Unified Liquidity** <a href="#block-1aead3a0895f806e8096f6e3daad184d" id="block-1aead3a0895f806e8096f6e3daad184d"></a>

A **single integration** that provides access to all liquidity sources across chains. Developers save **months, even years**, of integration overhead. They can onboard more users from any chain, and any token directly through our front-end widgets. While powering advanced use-cases like chain-agnostic trading, DeFi strategies, and AI agents through our comprehensive backend APIs.

### **Layer 2: Pathfinder – One-Click Execution** <a href="#block-1aead3a0895f802a86b3d55375666231" id="block-1aead3a0895f802a86b3d55375666231"></a>

Pathfinder is an **execution engine** that simplifies transactions by abstracting away complexities.

* **One-click execution** – No more signing multiple transactions.
* **Gas abstraction** – No need to manage gas tokens for different chains.
* **Unrestricted liquidity** – No longer limited by a single liquidity provider; Pathfinder combines multiple sources for the best execution.

By leveraging **Compass** to find optimal liquidity routes, even when no single provider can fulfill a request. Pathfinder **unlocks liquidity pathways that wouldn’t otherwise be possible.**

Pathfinder acts as a **solver**, handling order flow exclusively from Compass. While it improves execution and fills gaps in the short term, the expanding landscape demands **more solvers** with diverse execution capabilities. To ensure **optimal pricing and efficiency**, we need an **open marketplace** where solvers compete for order flow.

That’s why we’re building LEX Protocol - an open marketplace that eliminates bottlenecks and scales seamlessly across all chains, regardless of architecture.

### **Layer 3: LEX Protocol – The Open Liquidity Infrastructure** <a href="#block-1aead3a0895f800686bcf0d276879fdb" id="block-1aead3a0895f800686bcf0d276879fdb"></a>

LEX is an **intent-based liquidity protocol** that functions across **all virtual machines**, removing dependencies on smart contracts and cross-chain messaging. It is designed to be:

• **Infrastructure-light**: Works on any chain, regardless of tech architecture.

• **Permissionless**: Any Solver, Chain, liquidity provider can plug in.

• **Capital and cost-efficient**: Faster settlements, cheaper transactions for all participants.

All core functions of LEX protocol execute on a dedicated **high-speed, low-cost settlement layer built on** the **Solana Virtual Machine (SVM),** enabling seamless liquidity movement across chains without constraints. Pathfinder will eventually transition into one of the solvers on LEX competing in the open liquidity marketplace.

### Why Solana? <a href="#block-1aead3a0895f8038ac9efcd4ef827059" id="block-1aead3a0895f8038ac9efcd4ef827059"></a>

While Blockend is a chain-agnostic liquidity infrastructure, Solana was the clear choice for our base layer due to:

• **Scalability & Speed** – Solana’s high-throughput architecture ensures fast, cost-efficient settlements.

• **Strong Community & Ecosystem** – A thriving culture of builders, founders, and innovators committed to long-term growth.

• **Security & Settlement** – LEX runs as an app chain on SVM, executing transactions on the SVM while eventually settling batches on Solana L1.

### Why Blockend? <a href="#block-1aead3a0895f80699292d8ad85989a27" id="block-1aead3a0895f80699292d8ad85989a27"></a>

With 8+ years of experience running a centralized exchange with 250k+ users, and building multiple Web2, Web2.5, and Web3 products, we understand both sides of the equation - the challenges developers face in building applications and the friction users experience when interacting with them.

Our mission is simple: remove inefficiencies so builders can focus on innovation, not infrastructure bottlenecks.

We are building a full-stack open liquidity marketplace - Compass (discovery), Pathfinder (execution), and LEX (scalability) - to power frictionless liquidity movement across all chains, assets, and virtual machines.

One Web3. Any token. Any chain. No barriers.

\
