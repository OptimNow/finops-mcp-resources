# Google Cloud MCP Server (krzko) - Implementation Guide

A comprehensive guide to implementing the krzko/google-cloud-mcp server for Google Cloud Platform FinOps and management.

## 🏗️ Server Overview

### Google Cloud MCP Server by krzko
**Repository**: [krzko/google-cloud-mcp](https://github.com/krzko/google-cloud-mcp)  
**Status**: 🧪 Community Project (Active Development)  
**Maintainer**: krzko (Community Developer)

A comprehensive Model Context Protocol server that connects to Google Cloud services, providing context and tools for interacting with your GCP resources through natural language.

#### 🎯 **Key Strength**: Multi-Service Integration
Unlike single-service MCP servers, this provides a unified interface to 8 core GCP services essential for FinOps and cloud management.

---

## 🛠️ Supported Google Cloud Services

### 💰 **Billing** (Primary FinOps Focus)
**Available Tools:**
- `gcp-billing-list-accounts` - List all billing accounts
- `gcp-billing-get-account-details` - Get specific billing account details
- `gcp-billing-list-projects` - List projects under billing account
- `gcp-billing-get-project-info` - Get project billing information
- `gcp-billing-list-services` - List available services
- `gcp-billing-list-skus` - List service SKUs and pricing
- `gcp-billing-analyse-costs` - Analyze costs with optimization insights
- `gcp-billing-detect-anomalies` - Detect billing anomalies
- `gcp-billing-cost-recommendations` - Generate cost optimization recommendations
- `gcp-billing-service-breakdown` - Break down costs by service

**FinOps Use Cases:**
- Daily cost monitoring and analysis
- Anomaly detection for unexpected charges
- Cost optimization recommendations
- Service-level cost breakdowns
- Budget planning and forecasting

### 📊 **Monitoring** (Performance & Cost Insights)
**Available Tools:**
- `gcp-monitoring-query-metrics` - Query CloudWatch-style metrics
- `gcp-monitoring-list-metric-types` - List available metrics
- `gcp-monitoring-query-natural-language` - Natural language metric queries

**FinOps Use Cases:**
- Resource utilization analysis
- Performance vs. cost correlation
- Right-sizing recommendations based on actual usage

### 🚨 **Error Reporting** (Operational Excellence)
**Available Tools:**
- `gcp-error-reporting-list-groups` - List error groups
- `gcp-error-reporting-get-group-details` - Get error details
- `gcp-error-reporting-analyse-trends` - Analyze error trends

### 🔐 **Identity and Access Management (IAM)** (Security & Governance)
**Available Tools:**
- `gcp-iam-get-project-policy` - Get IAM policies
- `gcp-iam-test-project-permissions` - Test permissions
- `gcp-iam-validate-deployment-permissions` - Validate deployment access
- `gcp-iam-analyse-permission-gaps` - Analyze permission gaps

### 📝 **Logging** (Audit & Troubleshooting)
**Available Tools:**
- `gcp-logging-query-logs` - Query log entries
- `gcp-logging-query-time-range` - Time-based log queries
- `gcp-logging-search-comprehensive` - Comprehensive log search

### 🗃️ **Cloud Spanner** (Database Management)
**Available Tools:**
- `gcp-spanner-execute-query` - Execute SQL queries
- `gcp-spanner-list-tables` - List database tables
- `gcp-spanner-query-natural-language` - Natural language database queries

### 📈 **Profiler** (Performance Analysis)
**Available Tools:**
- `gcp-profiler-list-profiles` - List performance profiles
- `gcp-profiler-analyse-performance` - Analyze bottlenecks
- `gcp-profiler-compare-trends` - Compare performance trends

### 🔍 **Trace** (Distributed Tracing)
**Available Tools:**
- `gcp-trace-get-trace` - Get specific traces
- `gcp-trace-list-traces` - List traces with filters
- `gcp-trace-find-from-logs` - Find traces from logs

---

## 🚀 Implementation Guide

### Prerequisites
- **Node.js 18+** installed
- **pnpm** package manager (`npm install -g pnpm`)
- **Google Cloud SDK** installed and configured
- **GCP Project** with billing enabled
- **Service Account** with appropriate permissions

### Step 1: Clone and Install

```bash
# Clone the repository
git clone https://github.com/krzko/google-cloud-mcp.git
cd google-cloud-mcp

# Install dependencies
pnpm install

# Build the project
pnpm build
```

### Step 2: Authentication Setup

#### Method 1: Service Account Key File (Recommended)

```bash
# Authenticate with Google Cloud
gcloud auth application-default login

# Or use a service account key file
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"
```

#### Method 2: Environment Variables

```bash
export GOOGLE_CLIENT_EMAIL="your-service-account@your-project.iam.gserviceaccount.com"
export GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYour-Private-Key-Here\n-----END PRIVATE KEY-----"
export GOOGLE_CLOUD_PROJECT="your-project-id"
```

### Step 3: Required GCP Permissions

**For FinOps/Billing Analysis:**
```json
{
  "roles": [
    "roles/billing.viewer",
    "roles/billing.admin",
    "roles/monitoring.viewer",
    "roles/logging.viewer",
    "roles/cloudsql.viewer",
    "roles/spanner.viewer"
  ]
}
```

**Custom Permission Set:**
- `billing.accounts.get`
- `billing.accounts.list`
- `billing.budgets.get`
- `billing.budgets.list`
- `monitoring.metricDescriptors.list`
- `monitoring.timeSeries.list`
- `logging.entries.list`

### Step 4: Client Configuration

#### For Claude Desktop
```json
{
  "mcpServers": {
    "google-cloud-mcp": {
      "command": "node",
      "args": [
        "/Users/yourname/code/google-cloud-mcp/dist/index.js"
      ],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/Users/yourname/.config/gcloud/application_default_credentials.json",
        "GOOGLE_CLOUD_PROJECT": "your-project-id"
      }
    }
  }
}
```

#### For VS Code MCP Extension
```json
{
  "mcp.servers": {
    "google-cloud-mcp": {
      "command": "node",
      "args": ["./dist/index.js"],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/credentials.json"
      }
    }
  }
}
```

---

## 💡 FinOps Use Cases & Examples

### 1. Cost Analysis Workflows

#### Daily Cost Monitoring
```
"Show me all my billing accounts"
"Analyse costs for project my-production-app for the last 30 days"
"Generate cost recommendations for billing account billingAccounts/123456-789ABC-DEF012"
```

#### Anomaly Detection
```
"Check for billing anomalies in project my-ecommerce-site"
"Detect unusual spending patterns in the last 7 days"
"Alert me to any cost spikes above 20% of normal usage"
```

### 2. Resource Optimization

#### Performance vs. Cost Analysis
```
"What's the CPU usage of Compute Engine instances in project infrastructure-prod?"
"Show me memory utilization for instances in project backend-services"
"Compare resource usage trends for the last month"
```

#### Right-Sizing Recommendations
```
"Analyze performance bottlenecks in service my-api in project backend-prod"
"Recommend instance types based on actual usage patterns"
"Identify underutilized resources for cost savings"
```

### 3. Operational Intelligence

#### Error Cost Impact
```
"Show me error groups from project my-webapp-prod for the last hour"
"Correlate error rates with compute costs"
"Analyze error trends for service my-payment-api"
```

#### Database Cost Optimization
```
"List all databases in Spanner instance my-instance in project ecommerce-prod"
"Analyze query performance vs. costs"
"Recommend database optimization opportunities"
```

---

## 🔧 Advanced Configuration

### Lazy Authentication (Recommended for Smithery)

For environments where authentication delays cause timeouts:

```json
{
  "mcpServers": {
    "google-cloud-mcp": {
      "command": "node",
      "args": ["/path/to/dist/index.js"],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/credentials.json"
      },
      "lazyAuth": true,
      "debug": true
    }
  }
}
```

### Debug Mode

```bash
# Enable debug logging
export DEBUG=google-cloud-mcp:*

# Start with inspector for debugging
npx -y @modelcontextprotocol/inspector node dist/index.js
```

---

## 🧪 Testing & Validation

### Step 1: Basic Connectivity Test

```bash
# Test the server directly
pnpm start
```

### Step 2: Validate Authentication

```
"List all my billing accounts"
"Show me available metric types for Compute Engine"
```

### Step 3: Test Core FinOps Functions

```
"Analyze costs for my main project for the last 7 days"
"Check for any billing anomalies"
"Show me cost recommendations"
```

### Step 4: Performance Validation

```
"Query CPU metrics for the last hour"
"Show me error rates for my main service"
"List recent traces for my application"
```

---

## 🚨 Troubleshooting

### Common Issues & Solutions

#### Authentication Errors
```bash
# Verify credentials are valid
gcloud auth application-default print-access-token

# Check service account permissions
gcloud projects get-iam-policy your-project-id
```

#### Timeout Issues
- Enable `lazyAuth: true` in configuration
- Set `debug: true` for detailed logging
- Verify network connectivity to GCP APIs

#### Permission Denied
- Review IAM roles for service account
- Ensure billing account access is granted
- Check project-level permissions

### Debug Commands

```bash
# Check authentication status
gcloud auth list

# Test API connectivity
gcloud billing accounts list

# Validate service account key
gcloud auth activate-service-account --key-file=path/to/key.json
```

---

## 📊 Server Comparison

| Feature | krzko/google-cloud-mcp | Official GCP MCP | Custom Solutions |
|---------|------------------------|------------------|------------------|
| **Multi-Service Support** | ✅ 8 Services | ❓ Limited | ⚠️ Custom |
| **Billing Analysis** | ✅ Comprehensive | ❓ Unknown | ⚠️ Manual |
| **Natural Language** | ✅ Full Support | ❓ Limited | ❌ |
| **Community Support** | ✅ Active GitHub | ❓ Official | ⚠️ Variable |
| **Documentation** | ✅ Good | ❓ TBD | ⚠️ Variable |
| **Maintenance** | ⚠️ Community | ✅ Google | ⚠️ Self |
| **Production Readiness** | ⚠️ Evaluate | ✅ Likely | ⚠️ Depends |

---

## 🎯 Best Practices

### 1. Security
- Use service account keys, not personal credentials
- Follow principle of least privilege
- Regularly rotate service account keys
- Enable audit logging for billing access

### 2. Cost Management
- Start with read-only permissions
- Monitor API usage and costs
- Set up billing alerts for MCP usage
- Use project isolation for testing

### 3. Performance
- Enable lazy authentication for better startup times
- Cache frequently accessed data
- Use appropriate time ranges for queries
- Monitor MCP server resource usage

---

## 📈 Next Steps

1. **Start with billing analysis** - Primary FinOps value
2. **Test monitoring integration** - Resource optimization
3. **Explore multi-service workflows** - Combine billing + monitoring + logs
4. **Build custom automation** - Scheduled cost reports
5. **Share findings** with the community

---

## 🤝 Community & Support

- **GitHub Issues**: [krzko/google-cloud-mcp/issues](https://github.com/krzko/google-cloud-mcp/issues)
- **Documentation**: Repository README and examples
- **Community**: MCP Discord and Google Cloud communities
- **Contributions**: PRs welcome for improvements

---

**Last Updated**: January 2025  
**Implementation Status**: ✅ Ready for Testing  
**Production Readiness**: ⚠️ Community Project - Evaluate First
