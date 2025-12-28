# MCP Architecture

## 🏗️ Core Architectural Components

The **Model Context Protocol (MCP)** defines a modular, layered architecture that separates AI orchestration, data access, and interaction logic. This design enables flexibility, reusability, and observability — essential principles for scalable and governable AI systems.

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

Beyond the core components, MCP defines a standardized **interaction cycle** for communication between Clients, Hosts, and Servers. The client maintains a dedicated connection to the server, which acts as a bridge to external systems. When the model needs to take action, it invokes tools for structured operations. Contextual data (resources) is supplied to ground the LLM's tasks, and complex operations can be triggered via reusable prompts. The server can even adjust or influence inference through sampling control, while asynchronous updates flow back to the client via notifications. This cycle ensures consistent, auditable interactions across all MCP implementations.  

---

## 🌐 Key Architectural Considerations

**Security & Governance**: MCP doesn't standardize authentication, so implementations typically rely on OAuth, API keys, or enterprise SSO. Fine-grained RBAC (Role-Based Access Control) should be layered on top to avoid overexposing tools and resources to unauthorized users.

**FinOps & Observability**: MCP's modularity enables cost attribution at the level of prompts, tools, and resources. This aligns perfectly with FinOps practices by connecting usage patterns with cost accountability—you can see exactly which teams, projects, or prompts are driving your AI infrastructure costs.

**Extensibility**: MCP enables dynamic discovery of tools and capabilities. Clients can query a server to retrieve available resources and adjust their UI/UX accordingly, making MCP act as a "source of truth" for what's possible. This means adding a new FinOps tool doesn't require client updates—it's automatically discovered and made available.

---

## 🆕 MCP Specification 2025-11-25 Updates

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

## 📖 Additional Resources
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [One Year of MCP Blog Post](https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/)
- [MCP Authorization Spec Details](https://den.dev/blog/mcp-november-authorization-spec/)  
