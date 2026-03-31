# Agentic Commerce: The Emerging Infrastructure for AI-Initiated Transactions

**Last Updated:** March 31, 2026

**Authors:** Research compiled via Claude (Anthropic)

**Status:** Living document — continuously updated

---

## Abstract

Agentic commerce — the paradigm in which autonomous AI agents discover, negotiate, and complete purchases on behalf of human principals — has moved from speculative concept to deployed reality in under eighteen months. As of late March 2026, ChatGPT's shopping experience (pivoting from Instant Checkout to retailer-owned apps via Shopify Agentic Storefronts across 5.6 million merchants) reaches 900 million weekly users, Google's Universal Commerce Protocol (UCP) has the backing of Shopify, Walmart, Target, and PayPal, Stripe's Machine Payments Protocol (MPP) is live across more than 100 services, and Santander and Mastercard have completed Europe's first live end-to-end payment executed by an AI agent within a regulated banking framework. This paper surveys the technical infrastructure, key players, economic models, trust frameworks, regulatory landscape, and open research questions defining this nascent but rapidly consolidating field. McKinsey projects agentic commerce will orchestrate $3–5 trillion in global retail spend by 2030; Bain & Co. estimates $300–500 billion in the U.S. alone (15–25% of e-commerce), while Morgan Stanley projects $190–385 billion (10–20%). The stakes — and the unresolved questions — are enormous.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What is Agentic Commerce](#2-what-is-agentic-commerce)
3. [Core Technical Infrastructure](#3-core-technical-infrastructure)
4. [Key Players](#4-key-players)
5. [Comparison Table](#5-comparison-table)
6. [Economic Model](#6-economic-model)
7. [Market Size & Projections](#7-market-size--projections)
8. [Trust, Security & Fraud](#8-trust-security--fraud)
9. [Regulatory & Legal Landscape](#9-regulatory--legal-landscape)
10. [Developer Ecosystem](#10-developer-ecosystem)
11. [Open Research Questions](#11-open-research-questions)
12. [References](#12-references)

---

## 1. Executive Summary

The first quarter of 2026 marks an inflection point for digital commerce. Three converging developments have made it possible for AI agents — not humans — to be the primary actors in a transaction:

1. **Protocol maturity.** The Machine Payments Protocol (MPP), Universal Commerce Protocol (UCP), Agentic Commerce Protocol (ACP), and x402 have each moved from whitepapers to production deployments.
2. **Network adoption.** Visa's Trusted Agent Protocol and Mastercard's Agentic Token framework provide card-network-level infrastructure for verifying and authorizing agent-initiated payments.
3. **Consumer surface area.** ChatGPT (900M weekly users), Google Gemini, Microsoft Copilot, and Amazon's app each now offer agent-mediated shopping flows — though the model is shifting from fully embedded checkout toward retailer-owned experiences, with Shopify's Agentic Storefronts enabling 5.6 million merchants to sell across ChatGPT, Copilot, Google AI Mode, and Gemini from a single integration.
4. **First regulated agentic payment.** Santander and Mastercard completed Europe's first live end-to-end payment executed by an AI agent within a regulated banking framework (March 2026), validating the operational viability of agent-initiated payments in production banking infrastructure.

This paper is intended as a living reference for engineers, product leaders, investors, and policymakers navigating this transition. It synthesizes publicly available data, protocol specifications, and analyst projections current through March 31, 2026.

---

## 2. What is Agentic Commerce

### 2.1 Definition

Agentic commerce is a model of electronic commerce in which an autonomous software agent — typically powered by a large language model (LLM) — performs some or all of the following on behalf of a human principal:

- **Discovery:** identifying products or services that match a stated or inferred need.
- **Evaluation:** comparing options across price, reviews, availability, and personal preference.
- **Negotiation:** applying coupons, negotiating bundle pricing, or selecting optimal fulfillment.
- **Transaction execution:** submitting payment credentials, confirming the order, and handling post-purchase logistics (tracking, returns, disputes).

The defining characteristic is *delegation with autonomy*: the human sets intent and constraints ("buy the cheapest round-trip flight to Tokyo in April under $900"), and the agent executes the entire workflow, potentially across multiple merchants and payment rails, with minimal or zero human intervention at each step.

### 2.2 Key Characteristics

| Characteristic | Traditional E-Commerce | API Integration | Agentic Commerce |
|---|---|---|---|
| **Actor** | Human with browser | Developer-written code | Autonomous AI agent |
| **Intent specification** | Click-by-click | Hardcoded parameters | Natural language / context |
| **Adaptability** | None (static UI) | Limited (predefined logic) | High (reasoning + tool use) |
| **Payment initiation** | User-initiated | Server-initiated (recurring) | Agent-initiated (delegated) |
| **Error handling** | User retries | Programmatic retry | Agent reasons about alternatives |
| **Cross-merchant** | Manual tab-switching | Per-integration | Unified agent context |

### 2.3 How It Differs from Prior Paradigms

Traditional e-commerce requires a human in the loop at every decision point. API-based integrations (e.g., Zapier, IFTTT, or direct merchant APIs) automate *predefined* workflows but lack the ability to reason about novel situations. Agentic commerce occupies a third position: the agent possesses general reasoning capabilities and can adapt its strategy mid-task — for instance, switching merchants when a product is out of stock, or selecting a different payment method when a card is declined.

The critical innovation is not automation per se but *flexible delegation*. The agent acts as a proxy for the consumer's judgment, not merely their clicks.

---

## 3. Core Technical Infrastructure

### 3.1 Payment Protocols

Five protocols dominate the landscape as of Q1 2026:

**Machine Payments Protocol (MPP)** — Co-authored by Stripe and Tempo, MPP launched on March 18, 2026 as an open specification for agent-to-service payment coordination. It supports fiat payments (cards, BNPL) via Shared Payment Tokens (SPTs) as well as stablecoin settlement. Visa has partnered to enable card-based MPP transactions on its global network. Stripe users can accept MPP payments in a few lines of code via the existing PaymentIntents API. Over 100 services — spanning model providers, developer tools, compute platforms, and data vendors — have integrated MPP as of launch [[1]](#ref1).

**Agentic Commerce Protocol (ACP)** — Developed by OpenAI in collaboration with Stripe, ACP has been live since September 2025. Initially powering ChatGPT's Instant Checkout with a 4% merchant transaction fee, OpenAI pivoted in March 2026 to a retailer-owned checkout model — merchants now handle checkout on their own platforms while ACP powers product discovery and conversational shopping. ACP also underpins Stripe's new checkout experience for Facebook, enabling one-click purchases within Meta ads. Microsoft's Copilot Checkout has adopted ACP as its interoperability standard [[2]](#ref2).

**Universal Commerce Protocol (UCP)** — Announced by Google CEO Sundar Pichai at the National Retail Federation (NRF) conference on January 11, 2026. UCP is an open standard covering the entire shopping journey — discovery, purchase, and post-purchase support. It is backed by Shopify, Etsy, Wayfair, Target, and Walmart, with 20+ additional endorsements. UCP is designed to be compatible with A2A (Agent-to-Agent), ACP, and MCP [[3]](#ref3).

**x402** — Created by Coinbase and launched in May 2025, x402 repurposes the HTTP 402 "Payment Required" status code to embed stablecoin (USDC) payments directly into HTTP request/response cycles. Governed by the x402 Foundation (Coinbase + Cloudflare), it has processed over 119 million transactions on Base and 35 million on Solana, with ~$600M in annualized volume and zero protocol fees. Stripe began routing USDC agent payments via x402 on Base in February 2026 [[4]](#ref4).

**Agent Transaction Protocol (ATXP)** — Launched by Circuit & Chisel (founded by former Stripe and Google leaders) alongside a $19.2M seed round led by Primary Venture Partners and ParaFi Capital (with Stripe participating). ATXP is a web-wide protocol enabling AI agents to handle the commerce lifecycle — from discovery to payment — without human oversight. It supports instant, nested, delegated, and extremely low-cost micropayments between agents, with autonomous tool discovery and real-time decision-making. ATXP is positioned as an HTTP-like backbone for agent commerce: a shared set of message formats and rules for agents to request prices, confirm identity, present authorization, initiate payments, and exchange receipts [[50]](#ref50).

### 3.2 Identity & Authentication for Agents

Agent identity is a prerequisite for trusted commerce. Current approaches include:

- **Visa Trusted Agent Protocol:** Merchants verify that an incoming request originates from an authorized agent (not a bot or scraper). Partners include Microsoft, Shopify, Stripe, and Worldpay. The protocol provides cryptographic attestation of agent identity and delegated authority [[5]](#ref5).
- **Mastercard Agentic Tokens:** Extends the tokenization framework used for mobile payments (Apple Pay, Google Pay) to agents. Dynamic credentials are issued per-agent, per-session, with purchase-intent metadata attached [[6]](#ref6).
- **OAuth 2.0 + MCP:** Many developer-facing integrations use OAuth scopes to grant agents limited permissions (e.g., "can spend up to $50/month on compute resources"). The Model Context Protocol (MCP) provides the tool-use layer through which agents invoke payment actions [[7]](#ref7).
- **Agent Wallets:** Emerging pattern where an agent holds a prepaid balance or stablecoin wallet with cryptographic spending limits enforced on-chain. Tempo and Coinbase have both shipped agent wallet primitives.

### 3.3 Trust Models

Three trust architectures are in competition:

1. **Platform-mediated trust:** The AI platform (OpenAI, Google, Microsoft) acts as the trust anchor. The merchant trusts the platform's vetting of agents; the consumer trusts the platform's checkout flow. This is the dominant model today.
2. **Network-mediated trust:** Visa and Mastercard extend their existing trust frameworks to agent transactions. Merchants already trust card-network guarantees (chargeback protection, fraud screening), and these extend to agent-initiated payments.
3. **Protocol-mediated trust:** x402 and MPP aim for a decentralized model where trust is established through cryptographic proofs, on-chain settlement, and reputation scores rather than centralized intermediaries.

---

## 4. Key Players

### 4.1 Stripe / Tempo — Machine Payments Protocol

Stripe's MPP, co-authored with Tempo (a Layer-1 blockchain co-incubated by Stripe and Paradigm, valued at $5B after a $500M raise from Thrive Capital), is the most infrastructure-level play in the space. Rather than building a consumer-facing agent, Stripe is providing the payment rails that *all* agents can use — what analysts have termed "agentic internet rails." MPP launched on March 18, 2026 alongside Tempo's mainnet. The protocol's core primitive is the *session*: an agent pre-authorizes a spending limit and streams granular micropayments continuously within that session, without an on-chain transaction per interaction. MPP supports fiat (cards, BNPL) via Shared Payment Tokens (SPTs) and stablecoin settlement, is integrated into Stripe's existing PaymentIntents API, and is rail-agnostic — Visa has extended MPP to card-based payments on its global network, and Lightspark has extended it to Bitcoin Lightning [[1]](#ref1).

**Scale and recognition.** Stripe processed **$1.9 trillion in total payment volume in 2025** — a 34% year-over-year increase equivalent to roughly 1.6% of global GDP. Its valuation reached **$159 billion** (up 74% from the prior tender offer), and it now powers more than 5 million businesses, all top AI companies, 90% of the Dow Jones Industrial Average, and 80% of the Nasdaq 100. Stripe was named a **Leader in the Forrester Wave: Merchant Payment Providers, Q1 2026**, receiving the highest possible scores in more criteria than any other evaluated provider — and is the only company named Leader in both the Forrester Recurring Billing (Q1 2025) and Merchant Payment Providers (Q1 2026) evaluations [[19]](#ref19).

**AI billing tools.** On March 2, 2026, Stripe launched new AI-focused metering and billing capabilities within Stripe Billing, designed to help AI companies convert variable model inference costs into sustainable revenue. The tools support granular usage tracking (tokens processed, API calls, agent tasks), multi-provider cost monitoring, and automated markup application — for example, maintaining a consistent 30% margin over raw LLM token costs across providers in real time [[20]](#ref20).

**Agentic Commerce Suite.** Stripe launched the Agentic Commerce Suite, a turnkey solution enabling merchants to make products discoverable and purchasable by AI agents through a single integration. BigCommerce became a launch partner, giving its merchants agentic discovery, agent-powered checkout (with taxes, shipping, and order handoff), merchant-of-record control, and fraud protection via Shared Payment Tokens and Stripe Radar [[51]](#ref51).

**Meta/Facebook checkout.** On March 24, 2026, Stripe launched a new checkout experience for Facebook, powered by ACP, enabling one-click purchases within Facebook ads. Buyers tap "Buy now" on an ad and complete the purchase via a native checkout using their Meta wallet credentials. Businesses opt in through the Stripe Dashboard; Instagram ads support is forthcoming [[52]](#ref52).

**Expanded payment method support.** Stripe expanded Shared Payment Token (SPT) support beyond cards to include network-led agentic payment capabilities — **Mastercard Agent Pay** and **Visa Intelligent Commerce** — as well as BNPL methods including **Affirm and Klarna**, broadening the payment options available for agent-initiated transactions [[53]](#ref53).

**Adaptive Pricing.** Stripe published results from an A/B test across **1.5 million subscription checkout sessions** measuring auto-localized pricing. Average results: **4.7% higher conversion**, **1.9% improvement in authorization rate**, and **5.4% increase in LTV per session**, with some businesses seeing lifts exceeding 30%. Runway (video AI platform) saw a 14% increase in LTV per session and 17.7% more LTV per subscription. Over 500,000 businesses including 16,000+ subscription companies (Cursor, Perplexity, Runway) now use Adaptive Pricing [[21]](#ref21).

### 4.2 OpenAI — Instant Checkout / ACP

OpenAI's approach is vertically integrated: ChatGPT is both the agent interface and the commerce platform. Instant Checkout launched in September 2025 for U.S. users and initially supported Etsy sellers, with Shopify merchants (Glossier, SKIMS, Spanx, Vuori) onboarding soon after. OpenAI's business model charges merchants 4% per completed transaction — a significant margin on top of standard payment processing. With 900M weekly active users, ChatGPT represents arguably the largest single distribution channel for agentic commerce [[2]](#ref2). OpenAI also operates Operator, launched January 2025 as a ChatGPT Pro exclusive, which can autonomously navigate websites, click buttons, fill forms, and complete purchases — partnering with eBay, Instacart, and Etsy.

**Revenue trajectory and agent economics.** In February 2026, OpenAI shared investor materials projecting more than **$280 billion in annual revenue by 2030**, split roughly evenly between consumer and enterprise segments. Agent-driven SaaS replacement is a significant revenue thesis: revenue from agents is expected to grow from $3 billion in 2025 to **$29 billion by 2029**, as AI agents handle tasks currently performed by traditional SaaS tools — filing expenses, scheduling, updating CRM entries, processing invoices. For context, OpenAI generated $13.1 billion in revenue in 2025 and entered 2026 at a $20 billion annualized run rate [[22]](#ref22).

**Computer use breakthrough.** GPT-5.4, released March 5, 2026, is the first AI model to surpass the human expert baseline on the **OSWorld-Verified benchmark**: scoring **75.0%** versus the human baseline of **72.4%**. The improvement trajectory was rapid — GPT-5.2 scored 47.3%, GPT-5.3-Codex reached 64%, and GPT-5.4 hit 75% in approximately nine months. GPT-5.4 also introduced native computer use and Tool Search (a new API feature reducing cost of passing tool descriptions in agent system prompts) [[23]](#ref23).

**ACP open standard.** The Agentic Commerce Protocol (ACP), co-developed with Stripe, is an **open standard (Apache 2.0)** providing three core capabilities: machine-readable product discovery through structured feeds, conversational checkout flows with stateful negotiation, and secure payment handling using delegate tokens. ACP can be implemented as a RESTful HTTP interface or as an MCP server, positioning it as a complementary commerce layer alongside MCP (tool access), A2A (agent coordination), and UCP (Google-native commerce) [[2]](#ref2).

**Instant Checkout pivot.** By March 2026, OpenAI's initial Instant Checkout approach — completing purchases entirely within ChatGPT with a 4% merchant fee — had been sidelined. Only ~30 Shopify merchants were live, onboarding proved arduous, and high-intent browsers showed low conversion rates. OpenAI pivoted to a retailer-owned checkout model: "the initial version of Instant Checkout did not offer the level of flexibility that we aspire to provide, so we're allowing merchants to use their own checkout experiences while we focus our efforts on product discovery." Retailers now build dedicated in-ChatGPT app experiences while handling checkout on their own platforms. Product discovery has been enhanced with visually richer results, side-by-side comparison, conversational refinement, and image-based search [[24]](#ref24).

**Shopify Agentic Storefronts.** On March 24, 2026, Shopify activated Agentic Storefronts for all eligible merchants, making products from **5.6 million stores** discoverable inside ChatGPT, Microsoft Copilot, Google AI Mode, and the Gemini app. The rollout was **opt-out, not opt-in** — most Shopify stores are in the system by default, managed centrally from Shopify Admin with no separate integrations, apps, or additional transaction fees beyond standard processing rates. Buyers complete purchases on the merchant's own storefront through an in-app browser (mobile) or new tab (desktop) [[24b]](#ref24b).

**Walmart in-ChatGPT app.** OpenAI spotlighted Walmart's launch of a dedicated in-ChatGPT experience supporting customer account linking, loyalty program integration, and Walmart-native payments. Web browser access is live; iOS and Android app access is expected to follow [[24]](#ref24).

Target, Sephora, Nordstrom, Lowe's, Best Buy, The Home Depot, and Wayfair have integrated ACP for product discovery.

### 4.3 Google — Universal Commerce Protocol

Google's UCP play leverages its dominance in search and product discovery. UCP enables a "buy button" directly on Google Search, AI Mode, and Gemini, eliminating the need for users to visit merchant websites. The protocol is open and backed by major retailers. Google's monetization model likely extends its existing advertising and shopping commission structures — merchants already pay for Google Shopping placements, and UCP adds a transactional layer. The NRF announcement positioned UCP as compatible with A2A, ACP, and MCP, signaling a standards-diplomacy approach rather than a walled garden [[3]](#ref3).

**UCP March 2026 expansion.** On March 19, 2026, Google published a significant UCP update introducing three new optional capabilities: (1) **Cart** — agents can build multi-item shopping baskets before checkout, moving beyond single-item transactions; (2) **Catalog (Product Discovery)** — real-time queries against retailer inventories for variants, pricing, and stock; and (3) **Identity Linking** — OAuth 2.0-based account connection enabling loyalty pricing, member discounts, and promotional offers to transfer when purchases occur through AI Mode or Gemini. Simplified onboarding via Google Merchant Center was also announced, with Commerce Inc., Salesforce, and Stripe committing to UCP implementation [[25]](#ref25).

**A2A (Agent-to-Agent) protocol.** Google's A2A protocol, announced April 2025 and donated to the Linux Foundation in June 2025, is the agent coordination layer complementing MCP's tool-access layer. A2A enables agents built on different vendors and frameworks to discover capabilities (via JSON "Agent Cards"), manage task lifecycles, exchange context, and negotiate user-experience formats. The protocol builds on HTTP, SSE, and JSON-RPC 2.0. Version 0.3 (July 2025) added gRPC support for low-latency orchestration, signed security cards for cryptographic agent identity, and expanded SDKs across Python, Go, JavaScript, Java, and .NET. Over 150 organizations now support A2A, with founding Linux Foundation partners including Google Cloud, AWS, Cisco, Salesforce, SAP, Microsoft Azure AI, and ServiceNow [[26]](#ref26).

**PayPal UCP integration.** PayPal announced support for UCP in January 2026, committing to become a payment option within Google's new AI-powered checkout experiences. PayPal's **Agent Ready** product — which instantly unlocks millions of existing PayPal merchants for AI surface payments with built-in fraud detection, buyer protection, and dispute resolution — requires no additional technical lift from merchants [[54]](#ref54).

**European expansion.** In March 2026, Nexi Group (a leading European paytech) signed an MoU with Google Cloud to build agentic commerce infrastructure for Europe, supporting UCP and the Agent Payments Protocol (AP2) with cryptographically signed mandates and European regulatory compliance [[27]](#ref27).

### 4.4 Microsoft — Copilot Checkout / Brand Agents

Microsoft launched Copilot Checkout on January 8, 2026 with PayPal, Shopify, and Stripe as payment partners. Two key design decisions differentiate Microsoft's approach: (1) merchants remain the merchant of record — they own the transaction, customer data, and relationship; and (2) Microsoft is shipping "Brand Agents" that let Shopify merchants deploy AI assistants on their own sites. Performance data is compelling: shopping journeys involving Copilot are 33% shorter than traditional search and see a 53% increase in purchases within 30 minutes. When shopping intent is present, Copilot journeys are 194% more likely to convert [[8]](#ref8).

**Microsoft 365 E7 — "Frontier Suite."** Announced March 9, 2026 with a May 1 launch date, M365 E7 is priced at **$99/user/month** — a **65% premium over E5** ($60/user/month). E7 bundles M365 E5, Copilot ($30 standalone), **Agent 365** ($15 standalone — the control plane for observing, governing, and securing AI agents across the organization), and Entra Suite ($12 standalone) with advanced Defender, Intune, and Purview security capabilities. The strategic framing positions Copilot not merely as a productivity tool but as the primary **governance surface** for enterprise AI: all agent activity flows through auditable, policy-enforced channels. **Work IQ**, the cross-application intelligence layer underpinning Copilot and Agent 365, draws from emails, files, meetings, and chats to ensure agent actions are contextually aligned with organizational workflows [[28]](#ref28).

**Copilot Cowork.** Developed in collaboration with Anthropic, Copilot Cowork brings long-running, multi-step agentic work into M365 Copilot. It decomposes complex requests into steps, reasons across tools and files, and carries work forward with visible progress and user-steerable checkpoints. Entered Research Preview in late March 2026 [[29]](#ref29).

**Microsoft Agent Framework.** Microsoft open-sourced (MIT license) the **Microsoft Agent Framework**, merging AutoGen (Microsoft Research's multi-agent conversation project) and Semantic Kernel into a single commercial-grade framework. It supports graph-based workflows for multi-agent orchestration with sequential, concurrent, handoff, and group-chat patterns. Release Candidate shipped for .NET and Python with a 1.0 GA target by end of Q1 2026 [[30]](#ref30).

**Azure AI Foundry Agent Service.** Reached **General Availability on March 16, 2026** with enterprise-grade private networking: BYO VNet with zero public egress, end-to-end private networking through MCP connections, and VNet support for MCP server routing. Additional GA features include expanded MCP authentication (key-based, Entra Agent Identity, Managed Identity, OAuth Identity Passthrough), Voice Live preview, and built-in evaluation/monitoring via Azure Monitor [[31]](#ref31).

### 4.5 Amazon — Buy for Me

Amazon's "Buy for Me" feature, piloted in April 2025 and expanded in March 2026, lets consumers purchase from third-party brand websites without leaving the Amazon app. Powered by Amazon Nova (priced ~75% lower than comparable third-party models on Bedrock) and Anthropic's Claude (Opus 4.6 and Sonnet 4.6 on Bedrock), the agent navigates external checkout flows using encrypted customer credentials. Amazon also developed **Nova Act**, a specialized model for browser-based automation trained via reinforcement learning in synthetic "web gyms," which reached general availability for enterprise workflows [[9]](#ref9).

**Shop Direct formalization.** Amazon's initial "Project Starfish" approach — scraping independent brand websites without consent — generated significant retailer backlash in January 2026 (brands reported unauthorized listings, AI-generated product images, and "ghost orders" from anonymous proxy emails). On March 11, 2026, Amazon pivoted to a consent-based model: merchants connect via third-party feed syndicators (**Feedonomics, Salsify, CEDCommerce**) for automatic catalog, pricing, and inventory synchronization. Shop Direct now includes **over 100 million products from more than 400,000 merchants**, with a direct Amazon merchant portal planned [[32]](#ref32).

**Rufus agentic features.** Amazon's Rufus AI shopping assistant has reached **250+ million users** (monthly active users up 149%, interactions up 210% YoY). In November 2025, Amazon added agentic capabilities including **auto-buy** and **price tracking** — customers set target prices and Rufus automatically purchases when the price drops. Rufus users are reportedly 60% more likely to complete a purchase [[33]](#ref33).

**Legal precedent: Amazon v. Perplexity.** On March 10, 2026, a federal judge granted Amazon a **preliminary injunction** blocking Perplexity AI's Comet browser from using AI agents to access Amazon's site for shopping on behalf of customers. The court found that user consent alone does not override platform authorization requirements under the Computer Fraud and Abuse Act — an early legal milestone for agentic commerce. The 9th Circuit subsequently granted Perplexity a temporary stay on March 16 [[34]](#ref34).

**Amazon-OpenAI partnership.** In February 2026, Amazon committed **up to $50 billion in OpenAI** ($15B initial, $35B conditional), with AWS becoming OpenAI's exclusive third-party cloud distribution provider. Forrester called the pairing a potential winner in consumer agentic commerce, combining the most popular answer engine with the most popular product search engine [[35]](#ref35).

### 4.6 Anthropic — MCP / Computer Use / Claude

Anthropic's contribution is primarily infrastructural rather than consumer-facing. The Model Context Protocol (MCP), announced November 2024 and donated to the Linux Foundation's Agentic AI Foundation (AAIF) in December 2025, has crossed 97 million installs and **34,700+ dependent GitHub projects**, and is now the de facto standard for connecting AI agents to external tools and services — including commerce APIs. Every major AI provider (Claude, GPT-5.4, Gemini) ships MCP-compatible tooling, and OpenAI has announced deprecation of its Assistants API in favor of MCP [[7]](#ref7) [[10]](#ref10).

**MCP specification evolution.** The MCP spec has undergone two major revisions since its initial release. Spec version 2025-03-26 introduced **Streamable HTTP transport** for remote server connections and a comprehensive **OAuth 2.1 authorization framework**. Spec version 2025-06-18 added: (1) **OAuth 2.0 Resource Server classification** with RFC 8707 resource binding; (2) **Elicitation** — servers can ask users for input mid-session via `elicitation/create` requests with JSON schemas, enabling multi-round clarification; (3) **Structured output** (`structuredContent`) — tools return machine-readable JSON objects enabling sophisticated tool chaining; and (4) audio content support. The TypeScript SDK v1.27.1 (February 2026) shipped streaming methods for elicitation and sampling, with auth integration and streaming tool responses described as production-ready [[36]](#ref36).

**Claude Computer Use for Mac.** Announced March 23, 2026, Claude Computer Use gives Claude physical control of a Mac — native apps, browser, files, and dev tools — available as a research preview for Claude Pro and Max subscribers. Claude uses a tool hierarchy: it first tries connectors (dedicated integrations) before falling back to direct mouse, keyboard, and screen control. Paired with **Dispatch** (released the prior week), users can assign tasks from their iPhone, step away, and return to finished work. Safety measures include zero password transmission, per-app permission requests, and configurable app blocklists [[37]](#ref37).

**Open-source infrastructure: buffa and connect-rust.** In March 2026, Anthropic open-sourced two Rust crates: **buffa** (zero-copy Protocol Buffers with first-class editions support — view types borrow string data directly from request buffers, eliminating per-string allocations) and **connect-rust** (a Tower-based ConnectRPC implementation speaking Connect, gRPC, and gRPC-Web, nominated as the canonical Rust ConnectRPC implementation). On decode-heavy workloads, connect-rust + buffa achieves **5.6M records/sec vs. tonic+prost's 4.2M records/sec** — approximately **33% faster decode** — with allocator pressure at 3.6% vs. 9.6% of CPU [[38]](#ref38).

**Revenue and ecosystem scale.** Anthropic reached an estimated **$19 billion in annualized revenue** by March 2026 (up 1,167% year-over-year), with **Claude Code** alone hitting **$2.5 billion ARR** in February 2026. Claude paid subscriptions have more than doubled in 2026, with the majority of new subscribers at the $20/month Pro tier. Anthropic launched **Claude Marketplace**, a limited-preview program enabling enterprises to apply existing Anthropic spend commitments to Claude-powered partner applications (launch partners: GitLab, Harvey, Lovable, Replit, Rogo, Snowflake) with unified invoicing [[55]](#ref55).

**Claude Opus 4.6 C compiler.** In a February 2026 demonstration, **16 Claude Opus 4.6 agent instances** running in parallel Docker containers autonomously built a **100,000-line Rust C compiler** over ~2 weeks (~2,000 Claude Code sessions, ~$20,000 API cost). The compiler supports four architecture backends (x86-64, x86-32, ARM, RISC-V) with a standalone assembler and linker, can **compile and boot Linux kernel 6.9**, and compiles PostgreSQL (all 237 regression tests passing), SQLite, Redis, QEMU, FFmpeg, and DOOM. The project achieved a 99% pass rate on the GCC torture test suite [[39]](#ref39).

### 4.7 Coinbase / Cloudflare — x402

The x402 protocol represents the crypto-native approach to agent payments. By embedding USDC payments into the HTTP layer, x402 eliminates the need for accounts, credit cards, or manual intervention. The protocol has zero fees and is governed by the x402 Foundation. Stripe integrated x402 for on-chain agent payments in February 2026, validating the protocol's relevance beyond crypto-native use cases [[4]](#ref4).

**Foundation expansion.** The x402 Foundation has expanded beyond Coinbase and Cloudflare to include **Google and Visa** as members. Google has integrated x402 into its Agent Payments Protocol (AP2). The Foundation targets a **v1.0 specification by Q3 2026**, after which backward compatibility guarantees will apply. x402 V2 (December 2025) added wallet-based identity, automatic API discovery, dynamic payment recipients, expanded multi-chain and fiat support via CAIP standards, and a modular SDK. Cloudflare is adding a deferred payments scheme in its pay-per-crawl beta [[40]](#ref40).

**MCP integration.** The MCP TypeScript SDK v1.27.1 (February 2026) shipped streaming methods for long-running tool calls, enabling real-time progress reporting and incremental output — critical for agentic payment flows where x402-wrapped MCP tool calls involve payment negotiation, settlement confirmation, and result delivery. The x402-MCP library wraps MCP's streaming HTTP transport to intercept tool calls and handle payment flows transparently [[36]](#ref36).

**Volume reality check.** While x402 has processed 154M+ cumulative transactions across Base and Solana with a headline ~$600M in annualized volume, a CoinDesk investigation (March 11, 2026) found that approximately **half of observed transactions reflect artificial activity** ("gamified" or self-trading) rather than genuine commerce. Recent daily snapshots show approximately 131,000 transactions generating ~$28,000 in volume, with an average payment of ~$0.20. The narrative-vs-reality gap is significant: despite a roughly $7 billion ecosystem valuation, real daily commercial throughput remains modest [[41]](#ref41).

### 4.8 Visa / Mastercard — Network Infrastructure

Both card networks have recognized that agent-initiated payments represent a new transaction category requiring new infrastructure. Visa's Trusted Agent Protocol provides merchant-side verification of authorized agents, while Mastercard's Agentic Token framework extends mobile-payment tokenization to agent contexts. Both are positioning as trust layers that sit beneath any of the higher-level protocols (MPP, ACP, UCP) [[5]](#ref5) [[6]](#ref6).

**Visa Agentic Ready Program.** Launched March 17, 2026, first in Europe, with **21 issuing partners** in Phase 1 — including Barclays, HSBC UK, Nationwide, Revolut, Banco Santander, Commerzbank, and Raiffeisen Bank International. Phase 1 involves banks running agent-initiated transactions in production-grade testing environments alongside selected merchants. Visa is also supporting Stripe's MPP by releasing a card-based MPP spec, SDK, and enabling MPP on the Visa Acceptance Platform. The Visa Intelligent Commerce ecosystem now includes 100+ partners globally (30+ actively building, 20+ agents integrating directly), with Visa predicting **millions of consumers** will use AI agents for purchases by the 2026 holiday season [[42]](#ref42).

**Mastercard Verifiable Intent.** On March 5, 2026, Mastercard and Google jointly introduced **Verifiable Intent**, an open-source cryptographic framework linking consumer identity, spending mandates, and transaction outcomes into tamper-resistant records with selective disclosure. Verifiable Intent issues tokenized credentials that cryptographically bind each agent transaction to a **human-approved spending mandate** specifying allowed merchant categories, spending caps, and time windows. The framework supports recurring agent purchases — agents can re-order supplies or manage subscriptions within mandate parameters without per-transaction human approval. It is protocol-agnostic, aligned with Google's AP2 and UCP, and open-sourced with commitments from Google, Fiserv, IBM, Checkout.com, and Basis Theory. Mastercard Agent Pay has completed live authenticated agentic transactions in Australia, New Zealand, South Korea, and across Latin America and the Caribbean [[43]](#ref43).

**Santander: Europe's first live agentic payment.** On March 2, 2026, Banco Santander and Mastercard completed **Europe's first live end-to-end payment executed by an AI agent** within a regulated banking framework. The transaction used **Mastercard Agent Pay** and was processed through Santander's live payments infrastructure, validating the operational and control framework under real production conditions. The AI agent searched, selected, and completed a purchase on behalf of a user within predefined limits and permissions. The pilot represents the first agentic payment conducted within a regulated banking framework; Santander will now move into extended testing and scaling [[56]](#ref56).

**Visa threat landscape.** Visa PERC identified a **more than 450% increase** in dark web community posts mentioning "AI Agent" over the prior six months, alongside a **25% increase in malicious bot-initiated transactions** globally (40% increase in the U.S.). Visa flagged networks of fraudulent websites deploying conversational AI agents to give sites the appearance of legitimacy, and noted that AI agents' lack of human browsing patterns (no hesitation, mouse movements, or backtracking) undermines traditional behavioral fraud detection signals [[57]](#ref57).

**FIS dual-network partnership.** In January 2026, FIS launched an industry-first agentic commerce offering for banks partnering with **both Visa and Mastercard simultaneously** on authorization, fraud, and customer services for agent-initiated transactions — available to all FIS issuing bank clients by end of Q1 2026 [[44]](#ref44).

### 4.9 Replit — Developer-Facing Agents (Agent 4)

Replit has become one of the most consequential platforms in agentic commerce infrastructure. Its progression from Replit Agent (September 2025) to **Agent 4** (March 2026) represents a step-change in capability and commercial traction.

**Business trajectory:** Replit scaled from $10M to $100M ARR in 9 months on the back of Agent. In March 2026, it closed a **$400M Series D at a $9B valuation** — a 3× increase from its January 2026 valuation of $3B — led by Georgian Partners with participation from Databricks Ventures, Okta Ventures, a16z, Coatue, Y Combinator, and others. The company now serves 40M+ users and has penetration in 85% of the Fortune 500 [[11]](#ref11) [[11b]](#ref11b).

**Agent 4 capabilities:** Released March 2026, Agent 4 meaningfully expands Replit's agentic commerce surface area:
- **MCP-native tool integration:** Agent 4 ships with native integrations to BigQuery, Linear, Slack, Notion, and external commerce APIs via Model Context Protocol
- **Parallel task execution:** Agents can now run concurrent sub-tasks — for commerce applications, this means handling product catalog updates, payment flow testing, and API integration verification in a single session
- **Real-time collaboration:** Human-in-the-loop oversight during autonomous builds reduces error rates in production deployments
- **Stripe and payment API wiring:** Replit agents can autonomously scaffold, configure, and test Stripe Billing, Checkout, and Connect integrations — compressing what previously took a junior engineer days into minutes

**Agent commerce role:** Replit's platform sits at the intersection of *agent tooling* and *agentic commerce execution*. Developers use Replit agents to build commerce infrastructure (storefronts, payment flows, billing engines); enterprises use it to autonomously maintain and extend existing commerce systems. As agents become capable of writing and deploying complete commerce stacks, Replit's platform effectively industrializes agent-driven software commerce.

**Strategic position:** At $9B, Replit is priced as a platform — not a developer tool. The comparison case is Salesforce AppExchange: if Replit becomes the deployment surface for agent-built commerce applications, the long-term revenue opportunity is commission on the commerce that flows through those deployments, not just subscription SaaS.

### 4.10 Emerging Startups

- **Sapiom** ($15.75M seed led by Accel, February 2026; investors include Anthropic, Coinbase Ventures, Menlo Ventures, Okta Ventures): Building the "spend + auth" layer for AI agents to autonomously purchase APIs, compute, software, and data. Founded by Ilan Zerbib (former Director of Engineering for Payments at Shopify). Immediate beachhead is vibe-coding platforms, where agents handle procurement (e.g., signing up for Twilio, paying, copying API keys) without human intervention [[45]](#ref45).
- **Payman AI** ($13.8M total over 3 rounds; investors include Visa and Boost VC): Agent-specific payment wallets with programmable spending rules — segregated USD/USDC wallets per agent, configurable policies (daily limits, per-transaction caps, multi-tier approval chains), payee whitelisting, and human-in-the-loop escalation. SOC 2 and PCI compliant. Partnered with Middlesex Federal Savings in December 2025 for agentic banking [[46]](#ref46).
- **Proxy** (useproxy.ai): Virtual cards and bank accounts purpose-built for AI agents. Each agent or workflow receives its own card with hard spending limits enforced at the card network level ("attestation-before-access"). Ships an **MCP server** for programmatic card issuance and payment execution. Wraps existing Visa/Mastercard rails for universal merchant acceptance [[47]](#ref47).
- **Skyfire** ($9.5M total from a16z crypto, Coinbase, Neuberger Berman, Brevan Howard): Agent identity (**Know Your Agent / KYAPay protocol**) and payment settlement network. In December 2025, demonstrated fully autonomous AI agent purchases using KYAPay and Visa's Intelligent Commerce suite. In March 2026, partnered with F5 for Distributed Cloud Bot Defense integration, distinguishing verified agents from malicious bots [[48]](#ref48).
- **Circuit & Chisel** ($19.2M seed led by Primary Venture Partners and ParaFi Capital; Stripe participating): Building ATXP (Agent Transaction Protocol), a web-wide protocol enabling AI agents to handle the full commerce lifecycle autonomously. Founded by former Stripe and Google leaders. ATXP supports instant, nested, delegated, and extremely low-cost micropayments between agents — capabilities that traditional payment rails cannot support. The protocol provides a shared set of message formats and rules for agents to request prices, confirm identity, present authorization, initiate payments, and exchange receipts with merchants [[50]](#ref50).
- **Flexprice** ($500K seed led by TDV Partners): Open-source billing and metering platform positioned as a Lago/Orb challenger, purpose-built for AI-native companies. Key differentiators: native **programmable credit wallets** (with expiry windows, rollover caps, configurable deduction priority) and a **built-in MCP server** exposing all billing operations as tools that AI assistants can call directly — the first billing platform to ship MCP integration [[49]](#ref49).

---

## 5. Comparison Table

| Company | Product / Protocol | Technical Approach | Business Model | Stage | Key Use Cases | Open / Closed |
|---|---|---|---|---|---|---|
| **Stripe / Tempo** | Machine Payments Protocol (MPP) + Agentic Commerce Suite | Open spec; session-based micropayments; fiat + stablecoin via SPTs; PaymentIntents API; Visa + Lightning + Mastercard Agent Pay + BNPL (Affirm, Klarna); Agentic Commerce Suite; Meta/Facebook checkout | Processing fees (~2.9% + $0.30); $1.9T volume (2025); $159B valuation | GA (March 2026) | Agent-to-service payments, micropayments, compute billing, AI metering, social commerce | Open |
| **OpenAI** | ACP / Retailer Apps | Integrated in ChatGPT; Shopify Agentic Storefronts (5.6M merchants, opt-out); Walmart in-app; Operator web automation; GPT-5.4 native computer use (75% OSWorld) | Pivoted from 4% tx fee to retailer-owned checkout; $280B revenue projected by 2030 | GA (Sept 2025); pivoted to retailer-owned checkout (March 2026) | Consumer shopping, product discovery, booking, SaaS replacement | Open (ACP Apache 2.0) |
| **Google** | UCP + A2A | Open standards; buy button in Search/Gemini; Cart, Catalog, Identity Linking (March 2026); A2A for agent coordination; PayPal UCP integration | Ads/shopping commissions (likely) | GA (Jan 2026; expanded March 2026) | Product discovery, multi-item checkout, post-purchase, agent-to-agent | Open |
| **Microsoft** | Copilot Checkout / Brand Agents / E7 | PayPal + Shopify + Stripe; merchant stays MoR; Agent 365 governance; Copilot Cowork (with Anthropic); Agent Framework (open-source) | Ads revenue; E7 at $99/user/month; merchant tools | GA (Jan 2026; E7 May 2026) | Conversational shopping, brand-owned agents, enterprise governance | Mixed (ACP-compatible; Agent Framework MIT) |
| **Amazon** | Buy for Me / Shop Direct / Rufus | Nova + Claude on Bedrock; Nova Act browser automation; feed-based merchant opt-in (Feedonomics, Salsify, CEDCommerce) | Marketplace commissions; $50B OpenAI partnership | GA (expanding; 100M+ products, 400K+ merchants) | Cross-merchant shopping, auto-buy, price tracking | Closed |
| **Anthropic** | MCP / Computer Use / Claude | Open protocol (MCP v3: OAuth, elicitation, structured output); Mac computer use; buffa/connect-rust (33% faster decode); Claude Marketplace | API usage fees; enterprise licensing; $19B ARR (est. March 2026); Claude Code $2.5B ARR | GA (MCP); Research Preview (Computer Use Mac) | Tool integration, web automation, enterprise agents, infrastructure | Open (MCP) |
| **Coinbase / Cloudflare** | x402 | HTTP 402 + USDC; on-chain settlement (Base, Solana, Stellar); Foundation expanded (Google, Visa); MCP streaming integration | Zero protocol fees; ecosystem play | GA (May 2025); v1.0 spec targeted Q3 2026 | Micropayments, API monetization, pay-per-request | Open |
| **Visa** | Trusted Agent Protocol + Agentic Ready | Cryptographic agent verification; card-network settlement; Agentic Ready testing program (21 EU bank partners) | Network interchange fees | GA (Agentic Ready March 2026) | Agent identity verification, fraud prevention, bank testing | Consortium |
| **Mastercard** | Agent Pay + Verifiable Intent | Dynamic tokenization; cryptographic mandate binding (with Google); purchase-intent metadata; live in AU/NZ/LATAM/KR; **Europe's first live agentic payment** (with Santander, March 2026) | Network interchange fees | GA (Agent Pay live in multiple markets; first regulated banking payment completed) | Tokenized agent payments, recurring mandates, merchant verification | Consortium (Verifiable Intent open-source) |
| **Replit** | Agent 4 (March 2026) | Autonomous coding agent; MCP-native (BigQuery, Slack, Notion, Stripe); parallel task execution; human-in-the-loop | Subscription + credits; $9B valuation (Series D, $400M, March 2026) | GA | App building, e-commerce scaffolding, payment flow automation, API integration | Closed (MCP open) |
| **Circuit & Chisel** | ATXP (Agent Transaction Protocol) | Web-wide protocol for agent commerce lifecycle; nested/delegated micropayments; real-time decision-making; autonomous tool discovery | Transaction fees (likely) | Seed ($19.2M; Stripe participating) | Agent-to-agent payments, micropayments, autonomous procurement | Open |
| **Sapiom** | Agent procurement layer | Auth + payment for autonomous API/compute/SaaS purchasing | Processing fees (likely) | Seed ($15.75M, Feb 2026) | Vibe-coding procurement, agent-to-service payments | — |
| **Skyfire** | KYAPay / Agent settlement | Agent identity (KYA) + multi-rail settlement (cards, ACH, USDC); Visa TAP integration; F5 bot defense | Transaction fees | Post-beta ($9.5M total) | Agent identity, autonomous purchasing, bot verification | Open (KYAPay) |

---

## 6. Economic Model

### 6.1 Fee Structures

Agentic commerce introduces new fee layers atop existing payment processing:

- **Platform transaction fees:** OpenAI's 4% merchant fee on Instant Checkout is the most visible example. This sits on top of standard Stripe processing fees (~2.9% + $0.30), meaning merchants pay ~7% total per agent-initiated transaction — significantly higher than traditional e-commerce.
- **Protocol fees:** MPP and x402 charge zero protocol-level fees, competing on adoption. Revenue comes from adjacent services (Stripe's processing fees, Coinbase's exchange/custody fees).
- **Network interchange:** Visa and Mastercard collect standard interchange on card-based agent transactions, with potential for premium "agent transaction" interchange categories.

### 6.2 Value Capture and Revenue Generation

Agents generate revenue through several mechanisms:

1. **Affiliate and referral fees:** Agents that recommend and complete purchases can earn merchant referral commissions, similar to traditional affiliate marketing but at vastly higher conversion rates.
2. **Subscription access:** Premium agent capabilities (higher spending limits, more autonomous operation, priority merchant access) are gated behind subscription tiers (e.g., ChatGPT Plus/Pro).
3. **Data and insight monetization:** Aggregated purchasing patterns, demand signals, and price sensitivity data are valuable to merchants and brands.
4. **Compute and API billing:** Agent-to-service transactions (paying for API calls, compute time, data feeds) represent a growing B2B commerce category, particularly via MPP and x402.

### 6.3 Merchant Economics

For merchants, the equation is nuanced. Agent-initiated transactions carry higher fees but also higher conversion rates and lower customer acquisition costs (no ad spend required if the agent routes the customer). Microsoft reports Copilot-assisted shopping journeys convert at 194% the rate of non-assisted journeys [[8]](#ref8). The net ROI depends on margin structure: high-margin goods (fashion, beauty, digital products) benefit more than low-margin categories (groceries, electronics).

---

## 7. Market Size & Projections

| Source | Projection | Timeframe |
|---|---|---|
| McKinsey | $3–5 trillion globally; ~$1T in U.S. | By 2030 |
| Morgan Stanley AlphaWise | $190–385 billion in U.S. e-commerce (10–20% market share) | By 2030 |
| Bain & Co. | $300–500 billion in U.S. (15–25% of e-commerce) | By 2030 |
| Adobe | 520% growth in AI-assisted online shopping | 2025 holiday season YoY |
| Forrester | Micropayments market unlocked by MPP could reach $50B+ | By 2028 |

Morgan Stanley's AlphaWise survey indicates LLM adoption is approaching 50% in the U.S., with AI agents capturing 10–20% of e-commerce spend [[12]](#ref12). Bain & Co. projects a wider range ($300–500B) based on higher penetration assumptions (15–25% of overall U.S. e-commerce by 2030) [[58]](#ref58). JPMorgan's payments division has identified agentic commerce as a top-three strategic priority, noting that "agent-initiated transactions introduce a new category altogether" — payments executed on behalf of consumers, sometimes with human involvement but increasingly fully autonomous [[13]](#ref13).

A March 2026 PYMNTS Intelligence report (commissioned by Visa Acceptance Solutions) surveying 75 acquirers across the U.S., Brazil, and the UAE found that nearly **80% of acquirers** say they are at least somewhat prepared for agentic commerce — but perceive significant merchant readiness gaps due to integration costs, legacy systems, and unresolved questions around fraud, identity, and liability [[59]](#ref59).

The x402 protocol alone has demonstrated $600M in annualized volume across 154M+ transactions, almost entirely in the machine-to-machine segment. Stripe's MPP integration across 100+ services at launch suggests rapid scaling of the B2B agent payments vertical.

---

## 8. Trust, Security & Fraud

### 8.1 The Agent Fraud Surface

Agentic commerce introduces novel attack vectors that traditional fraud systems are not designed to handle:

- **Fraudulent agent storefronts:** Visa has identified networks of fraudulent websites deploying conversational AI agents that give sites the appearance of legitimacy. These sophisticated storefronts pass automated security checks, offer below-market prices, and when an agent completes a purchase, the malicious merchant harvests payment credentials [[5]](#ref5).
- **Credential delegation risks:** When a consumer authorizes an agent to use stored payment credentials, the blast radius of a compromised agent is potentially very large — the agent may have access to multiple payment methods, shipping addresses, and purchase history.
- **Agent impersonation:** Without robust identity verification (Visa's Trusted Agent Protocol, Mastercard's Agentic Tokens), a malicious actor can deploy agents that impersonate legitimate services to intercept transactions.

### 8.2 Mitigation Approaches

- **Cryptographic agent attestation:** Visa's Trusted Agent Protocol and Mastercard's framework both provide mechanisms for merchants to verify that an agent is authorized and has not been tampered with.
- **Spending limits and guardrails:** Agent wallets with on-chain spending caps (e.g., "this agent may spend at most $200/day") provide a hard ceiling on damage from compromise.
- **Human-in-the-loop checkpoints:** Most current implementations (ChatGPT Instant Checkout, Amazon Buy for Me, Copilot Checkout) still require human confirmation at the payment step. Fully autonomous purchasing remains limited to lower-value B2B transactions.
- **Behavioral anomaly detection:** Payment networks are training fraud models on agent transaction patterns, which differ significantly from human patterns (consistent timing, no mouse jitter, deterministic navigation paths). However, Visa has reported a **25% increase in malicious bot-initiated transactions** globally (40% in the U.S.), and a **450% surge** in dark web posts mentioning "AI Agent" over six months — indicating that adversaries are actively developing tools to exploit the behavioral detection gap [[57]](#ref57).

### 8.4 Small Business Fraud Exposure

Agentic commerce disproportionately shifts fraud exposure onto small and mid-size businesses (SMBs). AI agents do not recognize suspicious pricing, cloned websites, or brand impersonation unless explicitly programmed to assess legitimacy. Smaller businesses lack the monitoring tools and security resources that larger enterprises deploy to detect and shut down impersonation. If AI-driven transactions increase counterfeit disputes or unauthorized purchase claims, SMBs absorb the financial impact first. On average, 40% of attacks are now AI-assisted, with businesses reporting average annual losses of $4.5 million from AI-enabled fraud [[60]](#ref60).

### 8.3 Merchant-Side Concerns

Merchants face a dilemma: agents optimize ruthlessly for price, availability, and fulfillment speed. A stockout or missed delivery window means the agent routes future orders to a competitor. This creates pressure toward perfect inventory accuracy, real-time pricing APIs, and guaranteed SLAs — raising operational costs. The Payment Expert notes that payment service providers (PSPs) will be "central to building trust" in agentic commerce, functioning as both financial intermediaries and verification layers [[14]](#ref14).

---

## 9. Regulatory & Legal Landscape

### 9.1 The Liability Gap

As of March 2026, no jurisdiction has enacted regulation specifically addressing liability for autonomous AI purchases. This represents the most significant unresolved legal question in the space. When an agent makes a purchase the consumer did not intend, or purchases a defective product, the liability chain is ambiguous: is the consumer responsible (they delegated authority), the AI provider (they built the agent), or the merchant (they accepted the order)?

Clifford Chance identifies this as a "liability gap your contracts may not cover" — existing commercial agreements are drafted for human-to-business transactions and do not contemplate AI intermediaries [[15]](#ref15).

### 9.2 EU AI Act

The EU AI Act, with general-purpose AI provisions taking effect in August 2025 and high-risk system requirements from August 2026, classifies many agentic commerce systems as high-risk when they influence financial decisions or handle sensitive personal data. This triggers obligations around:

- Technical documentation and risk assessments
- Human oversight mechanisms
- Transparency (consumers must know they are interacting with an AI)
- Data governance and quality requirements

However, the Act does not specify whether human oversight must be real-time at the moment of purchase, creating ambiguity for fully autonomous agents [[16]](#ref16).

### 9.3 GDPR Implications

The GDPR applies to agentic commerce in several dimensions: agents process personal data (names, addresses, payment information, purchase history) and may transfer it across borders. The Spanish Supervisory Authority issued guidance in early 2026 stating that "greater technical autonomy does not reduce legal responsibility" — organizations deploying purchasing agents must demonstrate full GDPR compliance including lawful basis, data minimization, and the right to explanation for automated decisions [[17]](#ref17).

### 9.4 PSD3 and Payment Regulation

PSD3 (Payment Services Directive 3), expected to be finalized in 2026, aims to formalize "delegated payment initiation" — the legal framework for an agent to initiate a payment on behalf of a consumer. This would establish clear liability allocation and consumer protection standards for agent-initiated transactions. Details are still being negotiated [[16]](#ref16).

### 9.5 U.S. Regulatory Landscape

The U.S. lacks comprehensive federal legislation addressing AI-initiated transactions. The FTC has jurisdiction over unfair or deceptive practices and could act against misleading agent behavior. The CFPB's mandate covers consumer financial products and could extend to agent payment services. The Center for Data Innovation notes that "regulation meant for humans will slow [agentic commerce] down" — existing consumer protection laws assume a human decision-maker, and adapting them to agent contexts will require legislative or regulatory action [[18]](#ref18).

---

## 10. Developer Ecosystem

### 10.1 SDKs and APIs

- **Stripe MPP SDK:** Extends the existing Stripe SDK with MPP-specific endpoints. Merchants accept agent payments via `PaymentIntents` with an `agent_context` parameter.
- **OpenAI Checkout SDK:** Merchants integrate Instant Checkout via the ACP spec, handling product catalog exposure, order creation, and fulfillment webhooks.
- **Google UCP SDK:** Provides product feed ingestion, checkout flow integration, and post-purchase event handling. Compatible with existing Google Merchant Center feeds.
- **x402 Client Libraries:** Available for JavaScript, Python, Go, and Rust. Minimal integration: add middleware that responds to HTTP 402 with a USDC payment.

### 10.2 Model Context Protocol (MCP)

MCP has become the universal connector for AI agents. With 97 million installs as of March 2026, every major AI provider (Claude, GPT-5, Gemini) ships MCP-compatible tooling. Commerce-specific MCP servers expose:

- Product search and catalog browsing
- Cart management and checkout
- Order tracking and returns
- Payment method management

The MCP specification was donated to the Linux Foundation's Agentic AI Foundation (AAIF) in December 2025, co-founded by Anthropic, Block, and OpenAI [[7]](#ref7).

### 10.3 Agent Wallets

Agent wallets are an emerging primitive that gives agents independent payment capability:

- **Tempo Agent Wallets:** Prepaid stablecoin wallets with programmable spending rules (daily caps, merchant whitelists, category restrictions).
- **Coinbase Agent Wallets:** On-chain wallets on Base with x402 integration for pay-per-request API access.
- **Stripe Agent Cards:** Virtual card numbers issued to specific agents with merchant-category-code (MCC) restrictions and real-time spend alerts.

### 10.4 OAuth Patterns for Agent Authorization

The emerging standard for consumer-to-agent authorization follows an OAuth 2.0 flow:

1. Consumer authenticates with the AI platform (e.g., ChatGPT, Copilot).
2. Consumer grants the agent a scoped authorization token: "can spend up to $X on category Y at merchants Z."
3. The agent presents this token (or a derivative credential like a Visa Trusted Agent attestation) to the merchant at checkout.
4. The merchant validates the token against the issuing platform and processes the payment.

This pattern preserves consumer control while enabling agent autonomy within defined boundaries.

---

## 11. Open Research Questions

### 11.1 Autonomous Spending Limits and Consumer Protection

How should spending limits be set and enforced for fully autonomous agents? Current systems require human confirmation for most purchases, but the trajectory is toward greater autonomy. The relationship between spending authority, consumer liability, and agent error tolerance is not well understood.

### 11.2 Multi-Agent Commerce

What happens when a consumer's purchasing agent negotiates with a merchant's selling agent? Agent-to-agent negotiation introduces game-theoretic dynamics (pricing, bundling, loyalty) that may not converge to consumer-optimal outcomes. The Agent-to-Agent (A2A) protocol provides the communication layer, but the strategic implications are largely unexplored.

### 11.3 Agent Collusion and Market Manipulation

Could agents trained on similar data or using similar models converge on pricing or purchasing strategies that constitute tacit collusion? Antitrust regulators have not yet addressed this scenario, but it is a plausible concern as agents intermediate a growing share of transactions.

### 11.4 Identity and Attribution

When an agent operates across multiple platforms and merchants, how is its identity tracked and reputation established? Current approaches are platform-specific (Visa's protocol works within the Visa network, OpenAI's within ChatGPT). A cross-platform agent identity standard does not yet exist.

### 11.5 Sustainability and Resource Allocation

Agent commerce optimizes for consumer utility (low price, fast delivery) but may not account for externalities (carbon footprint, labor conditions, local business impact). Whether and how to encode sustainability preferences into agent objectives is an open design question.

### 11.6 Accessibility and Equity

Will agentic commerce exacerbate or reduce the digital divide? Agents could lower barriers for consumers with disabilities or limited digital literacy, but they also require access to AI platforms that may be subscription-gated. The equity implications depend on deployment models and pricing.

### 11.7 Protocol Convergence or Fragmentation

Five major protocols (MPP, ACP, UCP, x402, ATXP) are live simultaneously. Will the market converge on one standard, or will a multi-protocol world persist? UCP's explicit compatibility with A2A, ACP, and MCP suggests a layered rather than winner-take-all outcome, while ATXP's entry signals continued startup innovation in the payment layer. Convergence is far from certain.

---

## 12. References

<a id="ref1"></a>
**[1]** Stripe. "Introducing the Machine Payments Protocol." March 18, 2026. https://stripe.com/blog/machine-payments-protocol — See also: Stripe Documentation, "MPP - Machine payments." https://docs.stripe.com/payments/machine/mpp

<a id="ref2"></a>
**[2]** OpenAI. "Buy it in ChatGPT: Instant Checkout and the Agentic Commerce Protocol." September 2025. https://openai.com/index/buy-it-in-chatgpt/

<a id="ref3"></a>
**[3]** Google. "Read Sundar Pichai's remarks at the 2026 National Retail Federation." January 11, 2026. https://blog.google/company-news/inside-google/message-ceo/nrf-2026-remarks/ — See also: TechCrunch. "Google announces a new protocol to facilitate commerce using AI agents." https://techcrunch.com/2026/01/11/google-announces-a-new-protocol-to-facilitate-commerce-using-ai-agents/

<a id="ref4"></a>
**[4]** Coinbase. "Coinbase and Cloudflare Will Launch the x402 Foundation." September 2025. https://www.coinbase.com/blog/coinbase-and-cloudflare-will-launch-x402-foundation — See also: x402.org. https://www.x402.org/

<a id="ref5"></a>
**[5]** Visa. "Visa Introduces Trusted Agent Protocol: An Ecosystem-Led Framework for AI Commerce." 2025. https://investor.visa.com/news/news-details/2025/Visa-Introduces-Trusted-Agent-Protocol-An-Ecosystem-Led-Framework-for-AI-Commerce/default.aspx

<a id="ref6"></a>
**[6]** Mastercard. "Agentic token framework: Driving trusted AI transactions." 2025. https://www.mastercard.com/global/en/news-and-trends/stories/2025/agentic-commerce-framework.html

<a id="ref7"></a>
**[7]** Anthropic. "Model Context Protocol." — See also: TechCrunch. "OpenAI, Anthropic, and Block join new Linux Foundation effort to standardize the AI agent era." December 9, 2025. https://techcrunch.com/2025/12/09/openai-anthropic-and-block-join-new-linux-foundation-effort-to-standardize-the-ai-agent-era/

<a id="ref8"></a>
**[8]** Microsoft. "Conversations that Convert: Copilot Checkout and Brand Agents." January 2026. https://about.ads.microsoft.com/en/blog/post/january-2026/conversations-that-convert-copilot-checkout-and-brand-agents — See also: PayPal. "PayPal Powers Microsoft's Launch of Copilot Checkout." January 8, 2026. https://newsroom.paypal-corp.com/2026-01-08-PayPal-Powers-Microsofts-Launch-of-Copilot-Checkout

<a id="ref9"></a>
**[9]** Amazon. "Amazon's new 'Buy for Me' feature helps customers find..." https://www.aboutamazon.com/news/retail/amazon-shopping-app-buy-for-me-brands — See also: Digital Commerce 360. "Amazon opens up new AI-enabled Buy for Me, Shop Direct options for merchants." March 11, 2026. https://www.digitalcommerce360.com/2026/03/11/amazon-opens-up-new-ai-enabled-buy-for-me-shop-direct-options-for-merchants/

<a id="ref10"></a>
**[10]** Anthropic. "MCP Protocol Crosses 97 Million Installs." — See also: The New Stack. "Agent Skills: Anthropic's Next Bid to Define AI Standards." https://thenewstack.io/agent-skills-anthropics-next-bid-to-define-ai-standards/

<a id="ref11"></a>
**[11]** Replit. "2025: Replit in Review." https://blog.replit.com/2025-replit-in-review — See also: SaaStr. "By Late 2025, Replit Got Really Good." https://www.saastr.com/by-late-2025-replit-got-really-good-imagine-if-it-could-run-24x7/

<a id="ref12"></a>
**[12]** PYMNTS. "AI Agents Start Shopping and Payments Firms Adapt." 2026. https://www.pymnts.com/news/artificial-intelligence/2026/ai-agents-start-shopping-and-payments-firms-adapt/

<a id="ref13"></a>
**[13]** JPMorgan. "Agentic Commerce: The Future of AI-Powered Shopping." https://www.jpmorgan.com/payments/newsroom/agentic-commerce-ai-future-shopping

<a id="ref14"></a>
**[14]** PaymentExpert. "Building trust in the age of agentic commerce: Why PSPs will be central to its success." March 30, 2026. https://paymentexpert.com/2026/03/30/psps-agentic-commerce-trust-mechanism/

<a id="ref15"></a>
**[15]** Clifford Chance. "Agentic AI: The liability gap your contracts may not cover." February 2026. https://www.cliffordchance.com/insights/resources/blogs/talking-tech/en/articles/2026/02/agentic-ai-and-the-liability-gap-your-contracts-may-not-cover.html

<a id="ref16"></a>
**[16]** European Business Magazine. "Agentic Commerce: When AI Buys on Your Behalf." https://europeanbusinessmagazine.com/business/agentic-commerce-when-ai-buys-on-your-behalf-who-pays-whos-liable/ — See also: Edgar, Dunn & Company. "EU AI Act: What It Means for Agentic Commerce." https://www.edgardunn.com/articles/the-new-rules-for-ai-inside-the-eus-bold-legislation

<a id="ref17"></a>
**[17]** Inside Privacy. "Spanish Supervisory Authority Issues Detailed Guidance on Agentic AI and GDPR Compliance." 2026. https://www.insideprivacy.com/artificial-intelligence/spanish-supervisory-authority-issues-detailed-guidance-on-agentic-ai-and-gdpr-compliance/

<a id="ref18"></a>
**[18]** Center for Data Innovation. "Agentic Commerce is Coming, but Regulation Meant for Humans Will Slow It Down." March 2026. https://datainnovation.org/2026/03/agentic-commerce-is-coming-but-regulation-meant-for-humans-will-slow-it-down/

<a id="ref19"></a>
**[19]** Stripe. "Stripe Named a Leader by Forrester in Merchant Payment Providers, Q1 2026." 2026. https://stripe.com/newsroom/news/stripe-named-a-leader-by-forrester-2026 — See also: Yahoo Finance. "Stripe Reaches $159 Billion Valuation as Payment Volume Jumps 34%." https://finance.yahoo.com/news/stripe-reaches-159-billion-valuation-195143711.html

<a id="ref20"></a>
**[20]** TechCrunch. "Stripe wants to turn your AI costs into a profit center." March 2, 2026. https://techcrunch.com/2026/03/02/stripe-wants-to-turn-your-ai-costs-into-a-profit-center/ — See also: PYMNTS. "Stripe Introduces Billing Tools to Meter and Charge AI Usage." https://www.pymnts.com/news/artificial-intelligence/2026/stripe-introduces-billing-tools-to-meter-and-charge-ai-usage/

<a id="ref21"></a>
**[21]** Stripe. "Testing the impact of Adaptive Pricing across 1.5M subscription checkout sessions." 2026. https://stripe.com/blog/adaptive-pricing-for-subscriptions

<a id="ref22"></a>
**[22]** Fortune. "OpenAI forecasts its revenue will top $280 billion in 2030." February 20, 2026. https://fortune.com/2026/02/20/openai-revenue-forecast-280-billion-2030-capex-sam-altman/ — See also: CNBC. "OpenAI resets spending expectations, targets ~$600B by 2030." https://www.cnbc.com/2026/02/20/openai-resets-spend-expectations-targets-around-600-billion-by-2030.html

<a id="ref23"></a>
**[23]** OpenAI. "Introducing GPT-5.4." March 5, 2026. https://openai.com/index/introducing-gpt-5-4/

<a id="ref24"></a>
**[24]** CNBC. "OpenAI's first try at agentic shopping stumbled. It's trying again." March 20, 2026. https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html — See also: Digital Commerce 360. "OpenAI reveals updates to its agentic commerce experience for ChatGPT." March 24, 2026. https://www.digitalcommerce360.com/2026/03/24/openai-agentic-commerce-updates-chatgpt-walmart/

<a id="ref25"></a>
**[25]** Google. "Universal Commerce Protocol updates improve AI shopping for retailers." March 19, 2026. https://blog.google/products-and-platforms/products/shopping/ucp-updates/ — See also: Search Engine Journal. "Google Expands UCP With Cart, Catalog, Onboarding." https://www.searchenginejournal.com/google-expands-ucp-with-cart-catalog-onboarding/570084/

<a id="ref26"></a>
**[26]** Google Developers Blog. "Announcing the Agent2Agent Protocol (A2A)." April 2025. https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/ — See also: Linux Foundation. "Linux Foundation Launches the Agent2Agent Protocol Project." June 2025. https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents

<a id="ref27"></a>
**[27]** Google Cloud. "Nexi Group and Google Cloud Collaborate to Drive Agentic Commerce Across Europe." March 3, 2026. https://www.googlecloudpresscorner.com/2026-03-03-Nexi-Group-and-Google-Cloud-Collaborate-to-Drive-Agentic-Commerce-Across-Europe

<a id="ref28"></a>
**[28]** Microsoft. "Introducing the First Frontier Suite Built on Intelligence & Trust." March 9, 2026. https://blogs.microsoft.com/blog/2026/03/09/introducing-the-first-frontier-suite-built-on-intelligence-trust/ — See also: CNBC. "Microsoft adds higher-priced Office tier." https://www.cnbc.com/2026/03/09/microsoft-office-365-e7-copilot-ai.html

<a id="ref29"></a>
**[29]** Microsoft. "Copilot Cowork: A new way of getting work done." March 9, 2026. https://www.microsoft.com/en-us/microsoft-365/blog/2026/03/09/copilot-cowork-a-new-way-of-getting-work-done/ — See also: Fortune. "Microsoft debuts Copilot Cowork built with Anthropic's help." https://fortune.com/2026/03/09/microsoft-copilot-cowork-ai-agents-anthropic-e7-m365-saas/

<a id="ref30"></a>
**[30]** Microsoft Foundry Blog. "Introducing Microsoft Agent Framework." 2026. https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/ — See also: GitHub. https://github.com/microsoft/agent-framework

<a id="ref31"></a>
**[31]** Microsoft Foundry Blog. "Foundry Agent Service is Generally Available." March 16, 2026. https://devblogs.microsoft.com/foundry/foundry-agent-service-ga/

<a id="ref32"></a>
**[32]** About Amazon. "Amazon introduces feeds to make it easier for merchants." March 11, 2026. https://www.aboutamazon.com/news/retail/amazon-shop-direct-external-stores — See also: TechCrunch. "Amazon expands a program that lets customers shop from other retailers' sites." https://techcrunch.com/2026/03/11/amazon-expands-a-program-that-lets-customers-shop-from-other-retailers-sites/

<a id="ref33"></a>
**[33]** Nova Analytics. "Amazon Rufus Becomes Agentic — Auto-Buy, Price Tracking, and 250 Million Users." 2026. https://www.novadata.io/resources/news/amazon-rufus-agentic-auto-buy-250-million-users

<a id="ref34"></a>
**[34]** GeekWire. "Judge blocks Perplexity's AI bot from shopping on Amazon." March 10, 2026. https://www.geekwire.com/2026/judge-blocks-perplexitys-ai-bot-from-shopping-on-amazon-in-early-test-of-agentic-commerce/ — See also: CNBC. "Amazon wins court order to block Perplexity's AI shopping agent." https://www.cnbc.com/2026/03/10/amazon-wins-court-order-to-block-perplexitys-ai-shopping-agent.html

<a id="ref35"></a>
**[35]** OpenAI. "OpenAI and Amazon announce strategic partnership." February 2026. https://openai.com/index/amazon-partnership/ — See also: Forrester. "Power Couple: OpenAI + Amazon May Have Just Won Consumer Agentic Commerce." https://www.forrester.com/blogs/power-couple-openai-amazon-may-have-just-won-consumer-agentic-commerce/

<a id="ref36"></a>
**[36]** Context Studios. "MCP Ecosystem in 2026: What the v1.27 Release Actually Tells Us." 2026. https://www.contextstudios.ai/blog/mcp-ecosystem-in-2026-what-the-v127-release-actually-tells-us — See also: Cisco Blogs. "What's New in MCP: Elicitation, Structured Content, and OAuth Enhancements." https://blogs.cisco.com/developer/whats-new-in-mcp-elicitation-structured-content-and-oauth-enhancements

<a id="ref37"></a>
**[37]** Anthropic. "Dispatch and Computer Use." March 23, 2026. https://claude.com/blog/dispatch-and-computer-use — See also: 9to5Mac. "Anthropic is giving Claude the ability to use your Mac." https://9to5mac.com/2026/03/23/anthropic-is-giving-claude-the-ability-to-use-your-mac-for-you/

<a id="ref38"></a>
**[38]** McGinniss, Iain. "Zero-copy Protobuf and ConnectRPC for Rust." March 2026. https://medium.com/@iainmcgin/zero-copy-protobuf-and-connectrpc-for-rust-69bda8ac0f02 — See also: GitHub. https://github.com/anthropics/buffa and https://github.com/anthropics/connect-rust

<a id="ref39"></a>
**[39]** Anthropic Engineering. "Building a C compiler with a team of parallel Claudes." February 2026. https://www.anthropic.com/engineering/building-c-compiler — See also: GitHub. https://github.com/anthropics/claudes-c-compiler

<a id="ref40"></a>
**[40]** x402.org. "Introducing x402 V2: Evolving the Standard." December 2025. https://www.x402.org/writing/x402-v2-launch — See also: Cloudflare Blog. "Launching the x402 Foundation." https://blog.cloudflare.com/x402/

<a id="ref41"></a>
**[41]** CoinDesk. "Coinbase-backed AI payments protocol wants to fix micropayments but demand is just not there yet." March 11, 2026. https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet

<a id="ref42"></a>
**[42]** PYMNTS. "Visa Debuts Agentic Program to Help Banks Test AI Payments." March 17, 2026. https://www.pymnts.com/artificial-intelligence-2/2026/visa-launches-agentic-ready-program-to-help-banks-test-ai-payments/ — See also: Visa UK. "Visa Launches 'Agentic Ready' Programme in Europe." https://www.visa.co.uk/about-visa/newsroom/press-releases.3437886.html

<a id="ref43"></a>
**[43]** Mastercard. "How Verifiable Intent builds trust in agentic AI commerce." March 5, 2026. https://www.mastercard.com/us/en/news-and-trends/stories/2026/verifiable-intent.html — See also: PYMNTS. "Mastercard Unveils Open Standard to Verify AI Agent Transactions." https://www.pymnts.com/mastercard/2026/mastercard-unveils-open-standard-to-verify-ai-agent-transactions/

<a id="ref44"></a>
**[44]** FIS. "FIS Launches Industry-First Offering Enabling Banks to Lead in Agentic Commerce." January 2026. https://www.fisglobal.com/about-us/media-room/press-release/2026/fis-launches-industry-first-ai-transaction-platform-to-help-banks-lead — See also: Payments Dive. "FIS teams with Visa, Mastercard on agentic commerce." https://www.paymentsdive.com/news/fis-teams-with-visa-mastercard-on-agentic-commerce/809472/

<a id="ref45"></a>
**[45]** TechCrunch. "Sapiom raises $15M to help AI agents buy their own tech tools." February 5, 2026. https://techcrunch.com/2026/02/05/sapiom-raises-15m-to-help-ai-agents-buy-their-own-tech-tools/

<a id="ref46"></a>
**[46]** Payman AI. https://paymanai.com/ — See also: Privacy.com. "AI Agent Payment Solutions in 2026, Compared." https://www.privacy.com/blog/payment-solutions-ai-agents-2026-compared

<a id="ref47"></a>
**[47]** Proxy. https://www.useproxy.ai/ — See also: Proxy Blog. "The AI Agent Payments Landscape in 2026." https://www.useproxy.ai/blog/ai-agent-payments-landscape-2026

<a id="ref48"></a>
**[48]** Skyfire. https://skyfire.xyz/ — See also: BusinessWire. "Skyfire Demonstrates Secure Agentic Commerce Purchase Using KYAPay and Visa." December 2025. https://www.businesswire.com/news/home/20251218520399/en/ — See also: Yahoo Finance. "F5 and Skyfire Partner to Advance Secure Agentic Commerce." March 18, 2026. https://finance.yahoo.com/news/f5-skyfire-partner-advance-secure-130000878.html

<a id="ref49"></a>
**[49]** Flexprice. https://flexprice.io/ — See also: DEV Community. "We've Raised $500K to Build the Open-Source Billing Stack for AI and Agentic Companies." https://dev.to/flexprice_8116ed925/weve-raised-500k-to-build-the-open-source-billing-stack-for-ai-and-agentic-companies-3fj — See also: GitHub. https://github.com/flexprice/mcp-server

<a id="ref50"></a>
**[50]** Circuit & Chisel. "Circuit & Chisel Secures $19.2 Million and Launches ATXP, a Web-Wide Protocol for Agentic Payments." 2025. https://www.prnewswire.com/news-releases/circuit--chisel-secures-19-2-million-and-launches-atxp-a-web-wide-protocol-for-agentic-payments-302562331.html — See also: Fortune. "Stripe crypto alum raise $19.2 million to power agentic payments through ATXP protocol." https://fortune.com/2025/09/22/stripe-crypto-alum-agentic-ai-payments-circuit-chisel-atxp/

<a id="ref51"></a>
**[51]** Stripe. "Introducing the Agentic Commerce Suite: A complete solution for selling on AI agents." 2026. https://stripe.com/blog/agentic-commerce-suite — See also: BigCommerce. "BigCommerce Partners with Stripe to Support New Agentic Commerce Suite." https://investors.bigcommerce.com/news-releases/news-release-details/bigcommerce-partners-stripe-support-new-agentic-commerce-suite

<a id="ref52"></a>
**[52]** Stripe. "Stripe brings a new checkout experience for Facebook." March 24, 2026. https://stripe.com/newsroom/news/checkout-for-facebook — See also: PaymentExpert. "Stripe backs Facebook with agentic payments." March 27, 2026. https://paymentexpert.com/2026/03/27/stripe-facebook-agentic-payments/

<a id="ref53"></a>
**[53]** Stripe. "Supporting additional payment methods for agentic commerce." March 2026. https://stripe.com/blog/supporting-additional-payment-methods-for-agentic-commerce — See also: PYMNTS. "Visa Teams With Stripe on Agent Payments." https://www.pymnts.com/visa/2026/visa-scales-agentic-commerce-through-stripe-protocol-collaboration/

<a id="ref54"></a>
**[54]** PayPal. "From Search to Checkout: PayPal Supports Trusted AI Checkout with Google." January 2026. https://investor.pypl.com/news-and-events/news-details/2026/From-Search-to-Checkout-PayPal-Supports-Trusted-AI-Checkout-with-Google/default.aspx — See also: PayPal. "Agentic Commerce with PayPal." https://www.paypal.com/us/business/ai

<a id="ref55"></a>
**[55]** Sacra. "Anthropic revenue, valuation & funding." 2026. https://sacra.com/c/anthropic/ — See also: TechCrunch. "Anthropic's Claude popularity with paying consumers is skyrocketing." March 28, 2026. https://techcrunch.com/2026/03/28/anthropics-claude-popularity-with-paying-consumers-is-skyrocketing/

<a id="ref56"></a>
**[56]** Mastercard. "Santander and Mastercard complete Europe's first live end-to-end payment executed by an AI agent." March 2, 2026. https://www.mastercard.com/news/europe/en/newsroom/press-releases/en/2026/santander-and-mastercard-complete-europe-s-first-live-end-to-end-payment-executed-by-an-ai-agent/ — See also: PYMNTS. "Mastercard and Santander Complete EU's First Agentic Payment." https://www.pymnts.com/news/artificial-intelligence/2026/mastercard-santander-mark-agentic-payment-milestone/

<a id="ref57"></a>
**[57]** Visa. "Agentic Commerce: Threats and Risks." March 2026. https://corporate.visa.com/en/sites/visa-perspectives/security-trust/the-threats-landscape-of-agentic-commerce.html

<a id="ref58"></a>
**[58]** Bain & Co. "2030 Forecast: How Agentic AI Will Reshape US Retail." 2025. https://www.bain.com/insights/2030-forecast-how-agentic-ai-will-reshape-us-retail-snap-chart/ — See also: Digital Commerce 360. "Bain: Agentic AI could account for 25% of U.S. ecommerce sales by 2030." https://www.digitalcommerce360.com/2025/12/22/bain-agentic-ai-us-ecommerce-sales-2030/

<a id="ref59"></a>
**[59]** PYMNTS. "How Acquirers Prepare for Agentic Commerce." March 2026. https://www.pymnts.com/study/how-acquirers-prepare-for-agentic-commerce — See also: PYMNTS. "Acquirers Say Risk and Readiness Are Slowing Agentic Commerce." https://www.pymnts.com/news/artificial-intelligence/2026/80percent-of-acquirers-say-they-are-ready-for-agentic-commerce-but-merchants-lag/

<a id="ref60"></a>
**[60]** FinTech Weekly. "Agentic Commerce Is Optimized for Efficiency. Small Businesses Will Absorb the Fraud Risk." 2026. https://www.fintechweekly.com/magazine/articles/agentic-commerce-fraud-risk-small-businesses-payments-2026

<a id="ref24b"></a>
**[24b]** Shopify. "Millions of merchants can sell in AI chats." March 24, 2026. https://www.shopify.com/news/agentic-commerce-momentum — See also: Modern Retail. "Shopify says purchases are coming 'inside ChatGPT' through agentic storefronts." https://www.modernretail.co/technology/shopify-says-purchases-are-coming-inside-chatgpt-through-agentic-storefronts-as-openai-retreats-on-instant-checkout/

---

## Changelog

| Date | Change |
|---|---|
| 2026-03-31 | **v3 — End-of-day update.** Abstract updated to reflect OpenAI's Instant Checkout pivot and Shopify Agentic Storefronts (5.6M merchants). Added Bain ($300–500B) and Morgan Stanley ($190–385B) U.S. market projections. §3.1: Added ATXP (Circuit & Chisel, $19.2M seed) as fifth major protocol. §4.1: Added Stripe Agentic Commerce Suite (BigCommerce launch partner), Meta/Facebook one-click checkout via ACP, expanded SPT support (Mastercard Agent Pay, Visa Intelligent Commerce, Affirm, Klarna). §4.2: Detailed Instant Checkout pivot rationale; added Shopify Agentic Storefronts (opt-out across 5.6M stores); added Walmart in-ChatGPT app with account linking. §4.3: Added PayPal UCP integration and Agent Ready. §4.6: Added Anthropic $19B ARR, Claude Code $2.5B ARR, consumer subscription doubling, Claude Marketplace launch. §4.8: Added Santander–Mastercard Europe's first live regulated agentic payment (March 2, 2026); added Visa PERC threat data (450% dark web surge, 25% bot transaction increase). §4.10: Added Circuit & Chisel / ATXP. §5: Updated comparison table — revised Stripe, OpenAI, Google, Anthropic, Mastercard rows; added Circuit & Chisel row. §7: Added Bain projection, PYMNTS/Visa acquirer readiness survey (80% ready but merchant gap). §8: Added §8.4 (SMB fraud exposure); integrated Visa behavioral detection gap data. §11.7: Updated protocol count to five. Added 12 new references ([24b], [50]–[60]). |
| 2026-03-31 | **v2 — Comprehensive refresh of §4.1–4.10.** §4.1: Stripe $1.9T volume, Forrester Leader, Tempo $5B valuation, AI metering/billing tools, Adaptive Pricing (1.5M sessions). §4.2: OpenAI $280B 2030 projection, GPT-5.4 OSWorld-Verified 75.0%, ACP open standard details, Instant Checkout pivot to retailer apps. §4.3: Google UCP March expansion (Cart, Catalog, Identity Linking), A2A protocol (v0.3, Linux Foundation, 150+ orgs), Nexi EU partnership. §4.4: Microsoft E7 ($99/user), Copilot Cowork (with Anthropic), Agent Framework open-source, Foundry Agent Service GA with private networking. §4.5: Amazon Shop Direct formalization (400K+ merchants, 100M+ products), Rufus agentic auto-buy (250M+ users), Amazon v. Perplexity injunction, $50B OpenAI partnership. §4.6: MCP spec evolution (OAuth 2.1, elicitation, structured output), Claude Computer Use for Mac, buffa/connect-rust open-source (33% faster decode), Claude Opus 4.6 C compiler (100K-line Rust, boots Linux). §4.7: x402 Foundation expansion (Google, Visa), MCP streaming integration, wash trading reality check. §4.8: Visa Agentic Ready (21 EU banks), Mastercard Verifiable Intent (with Google, open-source), FIS dual-network partnership. §4.10: Added Sapiom ($15.75M), Flexprice ($500K); expanded Payman AI, Proxy, Skyfire entries. Updated comparison table with all changes. |
| 2026-03-31 | Updated §4.9 (Replit) with Agent 4 launch details, $400M Series D at $9B valuation, MCP-native integrations, parallel task execution, 40M users, 85% Fortune 500 penetration. Updated comparison table row. |
| 2026-03-31 | Initial publication (v1). Comprehensive survey of protocols (MPP, ACP, UCP, x402), key players, market projections, trust frameworks, regulatory landscape, and developer ecosystem. |
