# MCP Architecture: How Model Context Protocol Works

**Last Updated**: July 2026

The **Model Context Protocol (MCP)** defines a modular, layered architecture for connecting AI agents to external tools and data sources — including cloud cost management APIs, billing exports, and FinOps automation workflows. This page explains how MCP's client-host-server architecture works and why it matters for enterprise cloud cost optimization.

---

## How is MCP architected?

MCP separates AI orchestration, data access, and interaction logic into distinct layers. This design enables flexibility, reusability, and observability — essential principles for scalable and governable AI systems in FinOps.

### 1. MCP Client

The **Client** is the AI-enabled application or agent that consumes context and invokes actions. Think of it as the "brain" that decides what to do. The client discovers available tools and resources exposed by the protocol, dynamically invokes them based on model intent, and composes reusable structured prompts. All queries and contextual data flow through the MCP interface, making the client the primary interaction point for users.

---

### 2. MCP Host

The **Host** acts as the orchestrator and runtime environment for one or more MCP Servers. It provides the operational backbone needed for enterprise-scale deployments: authentication and authorization between clients and services, load balancing and scaling across multiple servers, and centralized governance for AI traffic. For FinOps teams, the host's observability, logging, and usage metering capabilities are critical for tracking costs and attributing usage to specific teams or projects.

---

### 3. MCP Server

Each **Server** implements the protocol and exposes three fundamental resource types:

- **Tools** → Actions the model can invoke, like querying a database, calling an API, or writing files
- **Resources** → Data elements (static or dynamic) used to ground model responses, from documents and metadata to logs and JSON records
- **Prompts** → Reusable, version-controlled templates that standardize AI behavior and ensure consistency across use cases

---

### 4. Interoperability & Orchestration

The true strength of MCP lies in how these components work together. When a client requests a prompt using a known template, the host routes that request to the appropriate server. The server then interpolates relevant resources, invokes the necessary tools, and assembles the final prompt. The result flows back through the host, enabling auditing, logging, and metric collection at every step. This orchestration layer is what makes MCP powerful for enterprise FinOps use cases.  

---

## 🔄 MCP Interaction Cycle

Beyond the core components, MCP defines a standardized **interaction cycle** for communication between Clients, Hosts, and Servers. The client sends requests to the server, which acts as a bridge to external systems. (As of the 2026-07-28 specification these are stateless request/response exchanges rather than a persistent session — see the 2026-07-28 section below.) When the model needs to take action, it invokes tools for structured operations. Contextual data (resources) is supplied to ground the LLM's tasks, and complex operations can be triggered via reusable prompts. The server can even adjust or influence inference through sampling control, while asynchronous updates flow back to the client via notifications. This cycle ensures consistent, auditable interactions across all MCP implementations.  

---

## 🌐 Key Architectural Considerations

**Security & Governance**: Early MCP left authentication to implementers, but the protocol has since standardized on OAuth 2.0 / OIDC — the 2026-07-28 specification adds issuer validation and an Enterprise Managed Authorization (EMA) extension that plugs into providers like Microsoft Entra and Okta (see below). Fine-grained RBAC (Role-Based Access Control) should still be layered on top to avoid overexposing tools and resources to unauthorized users.

**FinOps & Observability**: MCP's modularity enables cost attribution at the level of prompts, tools, and resources. This aligns perfectly with FinOps practices by connecting usage patterns with cost accountability—you can see exactly which teams, projects, or prompts are driving your AI infrastructure costs.

**Extensibility**: MCP enables dynamic discovery of tools and capabilities. Clients can query a server to retrieve available resources and adjust their UI/UX accordingly, making MCP act as a "source of truth" for what's possible. This means adding a new FinOps tool doesn't require client updates—it's automatically discovered and made available.

---

## 🆕 MCP Specification 2026-07-28 Updates

On July 28, 2026, MCP shipped its **fifth specification release** (`2026-07-28`) — the largest architectural change since the protocol launched — and Anthropic began rolling out support across Claude products. Three changes matter most for FinOps deployments.

### 1. Stateless Core

MCP moves from a **bidirectional, stateful** protocol to a **stateless request/response** model. The `initialize`/`initialized` handshake and the `Mcp-Session-Id` header are retired; every request now carries the protocol version and client identity in its `_meta` field, so *any* server instance can answer *any* request. Servers that still need application state pass explicit handles as tool arguments instead of relying on a transport-level session.

**Why this matters for FinOps**: stateless servers run cleanly on **serverless and edge infrastructure** (AWS Lambda, Azure Functions, GCP Cloud Run, Cloudflare Workers) and can **scale to zero** when idle. A cost-query server your team hits a few times a day no longer needs an always-on VM or sticky-session load balancing — the hosting bill for your own FinOps tooling can drop toward pay-per-request. See [Remote MCP Servers](../governance/remote-mcp-servers.md) for the cost breakdown.

### 2. Versioned Extensions Framework

Capabilities that used to be bolted onto the core now ship as **versioned extensions** with namespace identifiers (e.g. `io.modelcontextprotocol/tasks`). Two graduate with this release:

- **MCP Tasks** — the long-running-operation abstraction (introduced experimentally in 2025-11-25) is now the stable `io.modelcontextprotocol/tasks` extension, using poll-based `tasks/get` and a new `tasks/update` method. This is the feature that lets a full-year, multi-cloud billing analysis run asynchronously while you do other work.
- **MCP Apps** — servers can now render **interactive UI directly in the conversation** (a spend-breakdown table, an anomaly chart) instead of returning text only.

### 3. Enterprise-Grade Authorization

Authorization aligns with production OAuth 2.0 / OIDC and identity providers such as Microsoft Entra and Okta, via the **Enterprise Managed Authorization (EMA)** extension. Concrete hardening in this release: **RFC 9207 issuer validation** is required, clients set `application_type` during Dynamic Client Registration (so localhost redirects work correctly), and **client credentials are bound to the issuer that minted them** (no reuse across authorization servers). For FinOps, this is the change that lets a security admin authorize a cost-analysis connector **once** and let the whole team inherit access through existing corporate groups — the usual blocker for putting an agent near billing APIs.

### Migration note for server maintainers

The stateless shift is a **breaking change** for any custom server that depended on session identifiers. The official SDKs ship migration guidance, so budget a review before upgrading a server you operate. Platform teams also gain new **observability dashboards** in Claude covering connector usage, adoption, and errors.

**Sources**: [Bringing MCP 2026-07-28 to Claude (Anthropic)](https://claude.com/blog/bringing-mcp-2026-07-28-to-claude) · [The 2026-07-28 Specification (MCP blog)](https://blog.modelcontextprotocol.io/posts/2026-07-28/)

---

## MCP Specification 2025-11-25 Updates (previous release)

> Superseded by the 2026-07-28 release above for the core protocol and Tasks (Tasks is now the `io.modelcontextprotocol/tasks` extension rather than an experimental core feature). The concepts below still describe how asynchronous workflows and enterprise authentication first arrived in MCP.

On November 25, 2025, MCP released a major specification update to mark the protocol's first anniversary. The update addressed key pain points for enterprise deployments, particularly around asynchronous operations and authentication.

### 1. Task-Based Workflows

MCP now supports a new abstraction for tracking long-running operations. This is a game-changer for FinOps: multi-region pricing comparisons, complex optimization simulations, and large billing export queries can now run asynchronously without blocking the client.

Clients can query task status (`working`, `input_required`, `completed`, `failed`, `cancelled`) and retrieve results when ready. Servers can persist task results for a configurable duration, reducing server load and improving reliability. No need to block on expensive operations—start the job, do other work, and check back when the analysis is complete.

**Example**: Running a full year of multi-cloud billing data analysis across AWS, Azure, and GCP can take minutes. Tasks let you start the job, switch to another task, and retrieve results when they're ready.

### 2. Enhanced Authorization

The 2025 spec introduces several authentication improvements that make MCP more practical for enterprise use:

**OAuth Client ID Metadata Documents (CIMD)** replace complex Dynamic Client Registration with simpler URL-based client metadata. Clients specify their `client_id` as a URL pointing to a JSON document with registration details, reducing friction for enterprise MCP deployments.

**OAuth Client Credentials** enable machine-to-machine authorization without user interaction—critical for automated FinOps workflows like CI/CD cost checks, scheduled reports, and anomaly detection.

**Cross-App Access** is a huge productivity boost: sign in once through an MCP client and gain access to all authorized servers. This eliminates repeated logins across multiple MCP servers, which is invaluable for FinOps teams juggling AWS, Azure, GCP, Vantage, and other data sources.

**Step-Up Authorization** handles insufficient permissions during runtime operations. If a tool requires elevated access (like writing cost allocation tags), the system can request step-up auth on demand rather than failing the operation.

**PKCE is now mandatory** for all OAuth flows. Proof Key for Code Exchange significantly improves security for MCP servers exposed over public networks, protecting against authorization code interception attacks.

### 3. Why This Matters for FinOps

The 2025-11-25 spec addresses two major FinOps pain points: production readiness and enterprise security.

**Production Readiness**: Tasks enable asynchronous cost analyses at scale, cross-app access reduces authentication friction, and client credentials enable automated FinOps pipelines. You can now run MCP-powered cost analysis in CI/CD workflows, scheduled batch jobs, and real-time dashboards without manual intervention.

**Enterprise Security**: Mandatory PKCE protects against authorization code interception, step-up authorization provides just-in-time privilege escalation, and CIMD simplifies client registration for IT teams. Your security team can approve MCP deployments with confidence.

These updates move MCP from "prototype-friendly" to "enterprise-ready" for FinOps use cases. The protocol is now mature enough for production deployments at scale.

---

## 🏛️ Governance milestones (May 2026)

At [MCP Dev Summit North America 2026](https://aaif.io/blog/mcp-is-now-enterprise-infrastructure-everything-that-happened-at-mcp-dev-summit-north-america-2026/), the Agentic AI Foundation announced two governance changes that affect anyone planning long-term MCP deployments:

- **Formal project lifecycle policy** with three stages (Growth, Impact, Emeritus). External project submissions are open for the first time, giving enterprises a clearer path for contributing or sponsoring MCP-adjacent work.
- **Leadership change**: Mazin Gilbert (Google AI veteran, Wharton MBA) replaces Jim Zemlin as AAIF Executive Director.

For FinOps practitioners, the practical signal is that MCP has moved from "open-source experiment" to a foundation-governed protocol with formal project intake and a permanent executive function. This is what enterprise procurement teams want to see before approving production MCP deployments.

---

## 📖 Additional Resources
- [MCP Specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28)
- [The 2026-07-28 Specification (MCP blog)](https://blog.modelcontextprotocol.io/posts/2026-07-28/)
- [Bringing MCP 2026-07-28 to Claude (Anthropic)](https://claude.com/blog/bringing-mcp-2026-07-28-to-claude)
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [One Year of MCP Blog Post](https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/)
- [MCP Authorization Spec Details](https://den.dev/blog/mcp-november-authorization-spec/)  
