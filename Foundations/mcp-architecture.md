# MCP Architecture

## 🏗️ Core Architectural Components  

The **Model Context Protocol (MCP)** defines a modular, layered architecture that separates AI orchestration, data access, and interaction logic.  
This design enables flexibility, reusability, and observability — essential principles for scalable and governable AI systems.  

### 1. MCP Client  
The **Client** is the AI-enabled application or agent that consumes context and invokes actions.  
**Responsibilities include:**  
- Discovering available tools and resources exposed by the protocol.  
- Dynamically invoking tools based on model intent.  
- Composing and reusing structured prompts.  
- Routing queries and contextual data through the MCP interface.  

---

### 2. MCP Host  
The **Host** acts as the orchestrator and runtime environment for one or more MCP Servers.  
It provides the operational, networking, and governance layers needed for enterprise-scale deployments.  
**Responsibilities include:**  
- Authentication and authorization between clients and services.  
- Load balancing and scaling across multiple MCP Servers.  
- Observability, logging, and usage metering (critical for FinOps).  
- Centralized governance and routing for AI traffic.  

---

### 3. MCP Server  
Each **Server** implements the protocol and exposes three fundamental resource types:  

- **Tools** → Declarative, model-invoked actions (e.g., query a database, call an API, write files).  
- **Resources** → Application-controlled data elements, either static (documents, metadata) or dynamic (logs, JSON records), used to ground model responses.  
- **Prompts** → Reusable, version-controlled interaction templates that standardize AI behavior, support cost attribution, and ensure consistency across use cases.  

---

### 4. Interoperability & Orchestration  
The true strength of MCP lies in how these components interoperate:  
1. The **Client** requests a prompt using a known template.  
2. The **Host** routes the request to the appropriate **Server**.  
3. The **Server** interpolates relevant **Resources**, invokes **Tools**, and assembles the final prompt.  
4. The result flows back through the **Host**, enabling auditing, logging, and metric collection.  

---

## 🔄 MCP Interaction Cycle  

Beyond the core components, MCP defines a standardized **interaction cycle** for communication between Clients, Hosts, and Servers:  

1. **Client Communication** – the Client maintains a dedicated connection to the Server.  
2. **Server Bridging** – the Server acts as a bridge to external systems.  
3. **Tool Invocation** – the model calls tools for structured actions.  
4. **Resource Provisioning** – contextual data is supplied to ground the LLM’s tasks.  
5. **Prompt Execution** – complex operations are triggered via prompts.  
6. **Sampling Control** – the Server can adjust or influence inference.  
7. **Notification Push** – asynchronous updates flow back to the Client.  

---

## 🌐 Key Architectural Considerations

- **Security & Governance**
  - MCP does not standardize authentication; implementations often rely on OAuth, API keys, or enterprise SSO.
  - Fine-grained RBAC (Role-Based Access Control) should be layered on top to avoid overexposure of tools and resources.

- **FinOps & Observability**
  - MCP's modularity enables cost attribution at the level of prompts, tools, and resources.
  - This aligns with FinOps practices by connecting usage patterns with cost accountability.

- **Extensibility**
  - MCP enables dynamic discovery of tools and capabilities.
  - Clients can query a Server to retrieve available resources and adjust UI/UX accordingly — making MCP act as a "source of truth" for capabilities.

---

## 🆕 MCP Specification 2025-11-25 Updates

On November 25, 2025, MCP released a major specification update to mark the protocol's first anniversary. Key enhancements include:

### 1. Task-Based Workflows
**What it is**: A new abstraction for tracking long-running operations performed by MCP servers.

**Why it matters for FinOps**:
- **Long-running cost analyses**: Multi-region pricing comparisons, complex optimization simulations, and large billing export queries can now run asynchronously.
- **Status tracking**: Clients can query task status (`working`, `input_required`, `completed`, `failed`, `cancelled`) and retrieve results when ready.
- **Durable requests**: Servers can persist task results for a configurable duration, reducing server load and improving reliability.
- **Polling and deferred retrieval**: No need to block on expensive operations; check back when the analysis is complete.

**Use case example**: Running a full year of multi-cloud billing data analysis across AWS, Azure, and GCP can take minutes. Tasks let you start the job, do other work, and retrieve results when ready.

### 2. Enhanced Authorization
**OAuth Client ID Metadata Documents (CIMD)**:
- Replaces complex Dynamic Client Registration (DCR) with simpler URL-based client metadata.
- Clients specify their `client_id` as a URL pointing to a JSON document with registration details.
- Reduces friction for enterprise MCP deployments.

**OAuth Client Credentials (SEP-1046)**:
- Enables machine-to-machine authorization without user interaction.
- Critical for automated FinOps workflows (CI/CD cost checks, scheduled reports, anomaly detection).

**Cross-App Access (SEP-990)**:
- Sign in once through an MCP client, gain access to all authorized servers.
- Eliminates repeated logins across multiple MCP servers in enterprise environments.
- Huge productivity boost for FinOps teams using many data sources (AWS, Azure, GCP, Vantage, etc.).

**Step-Up Authorization**:
- Handles insufficient permissions during runtime operations.
- If a tool requires elevated access (e.g., writing cost allocation tags), the system can request step-up auth on demand.

**PKCE Now Mandatory**:
- Proof Key for Code Exchange (PKCE) is now required for all OAuth flows.
- Significantly improves security for MCP servers exposed over public networks.

### 3. Why This Matters for FinOps
The 2025-11-25 spec addresses two major FinOps pain points:

**Production Readiness**:
- Tasks enable asynchronous cost analyses at scale
- Cross-app access reduces authentication friction
- Client credentials enable automated FinOps pipelines

**Enterprise Security**:
- Mandatory PKCE protects against authorization code interception
- Step-up authorization provides just-in-time privilege escalation
- CIMD simplifies client registration for IT teams

These updates move MCP from "prototype-friendly" to "enterprise-ready" for FinOps use cases.

---

## 📖 Additional Resources
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [One Year of MCP Blog Post](https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/)
- [MCP Authorization Spec Details](https://den.dev/blog/mcp-november-authorization-spec/)  
