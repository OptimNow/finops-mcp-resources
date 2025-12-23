# Remote MCP Servers for FinOps

**Last Updated**: December 2025

Remote MCP servers are cloud-hosted or network-accessible MCP servers that clients connect to over HTTP/HTTPS or Server-Sent Events (SSE), as opposed to local servers running on the same machine as the MCP client.

Remote MCP support became mainstream in 2025, with Claude Code adding native support in June 2025 and other major clients following suit.

---

## 🌐 What Are Remote MCP Servers?

### Local MCP Servers (Traditional)
- Run as processes on the same machine as the MCP client
- Typically launched via STDIO (standard input/output)
- Example: `npx @modelcontextprotocol/server-aws-pricing`
- **Pros**: No network latency, simpler auth, no hosting costs
- **Cons**: Must install/manage on every client machine, can't share across teams

### Remote MCP Servers (Modern)
- Run as cloud-hosted services accessible over the network
- Connect via HTTP/HTTPS or Server-Sent Events (SSE)
- Example: `https://mcp.finops-corp.example.com/aws-pricing`
- **Pros**: Centralized management, shared across teams, no local installation
- **Cons**: Requires hosting infrastructure, network latency, more complex auth

---

## ✅ Benefits of Remote MCP Servers for FinOps

### 1. Centralized Management
- **Single source of truth**: One MCP server instance serves entire organization
- **Easier updates**: Update server once instead of updating every client machine
- **Consistent behavior**: All users get same data, calculations, and tool versions
- **Cost efficiency**: Run one powerful server instead of many local instances

### 2. Enterprise Security
- **Centralized credentials**: Store cloud credentials (AWS, Azure, GCP) in one secure location
- **No credential distribution**: Users never need direct access to billing APIs
- **Enhanced auditing**: All queries logged centrally for compliance
- **Network controls**: IP allowlisting, VPCs, WAF protection

### 3. Better Performance
- **Caching**: Share cached pricing data, billing exports across all users
- **Compute power**: Run complex cost analyses on powerful cloud instances
- **Reduced API calls**: Batch requests, deduplicate queries, respect rate limits centrally
- **Faster for remote teams**: Cloud-hosted servers often faster than local execution

### 4. Team Collaboration
- **Shared context**: Multiple FinOps team members query same live data
- **Cross-app access**: Single sign-on across all MCP servers (2025-11-25 spec)
- **Consistent results**: Everyone sees same cost data, eliminating discrepancies

---

## 🏗️ Architecture Patterns

### Pattern 1: Cloud-Hosted MCP Server (Recommended for Enterprises)

```
┌─────────────────────────────────────────────────────┐
│ MCP Clients (Claude Code, ChatGPT, VS Code, etc.)  │
│ - Developer workstations                           │
│ - CI/CD pipelines                                  │
│ - FinOps dashboards                                │
└──────────────────┬──────────────────────────────────┘
                   │ HTTPS / SSE
                   │ (OAuth 2.0 + PKCE)
                   ▼
┌─────────────────────────────────────────────────────┐
│ Load Balancer (ALB / Azure LB / GCP LB)            │
│ - TLS termination                                  │
│ - WAF protection                                   │
│ - IP allowlisting                                  │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ MCP Server Cluster (Auto-scaling)                  │
│ - AWS: ECS Fargate / Lambda                        │
│ - Azure: Container Apps / Functions                │
│ - GCP: Cloud Run / Cloud Functions                 │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ Cloud Provider APIs                                 │
│ - AWS Cost Explorer, Pricing API                   │
│ - Azure Cost Management                            │
│ - GCP BigQuery (billing exports)                   │
└─────────────────────────────────────────────────────┘
```

**Best for**:
- Large FinOps teams (10+ users)
- Multi-account/multi-cloud environments
- Organizations with strict security/compliance requirements

---

### Pattern 2: Shared Team Server (Mid-sized Teams)

```
┌─────────────────────────────────────────────────────┐
│ FinOps Team (5-10 users)                           │
│ - Claude Code, ChatGPT, VS Code clients            │
└──────────────────┬──────────────────────────────────┘
                   │ HTTPS
                   ▼
┌─────────────────────────────────────────────────────┐
│ Single Cloud Instance (t3.medium / Standard_D2s_v3)│
│ - Docker container running MCP server              │
│ - Nginx reverse proxy with basic auth             │
│ - Let's Encrypt TLS certificate                    │
└──────────────────┬──────────────────────────────────┘
                   │ IAM role / Service principal
                   ▼
┌─────────────────────────────────────────────────────┐
│ Cloud Provider Billing APIs                        │
└─────────────────────────────────────────────────────┘
```

**Best for**:
- Small to mid-sized teams
- Teams starting remote MCP journey
- Budget-conscious deployments

---

### Pattern 3: Hybrid Local + Remote (Flexible)

```
Developer 1 (local server)        FinOps Team (remote server)
       │                                    │
       │                                    │
       └──────────┬───────────────────────┘
                  │
                  ▼
          MCP Client Configuration
          - Local server for dev/test
          - Remote server for production queries
```

**Best for**:
- Development teams needing offline access
- Organizations transitioning from local to remote
- Scenarios with mixed connectivity (on-prem + cloud)

---

## 🔧 Implementation Guide

### Step 1: Choose Your Hosting Platform

| Platform | Best For | MCP Transport | Pricing Model |
|----------|----------|---------------|---------------|
| **AWS Lambda** | Serverless, pay-per-use | HTTP | Per-request |
| **AWS ECS Fargate** | Containerized, always-on | SSE, HTTP | Per-hour |
| **Azure Container Apps** | Containerized, auto-scaling | SSE, HTTP | Per-second |
| **Azure Functions** | Serverless, pay-per-use | HTTP | Per-execution |
| **GCP Cloud Run** | Containerized, serverless | SSE, HTTP | Per-request |
| **GCP Cloud Functions** | Serverless, pay-per-use | HTTP | Per-invocation |
| **Kubernetes** | Self-managed, full control | SSE, HTTP | Infrastructure costs |
| **VM / VPS** | Simple, persistent | SSE, HTTP | Monthly flat rate |

---

### Step 2: Configure MCP Server for Remote Access

**Example: AWS Pricing MCP Server on Cloud Run (Node.js)**

```javascript
// server.js
const { Server } = require('@modelcontextprotocol/sdk/server/index.js');
const { SSEServerTransport } = require('@modelcontextprotocol/sdk/server/sse.js');
const express = require('express');

const app = express();
const server = new Server({
  name: 'aws-pricing-remote',
  version: '1.0.0',
});

// Register MCP tools (pricing queries, etc.)
server.setRequestHandler('tools/list', async () => {
  return { tools: [/* your tools */] };
});

// SSE endpoint
app.get('/sse', async (req, res) => {
  const transport = new SSEServerTransport('/message', res);
  await server.connect(transport);
});

app.post('/message', async (req, res) => {
  // Handle MCP messages
});

const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Remote MCP server running on port ${PORT}`);
});
```

**Dockerfile:**
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]
```

---

### Step 3: Implement Authentication

**Option 1: OAuth 2.0 with PKCE (Recommended)**
```javascript
const { OAuth2Server } = require('oauth2-server');

app.use('/oauth', (req, res, next) => {
  // Implement OAuth 2.0 endpoints
  // - /oauth/authorize
  // - /oauth/token
  // - Validate PKCE code_challenge
});

app.use('/sse', authenticateOAuth, (req, res) => {
  // SSE endpoint protected by OAuth
});
```

**Option 2: API Keys (Simple, less secure)**
```javascript
const API_KEYS = process.env.MCP_API_KEYS.split(',');

app.use('/sse', (req, res, next) => {
  const apiKey = req.headers['x-api-key'];
  if (!API_KEYS.includes(apiKey)) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
});
```

---

### Step 4: Deploy to Cloud

**AWS ECS Fargate Example:**
```bash
# Build and push Docker image
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account>.dkr.ecr.us-east-1.amazonaws.com
docker build -t mcp-aws-pricing .
docker tag mcp-aws-pricing:latest <account>.dkr.ecr.us-east-1.amazonaws.com/mcp-aws-pricing:latest
docker push <account>.dkr.ecr.us-east-1.amazonaws.com/mcp-aws-pricing:latest

# Create ECS service
aws ecs create-service \
  --cluster finops-mcp \
  --service-name mcp-aws-pricing \
  --task-definition mcp-aws-pricing:1 \
  --desired-count 2 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xxx],securityGroups=[sg-xxx],assignPublicIp=ENABLED}" \
  --load-balancers "targetGroupArn=arn:aws:elasticloadbalancing:...,containerName=mcp-aws-pricing,containerPort=8080"
```

**GCP Cloud Run Example:**
```bash
# Build and deploy
gcloud builds submit --tag gcr.io/project-id/mcp-aws-pricing
gcloud run deploy mcp-aws-pricing \
  --image gcr.io/project-id/mcp-aws-pricing \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated  # Or use --no-allow-unauthenticated + IAM
```

---

### Step 5: Configure MCP Client

**Claude Code (.claude/mcp.json):**
```json
{
  "mcpServers": {
    "aws-pricing-remote": {
      "url": "https://mcp.finops-corp.example.com/sse",
      "transport": "sse",
      "headers": {
        "Authorization": "Bearer ${secrets:mcp-oauth-token}"
      }
    }
  }
}
```

**VS Code (settings.json):**
```json
{
  "mcp.servers": {
    "aws-pricing": {
      "type": "remote",
      "url": "https://mcp.finops-corp.example.com/sse",
      "auth": {
        "type": "oauth2",
        "tokenUrl": "https://auth.finops-corp.example.com/oauth/token",
        "clientId": "https://finops-corp.example.com/mcp-client.json"
      }
    }
  }
}
```

---

## 🔐 Security Best Practices

1. **Always use HTTPS/TLS** - Never expose MCP servers over plain HTTP
2. **Implement OAuth 2.0 with PKCE** - Required by MCP spec 2025-11-25
3. **Use secrets managers** - Store credentials in AWS Secrets Manager, Azure Key Vault, GCP Secret Manager
4. **Enable comprehensive logging** - CloudTrail, Azure Monitor, Cloud Logging
5. **IP allowlisting** - Restrict access to known corporate IPs if possible
6. **VPC/Private endpoints** - Keep MCP servers private, use VPN/bastion for access
7. **Regular security audits** - Penetration testing, dependency scanning
8. **Rate limiting** - Prevent abuse, protect cloud provider API quotas

---

## 💰 Cost Considerations

### Remote Server Hosting Costs
- **Serverless** (Lambda, Cloud Functions): $0.20-$1.00 per million requests
- **Container platforms** (Cloud Run, Container Apps): $0.05-$0.15 per hour
- **Always-on VMs**: $20-$100 per month depending on size
- **Load balancers**: $15-$25 per month

### Cost Savings from Remote Servers
- **Reduced API calls**: Caching and deduplication save 30-50% on cloud provider API costs
- **Shared compute**: One powerful server cheaper than many local instances
- **Efficiency**: Centralized optimization reduces overall cloud spend

**ROI estimate**: For teams of 10+ users, remote MCP servers typically pay for themselves within 1-2 months through reduced API costs and improved efficiency.

---

## 📖 Additional Resources

- [Claude Code Remote MCP Documentation](https://code.claude.com/docs/en/mcp)
- [MCP SDK - SSE Server Transport](https://github.com/modelcontextprotocol/sdk)
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [OAuth 2.0 for MCP](https://den.dev/blog/mcp-november-authorization-spec/)
- [Remote MCP Announcement](https://www.anthropic.com/news/claude-code-remote-mcp)

---

## 🤝 Contributing

Have experience deploying remote MCP servers for FinOps? Share your learnings via Issues or PRs!
