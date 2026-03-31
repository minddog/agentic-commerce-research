# Agentic Commerce: The Emerging Infrastructure for AI-Initiated Transactions

**Last Updated:** March 31, 2026

**Authors:** Research compiled via Claude (Anthropic)

**Status:** Living document — continuously updated

---

## Abstract

Agentic commerce — the paradigm in which autonomous AI agents discover, negotiate, and complete purchases on behalf of human principals — has moved from speculative concept to deployed reality in under eighteen months. As of March 2026, ChatGPT's Instant Checkout serves 900 million weekly users, Google's Universal Commerce Protocol (UCP) has the backing of Shopify, Walmart, and Target, and Stripe's Machine Payments Protocol (MPP) is live across more than 100 services. This paper surveys the technical infrastructure, key players, economic models, trust frameworks, regulatory landscape, and open research questions defining this nascent but rapidly consolidating field. McKinsey projects agentic commerce will orchestrate $3–5 trillion in global retail spend by 2030, with nearly $1 trillion originating in the United States alone. The stakes — and the unresolved questions — are enormous.

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
3. **Consumer surface area.** ChatGPT (900M weekly users), Google Gemini, Microsoft Copilot, and Amazon's app each now offer embedded checkout flows where an agent completes a purchase without the user ever visiting a merchant website.

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

Four protocols dominate the landscape as of Q1 2026:

**Machine Payments Protocol (MPP)** — Co-authored by Stripe and Tempo, MPP launched on March 18, 2026 as an open specification for agent-to-service payment coordination. It supports fiat payments (cards, BNPL) via Shared Payment Tokens (SPTs) as well as stablecoin settlement. Visa has partnered to enable card-based MPP transactions on its global network. Stripe users can accept MPP payments in a few lines of code via the existing PaymentIntents API. Over 100 services — spanning model providers, developer tools, compute platforms, and data vendors — have integrated MPP as of launch [[1]](#ref1).

**Agentic Commerce Protocol (ACP)** — Developed by OpenAI in collaboration with Stripe, ACP has been live since September 2025 powering ChatGPT's Instant Checkout. OpenAI charges merchants a 4% transaction fee on completed purchases (shoppers pay nothing extra), on top of standard payment processing fees. Microsoft's Copilot Checkout has adopted ACP as its interoperability standard [[2]](#ref2).

**Universal Commerce Protocol (UCP)** — Announced by Google CEO Sundar Pichai at the National Retail Federation (NRF) conference on January 11, 2026. UCP is an open standard covering the entire shopping journey — discovery, purchase, and post-purchase support. It is backed by Shopify, Etsy, Wayfair, Target, and Walmart, with 20+ additional endorsements. UCP is designed to be compatible with A2A (Agent-to-Agent), ACP, and MCP [[3]](#ref3).

**x402** — Created by Coinbase and launched in May 2025, x402 repurposes the HTTP 402 "Payment Required" status code to embed stablecoin (USDC) payments directly into HTTP request/response cycles. Governed by the x402 Foundation (Coinbase + Cloudflare), it has processed over 119 million transactions on Base and 35 million on Solana, with ~$600M in annualized volume and zero protocol fees. Stripe began routing USDC agent payments via x402 on Base in February 2026 [[4]](#ref4).

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

Stripe's MPP, co-authored with the crypto startup Tempo (backed by Paradigm), is the most infrastructure-level play in the space. Rather than building a consumer-facing agent, Stripe is providing the payment rails that *all* agents can use. MPP supports both fiat and stablecoin settlement, is integrated into Stripe's existing API surface, and has Visa's backing for card-based transactions. Tempo raised funding from Paradigm and launched alongside MPP on March 18, 2026. Their business model is the standard Stripe processing fee (~2.9% + $0.30 for cards) plus potential stablecoin settlement fees [[1]](#ref1).

### 4.2 OpenAI — Instant Checkout / ACP

OpenAI's approach is vertically integrated: ChatGPT is both the agent interface and the commerce platform. Instant Checkout launched in September 2025 for U.S. users and initially supported Etsy sellers, with Shopify merchants (Glossier, SKIMS, Spanx, Vuori) onboarding soon after. OpenAI's business model charges merchants 4% per completed transaction — a significant margin on top of standard payment processing. With 900M weekly active users, ChatGPT represents arguably the largest single distribution channel for agentic commerce [[2]](#ref2). OpenAI also operates Operator, launched January 2025 as a ChatGPT Pro exclusive, which can autonomously navigate websites, click buttons, fill forms, and complete purchases — partnering with eBay, Instacart, and Etsy.

### 4.3 Google — Universal Commerce Protocol

Google's UCP play leverages its dominance in search and product discovery. UCP enables a "buy button" directly on Google Search, AI Mode, and Gemini, eliminating the need for users to visit merchant websites. The protocol is open and backed by major retailers. Google's monetization model likely extends its existing advertising and shopping commission structures — merchants already pay for Google Shopping placements, and UCP adds a transactional layer. The NRF announcement positioned UCP as compatible with A2A, ACP, and MCP, signaling a standards-diplomacy approach rather than a walled garden [[3]](#ref3).

### 4.4 Microsoft — Copilot Checkout / Brand Agents

Microsoft launched Copilot Checkout on January 8, 2026 with PayPal, Shopify, and Stripe as payment partners. Two key design decisions differentiate Microsoft's approach: (1) merchants remain the merchant of record — they own the transaction, customer data, and relationship; and (2) Microsoft is shipping "Brand Agents" that let Shopify merchants deploy AI assistants on their own sites. Performance data is compelling: shopping journeys involving Copilot are 33% shorter than traditional search and see a 53% increase in purchases within 30 minutes. When shopping intent is present, Copilot journeys are 194% more likely to convert [[8]](#ref8).

### 4.5 Amazon — Buy for Me

Amazon's "Buy for Me" feature, piloted in April 2025 and expanded in March 2026, lets consumers purchase from third-party brand websites without leaving the Amazon app. Powered by Amazon Nova and Anthropic's Claude running on Amazon Bedrock, the agent completes transactions using encrypted customer credentials. Controversially, Amazon initially surfaced third-party products without merchant permission, generating significant retailer backlash. The "Shop Direct" program has since formalized merchant opt-in [[9]](#ref9).

### 4.6 Anthropic — MCP / Computer Use / Claude

Anthropic's contribution is primarily infrastructural rather than consumer-facing. The Model Context Protocol (MCP), announced November 2024 and donated to the Linux Foundation's Agentic AI Foundation (AAIF) in December 2025, has crossed 97 million installs and is now the de facto standard for connecting AI agents to external tools and services — including commerce APIs. Claude's Computer Use capability, introduced with Claude 3.5 Sonnet, allows agents to interact with arbitrary web interfaces as a human would (screen reading, cursor movement, clicking). Success rates reached the high 80s for standard tasks by late 2025 with Claude 4. Claude powers Amazon's Buy for Me feature and is used by numerous enterprise agent deployments [[7]](#ref7) [[10]](#ref10).

### 4.7 Coinbase / Cloudflare — x402

The x402 protocol represents the crypto-native approach to agent payments. By embedding USDC payments into the HTTP layer, x402 eliminates the need for accounts, credit cards, or manual intervention. The protocol has zero fees and is governed by the x402 Foundation. As of March 2026, it has processed 154M+ transactions across Base and Solana with ~$600M in annualized volume. Stripe integrated x402 for on-chain agent payments in February 2026, validating the protocol's relevance beyond crypto-native use cases [[4]](#ref4).

### 4.8 Visa / Mastercard — Network Infrastructure

Both card networks have recognized that agent-initiated payments represent a new transaction category requiring new infrastructure. Visa's Trusted Agent Protocol provides merchant-side verification of authorized agents, while Mastercard's Agentic Token framework extends mobile-payment tokenization to agent contexts. Both are positioning as trust layers that sit beneath any of the higher-level protocols (MPP, ACP, UCP) [[5]](#ref5) [[6]](#ref6).

### 4.9 Replit — Developer-Facing Agents

Replit's Agent 3 (launched September 2025) represents the developer-tools angle of agentic commerce. While not a payment protocol, Replit agents can autonomously build, deploy, and integrate e-commerce applications — including connecting to Stripe for payment processing. Replit went from $10M to $100M ARR in 9 months after launching Agent, raised $250M at a $3B valuation in January 2026, and supports custom MCP servers for external tool integration [[11]](#ref11).

### 4.10 Emerging Startups

- **Proxy:** Building agent payment orchestration infrastructure.
- **Payman AI:** Agent-specific payment wallets with programmable spending rules.
- **Skyfire:** Agent-to-agent payment settlement network.

---

## 5. Comparison Table

| Company | Product / Protocol | Technical Approach | Business Model | Stage | Key Use Cases | Open / Closed |
|---|---|---|---|---|---|---|
| **Stripe / Tempo** | Machine Payments Protocol (MPP) | Open spec; fiat + stablecoin via SPTs; PaymentIntents API | Processing fees (~2.9% + $0.30) | GA (March 2026) | Agent-to-service payments, micropayments, compute billing | Open |
| **OpenAI** | Instant Checkout / ACP | Integrated in ChatGPT; Operator for web automation | 4% merchant tx fee + processing | GA (Sept 2025) | Consumer shopping, product discovery, booking | Closed (protocol open) |
| **Google** | Universal Commerce Protocol (UCP) | Open standard; buy button in Search/Gemini; A2A compatible | Ads/shopping commissions (likely) | GA (Jan 2026) | Product discovery, cross-merchant checkout, post-purchase | Open |
| **Microsoft** | Copilot Checkout / Brand Agents | PayPal + Shopify + Stripe; merchant stays MoR | Ads revenue; merchant tools | GA (Jan 2026) | Conversational shopping, brand-owned agents | Mixed (ACP-compatible) |
| **Amazon** | Buy for Me / Shop Direct | Nova + Claude on Bedrock; scrapes third-party sites | Marketplace commissions | Beta (expanding) | Cross-merchant shopping within Amazon app | Closed |
| **Anthropic** | MCP / Computer Use / Claude | Open protocol (MCP); vision-based web interaction | API usage fees; enterprise licensing | GA (MCP); Beta (Computer Use) | Tool integration, web automation, enterprise agents | Open (MCP) |
| **Coinbase / Cloudflare** | x402 | HTTP 402 + USDC; on-chain settlement (Base, Solana) | Zero protocol fees; ecosystem play | GA (May 2025) | Micropayments, API monetization, pay-per-request | Open |
| **Visa** | Trusted Agent Protocol | Cryptographic agent verification; card-network settlement | Network interchange fees | Beta / Early GA | Agent identity verification, fraud prevention | Consortium |
| **Mastercard** | Agentic Token Framework | Dynamic tokenization; purchase-intent metadata | Network interchange fees | Beta | Tokenized agent payments, merchant verification | Consortium |
| **Replit** | Agent 3 | Autonomous coding agent; MCP integration; Stripe connect | Subscription + credits | GA (Sept 2025) | App building, e-commerce site creation, integrations | Closed (MCP support) |

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
| Morgan Stanley AlphaWise | $190–385 billion in U.S. e-commerce captured by agents | 2026–2028 |
| Adobe | 520% growth in AI-assisted online shopping | 2025 holiday season YoY |
| Forrester | Micropayments market unlocked by MPP could reach $50B+ | By 2028 |

Morgan Stanley's AlphaWise survey indicates LLM adoption is approaching 50% in the U.S., with AI agents capturing 10–20% of e-commerce spend [[12]](#ref12). JPMorgan's payments division has identified agentic commerce as a top-three strategic priority, noting that "agent-initiated transactions introduce a new category altogether" — payments executed on behalf of consumers, sometimes with human involvement but increasingly fully autonomous [[13]](#ref13).

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
- **Behavioral anomaly detection:** Payment networks are training fraud models on agent transaction patterns, which differ significantly from human patterns (consistent timing, no mouse jitter, deterministic navigation paths).

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

Four major protocols (MPP, ACP, UCP, x402) are live simultaneously. Will the market converge on one standard, or will a multi-protocol world persist? UCP's explicit compatibility with A2A, ACP, and MCP suggests a layered rather than winner-take-all outcome, but this is far from certain.

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

---

## Changelog

| Date | Change |
|---|---|
| 2026-03-31 | Initial publication (v1). Comprehensive survey of protocols (MPP, ACP, UCP, x402), key players, market projections, trust frameworks, regulatory landscape, and developer ecosystem. |
