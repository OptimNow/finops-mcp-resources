# FinOps Tag Compliance MCP Server - Specification

**Version**: 1.0
**Last Updated**: December 2024
**Status**: Specification (Implementation Pending)
**Type**: Remote MCP Server (Cloud-Hosted)
**Target Audience**: FinOps Practitioners, Solution Architects, DevOps Engineers

---

## Executive Summary

The **FinOps Tag Compliance MCP Server** is a purpose-built remote MCP server that solves the tag governance challenge plaguing enterprise cloud environments. While generic cloud MCP servers (AWS, Azure, GCP) provide raw tag data access, this server adds intelligence: schema validation, cost attribution analysis, cross-cloud consistency enforcement, and ML-powered tag suggestions.

**The Problem**: 30-50% of cloud resources are untagged or incorrectly tagged in typical enterprises, making cost allocation impossible and compliance reporting a nightmare.

**The Solution**: A centralized MCP server that validates tags against your organization's policies, connects tag violations to dollar impact, suggests remediation, and automates bulk tagging workflows with approval controls.

---

## Differentiation from Official Cloud MCP Servers

| Capability | Official AWS/Azure/GCP MCP | FinOps Tag Compliance MCP |
|------------|---------------------------|---------------------------|
| **Tag Data Access** | ✅ Read tags via API | ✅ Read tags via API |
| **Schema Validation** | ❌ No | ✅ Validates against org policy |
| **Cross-Cloud Consistency** | ❌ Single cloud only | ✅ AWS + Azure + GCP unified |
| **Cost Attribution** | ❌ No | ✅ Links violations to $ impact |
| **Tag Suggestions** | ❌ No | ✅ ML-powered recommendations |
| **Bulk Tagging Workflows** | ❌ No | ✅ Step-up auth + approval |
| **Compliance Reporting** | ❌ No | ✅ Scheduled audits + dashboards |
| **Violation Tracking** | ❌ No | ✅ Historical trend analysis |

**Key Value Add**: This MCP server transforms raw tag data into actionable FinOps intelligence.

---

## Architecture

### Deployment Model

**Type**: Remote MCP Server (cloud-hosted)

**Why Remote**:
- Centralized tag schema enforcement across organization
- Secure credential management (multi-cloud in secrets manager)
- Shared cache for compliance violations
- Team collaboration on remediation
- Approval workflows for bulk operations
- Unified audit logs for compliance

### Recommended Hosting

**Primary**: AWS ECS Fargate / GCP Cloud Run / Azure Container Apps
**Fallback**: Kubernetes cluster
**Data Store**: Redis for caching, PostgreSQL for audit logs
**Secrets**: AWS Secrets Manager / Azure Key Vault / GCP Secret Manager

### High-Level Architecture

```
┌──────────────────────────────────────────────┐
│ MCP Clients (Claude Desktop, ChatGPT, etc.) │
└────────────────┬─────────────────────────────┘
                 │ HTTPS + OAuth 2.0 + PKCE
                 ▼
┌──────────────────────────────────────────────┐
│   FinOps Tag Compliance MCP Server           │
│                                              │
│   Authentication: Enterprise SSO via OAuth   │
│   Authorization: Read-only + Step-up Auth    │
│                                              │
│   Tools (15):                                │
│   - check_tag_compliance()                   │
│   - validate_resource_tags()                 │
│   - find_untagged_resources()                │
│   - suggest_tags()                           │
│   - bulk_tag_resources() [step-up required] │
│   - generate_compliance_report()             │
│   - get_cost_attribution_gap()               │
│   - detect_tag_drift()                       │
│   - preview_bulk_tagging()                   │
│   - get_violation_history()                  │
│   - suggest_tag_schema_updates()             │
│   - cross_cloud_consistency_check()          │
│   - export_violations_csv()                  │
│   - schedule_compliance_audit()              │
│   - approve_bulk_tagging_request()           │
│                                              │
│   Resources (5):                             │
│   - tagging_policy.json                      │
│   - violations_cache                         │
│   - cost_attribution_data                    │
│   - tag_suggestion_models                    │
│   - audit_log                                │
│                                              │
│   Prompts (3):                               │
│   - "Run monthly compliance audit"           │
│   - "Find and fix common tag violations"     │
│   - "Generate executive tagging report"      │
└────────────────┬─────────────────────────────┘
                 │
        ┌────────┴────────┬─────────────┐
        ▼                 ▼             ▼
  ┌──────────┐      ┌──────────┐  ┌──────────┐
  │   AWS    │      │  Azure   │  │   GCP    │
  │ Cost Exp │      │  Cost    │  │ BigQuery │
  │ Tagging  │      │  Mgmt    │  │ Labels   │
  └──────────┘      └──────────┘  └──────────┘
```

---

## Use Cases

### Use Case 1: Monthly Tag Compliance Audit

**User**: FinOps Practitioner
**Scenario**: End-of-month compliance report for CFO

**Interaction**:
```
User: "Run a complete tag compliance audit across all clouds and show me
       the cost impact of violations"

MCP Server Response:
Tag Compliance Audit - December 2024

Overall Compliance: 67% (down 3% from last month)

Violations by Cloud:
- AWS:   234 resources untagged ($47,230/month impact)
- Azure: 156 resources untagged ($28,940/month impact)
- GCP:    89 resources untagged ($12,330/month impact)

Total Cost Attribution Gap: $88,500/month (18% of cloud spend)

Top Violations:
1. Missing CostCenter tag: 127 resources ($31,200/month)
2. Missing Owner tag: 94 resources ($22,100/month)
3. Incorrect Environment tag: 67 resources ($15,800/month)
4. Tag naming inconsistency: 51 resources ($11,400/month)

Recommended Actions:
1. Bulk-tag EC2 production instances with CostCenter=Engineering
   (Preview: Would fix 42 violations, recover $12,300/month allocation)
2. Standardize Environment tags (prod → production) across AWS
   (Preview: Would fix 67 violations)
3. Review untagged RDS instances with platform-team@ (no clear owner)

Export full report to CSV? [yes/no]
Schedule automated monthly audits? [yes/no]
```

**Value**: Turns raw tag data into executive-ready financial reports.

---

### Use Case 2: Real-Time Tag Validation (Pre-Deployment)

**User**: DevOps Engineer
**Scenario**: CI/CD pipeline validates tags before infrastructure deployment

**Interaction**:
```
User: "Check if my new Terraform resources in staging comply with
       our tag schema before I apply"

MCP Server Response:
Scanning terraform plan output...

✅ Compliant: 12 resources
❌ Violations Found: 3 resources

Violations:
1. aws_instance.web_server[0]
   - Missing required tag: CostCenter
   - Missing required tag: Owner
   - Invalid Environment value: "stage" (allowed: dev, staging, production)

2. aws_rds_instance.db
   - Tag naming inconsistency: "cost-center" should be "CostCenter"

3. aws_s3_bucket.data
   - Missing required tag: DataClassification

Suggested Fixes:
resource "aws_instance" "web_server" {
  tags = {
    Name           = "web-server"
    Environment    = "staging"  # Changed from "stage"
    CostCenter     = "Engineering"  # Suggested based on subnet pattern
    Owner          = "platform-team@company.com"  # Suggested based on repo
    ManagedBy      = "terraform"
  }
}

Apply these suggestions? [yes/no]
```

**Value**: Prevents tag violations at deployment time, not after resources are live.

---

### Use Case 3: Intelligent Bulk Tagging with Approval

**User**: FinOps Practitioner
**Scenario**: Fix 50+ untagged EC2 instances discovered in audit

**Interaction**:
```
User: "Tag all untagged EC2 instances in production VPC with
       Environment=production and CostCenter=Engineering"

MCP Server Response:
Bulk Tagging Preview

Scope: EC2 instances in vpc-prod-east-1 with missing tags
Affected Resources: 47 instances
Estimated Cost Impact: $23,400/month (will enable proper cost allocation)

Tags to Apply:
- Environment: production
- CostCenter: Engineering

Safety Checks:
✅ All instances are in production VPC
✅ No existing conflicting tags detected
⚠️  This is a write operation requiring elevated permissions

Step-Up Authorization Required

Current permissions: Read-only
Required permissions: Write tags (ec2:CreateTags)

Approval workflow triggered:
1. Request sent to manager: jane.smith@company.com
2. Requires MFA re-authentication
3. Write token valid for 30 minutes after approval
4. All changes will be logged to audit trail

Awaiting approval... [cancel request]

---

[After approval]

Approval granted by jane.smith@company.com at 2024-12-15 14:23:45 UTC

Applying tags to 47 EC2 instances...
Progress: ████████████████████ 47/47 (100%)

✅ Successfully tagged 47 instances
📊 Compliance improved: 67% → 74% (+7%)
💰 Cost attribution gap reduced: $88,500 → $65,100 (-$23,400)

Audit log entry: audit-2024-12-15-142412.json
```

**Value**: Safe, auditable bulk operations with built-in approval workflows.

---

### Use Case 4: Cross-Cloud Tag Consistency Enforcement

**User**: Solution Architect
**Scenario**: Ensure tagging standards are consistent across AWS, Azure, and GCP

**Interaction**:
```
User: "Find tag naming inconsistencies across all three clouds for
       the analytics project"

MCP Server Response:
Cross-Cloud Tag Consistency Analysis - Project: analytics

Naming Inconsistencies Detected:

1. Cost Center Tag:
   - AWS: "CostCenter" (42 resources)
   - Azure: "cost-center" (28 resources) ⚠️ Inconsistent
   - GCP: "costcenter" (19 resources) ⚠️ Inconsistent

   Recommendation: Standardize to "CostCenter" (matches org policy)
   Impact: 47 resources need remediation

2. Environment Tag:
   - AWS: "Environment" (38 resources)
   - Azure: "env" (31 resources) ⚠️ Inconsistent
   - GCP: "Environment" (22 resources)

   Recommendation: Standardize to "Environment"
   Impact: 31 Azure resources need remediation

3. Owner Tag:
   - AWS: "Owner" (42 resources)
   - Azure: "Owner" (35 resources)
   - GCP: "owner" (19 resources) ⚠️ Case mismatch

   Recommendation: Standardize to "Owner" (case-sensitive)
   Impact: 19 GCP resources need remediation

Total Resources Requiring Update: 97
Estimated Time to Fix: 15 minutes (bulk operations)

Generate remediation plan? [yes/no]
Apply fixes across all clouds? [yes/no] [requires approval]
```

**Value**: Impossible to achieve with single-cloud MCP servers. Unified governance.

---

### Use Case 5: Cost Attribution Gap Analysis

**User**: FinOps Practitioner
**Scenario**: CFO asks "Why can't we allocate 18% of our cloud spend?"

**Interaction**:
```
User: "Show me exactly which resources are causing our cost allocation
       gaps and why"

MCP Server Response:
Cost Attribution Gap Analysis

Unallocated Spend: $88,500/month (18% of total cloud spend)

Breakdown by Reason:

1. Missing CostCenter Tag: $31,200/month (35% of gap)
   Top Contributors:
   - EC2 instances (us-east-1): $12,300 (42 instances)
   - RDS databases (eu-west-1): $8,900 (12 databases)
   - S3 storage (global): $6,700 (89 buckets)
   - EBS volumes (orphaned): $3,300 (67 volumes)

2. Missing Owner Tag: $22,100/month (25% of gap)
   Top Contributors:
   - Lambda functions: $8,400 (234 functions)
   - Azure VMs: $7,200 (23 VMs)
   - GCP Compute instances: $6,500 (18 instances)

3. Tag Naming Inconsistency: $19,400/month (22% of gap)
   - "cost-center" vs "CostCenter": $11,200
   - "env" vs "Environment": $8,200

4. Invalid Tag Values: $15,800/month (18% of gap)
   - Environment="stage" (not in allowed values): $8,900
   - CostCenter="legacy" (deprecated value): $6,900

Pattern Analysis:
- 67% of violations are in resources >90 days old
- US-East-1 has 3x more violations than other regions
- Infrastructure team has 2x violation rate vs application teams

Quick Wins (can fix immediately):
1. Bulk-tag 42 EC2 prod instances → Recover $12,300/month
2. Standardize Environment tags → Recover $8,200/month
3. Update deprecated CostCenter values → Recover $6,900/month

Total Quick Win Impact: $27,400/month (31% gap reduction)

Generate executive summary? [yes/no]
Start remediation workflow? [yes/no]
```

**Value**: Connects tag violations directly to financial impact for CFO-level reporting.

---

## Tool Specifications

### Tool 1: `check_tag_compliance`

**Purpose**: Validate resources against organization's tagging policy

**Parameters**:
```json
{
  "cloud_provider": "aws | azure | gcp | all",
  "filters": {
    "region": "string (optional)",
    "account_id": "string (optional)",
    "resource_type": "string (optional)",
    "project": "string (optional)"
  },
  "severity": "errors_only | warnings_only | all"
}
```

**Returns**:
```json
{
  "compliance_rate": "67%",
  "total_resources": 1247,
  "compliant_resources": 836,
  "violations": [
    {
      "resource_id": "i-abc123",
      "resource_type": "ec2_instance",
      "cloud": "aws",
      "region": "us-east-1",
      "violations": [
        {
          "type": "missing_required_tag",
          "tag_name": "CostCenter",
          "severity": "error"
        }
      ],
      "cost_impact_monthly": 240.50,
      "suggested_fix": {
        "tag": "CostCenter",
        "value": "Engineering",
        "confidence": "high",
        "reason": "Based on subnet pattern vpc-eng-prod"
      }
    }
  ],
  "cost_attribution_gap": 88500.00
}
```

---

### Tool 2: `bulk_tag_resources`

**Purpose**: Apply tags to multiple resources with approval workflow

**Authorization**: Requires step-up authorization (write permissions)

**Parameters**:
```json
{
  "resource_ids": ["i-abc123", "i-def456", "..."],
  "tags": {
    "CostCenter": "Engineering",
    "Environment": "production"
  },
  "dry_run": true,
  "require_approval": true,
  "approver_email": "manager@company.com"
}
```

**Workflow**:
1. Validate tags against schema
2. Calculate impact (resources affected, cost attribution improvement)
3. If `require_approval=true`: Send approval request
4. Await approval via email/Slack/webhook
5. Upon approval: Request step-up authorization from user
6. Apply tags
7. Log to audit trail

**Returns**:
```json
{
  "status": "pending_approval | approved | completed | failed",
  "approval_request_id": "req-abc123",
  "resources_affected": 47,
  "estimated_impact": {
    "compliance_improvement": "7%",
    "cost_attribution_gap_reduction": 23400.00
  },
  "approval_url": "https://mcp.company.com/approve/req-abc123"
}
```

---

### Tool 3: `suggest_tags`

**Purpose**: ML-powered tag suggestions based on resource patterns

**Parameters**:
```json
{
  "resource_id": "i-abc123",
  "cloud_provider": "aws | azure | gcp"
}
```

**Returns**:
```json
{
  "suggestions": [
    {
      "tag_name": "CostCenter",
      "suggested_value": "Engineering",
      "confidence": "high",
      "reasoning": "95% of resources in subnet vpc-eng-prod have CostCenter=Engineering"
    },
    {
      "tag_name": "Owner",
      "suggested_value": "platform-team@company.com",
      "confidence": "medium",
      "reasoning": "Created by IAM user john.doe@company.com who is in platform-team"
    },
    {
      "tag_name": "Environment",
      "suggested_value": "production",
      "confidence": "high",
      "reasoning": "VPC name contains 'prod', instance type is production-grade (m5.2xlarge)"
    }
  ]
}
```

**ML Model**: Pattern recognition based on:
- VPC/subnet naming conventions
- IAM user/role patterns
- Instance types/sizes
- Historical tagging patterns
- Team membership from SSO

---

### Tool 4: `get_cost_attribution_gap`

**Purpose**: Calculate financial impact of tag violations

**Parameters**:
```json
{
  "cloud_provider": "aws | azure | gcp | all",
  "time_range": "last_30_days | last_90_days | current_month",
  "group_by": "tag_name | resource_type | cloud | team"
}
```

**Returns**:
```json
{
  "total_cloud_spend": 491700.00,
  "allocated_spend": 403200.00,
  "unallocated_spend": 88500.00,
  "allocation_rate": "82%",
  "gap_breakdown": [
    {
      "reason": "missing_cost_center_tag",
      "amount": 31200.00,
      "percentage": "35%",
      "resource_count": 127,
      "quick_win_potential": "high"
    }
  ],
  "historical_trend": [
    {"month": "2024-09", "gap": 92300.00},
    {"month": "2024-10", "gap": 89100.00},
    {"month": "2024-11", "gap": 88500.00}
  ]
}
```

---

## Resource Specifications

### Resource 1: `tagging_policy.json`

**Purpose**: Organization's tagging schema and enforcement rules

**Format**:
```json
{
  "version": "2.1",
  "last_updated": "2024-12-01",
  "required_tags": [
    {
      "name": "CostCenter",
      "description": "Department or team for cost allocation",
      "allowed_values": ["Engineering", "Marketing", "Sales", "Operations"],
      "validation_regex": "^[A-Z][a-z]+$",
      "applies_to": ["aws:*", "azure:*", "gcp:*"]
    },
    {
      "name": "Owner",
      "description": "Email of resource owner",
      "validation_regex": "^[a-z0-9._%+-]+@company\\.com$",
      "applies_to": ["aws:*", "azure:*", "gcp:*"]
    },
    {
      "name": "Environment",
      "description": "Deployment environment",
      "allowed_values": ["dev", "staging", "production"],
      "applies_to": ["aws:*", "azure:*", "gcp:*"]
    }
  ],
  "optional_tags": [
    {
      "name": "DataClassification",
      "allowed_values": ["public", "internal", "confidential", "restricted"],
      "recommended_for": ["aws:s3:bucket", "azure:storage:account"]
    }
  ],
  "naming_conventions": {
    "case_sensitive": true,
    "allow_hyphens": false,
    "allow_underscores": false
  },
  "exemptions": [
    {
      "resource_pattern": "aws:ec2:instance:i-legacy-*",
      "exempt_tags": ["CostCenter"],
      "reason": "Legacy instances scheduled for decommission",
      "expires": "2025-06-30"
    }
  ]
}
```

**Storage**: S3 bucket / Azure Blob / GCS bucket (version controlled)

---

### Resource 2: `violations_cache`

**Purpose**: Redis cache of current tag violations for fast queries

**TTL**: 6 hours (refreshed on schedule or on-demand)

**Format**:
```json
{
  "last_updated": "2024-12-15T14:30:00Z",
  "violations_by_resource": {
    "aws:i-abc123": {
      "violations": ["missing:CostCenter", "missing:Owner"],
      "cost_monthly": 240.50,
      "last_checked": "2024-12-15T14:30:00Z"
    }
  },
  "violations_by_tag": {
    "CostCenter": {
      "missing_count": 127,
      "cost_impact": 31200.00
    }
  }
}
```

---

## Prompts

### Prompt 1: "Run monthly compliance audit"

**Template**:
```
Execute comprehensive tag compliance audit:
1. Check compliance across all clouds (AWS, Azure, GCP)
2. Calculate cost attribution gap
3. Identify top 10 violations by financial impact
4. Generate executive summary with trend analysis
5. Recommend 3 quick wins for immediate remediation
6. Export full report to CSV

Format output for CFO presentation.
```

### Prompt 2: "Find and fix common tag violations"

**Template**:
```
Identify most common tag violations:
1. Find top 5 violation patterns by resource count
2. Suggest bulk-tagging fixes with confidence scores
3. Preview impact of each fix (compliance % improvement, cost allocation)
4. Generate approval requests for bulk operations
5. Track remediation progress

Focus on quick wins that can be fixed in <1 hour.
```

### Prompt 3: "Generate executive tagging report"

**Template**:
```
Create executive summary of tag compliance:
1. Overall compliance rate with month-over-month trend
2. Cost attribution gap in dollars and percentage
3. Compliance by cloud provider (AWS vs Azure vs GCP)
4. Compliance by team/department
5. ROI of remediation (cost to fix vs cost allocation value)
6. Risk assessment (compliance, audit, showback/chargeback impact)

Format for non-technical executive audience. Include visualizations.
```

---

## Security Model

### Authentication

**Method**: OAuth 2.0 with PKCE via enterprise SSO (Okta, Azure AD, Google Workspace)

**Flow**:
1. User initiates MCP connection
2. Redirected to enterprise SSO
3. OAuth + PKCE exchange
4. Issued read-only access token (8-hour lifetime)

### Authorization

**Default**: Read-only access (all query tools)

**Step-Up Required For**:
- `bulk_tag_resources()` - Write tags
- `approve_bulk_tagging_request()` - Approve others' bulk operations

**Step-Up Flow**:
1. User attempts write operation
2. Server triggers step-up authorization
3. User re-authenticates with MFA
4. Approval request sent to manager (configurable)
5. Upon approval: Issue write-scoped token (30-minute TTL)
6. Execute operation
7. Token expires, reverts to read-only

### Audit Logging

**All Operations Logged**:
- Who (user identity from SSO)
- What (tool invoked, parameters)
- When (ISO 8601 timestamp)
- Where (IP address, MCP client)
- Result (success/failure, resources affected)

**Log Retention**: 2 years (configurable)

**Storage**: PostgreSQL database, exported to CloudWatch/Azure Monitor/Cloud Logging

---

## IAM Permissions Required

### AWS

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "tag:GetResources",
        "tag:GetTagKeys",
        "tag:GetTagValues",
        "ce:GetCostAndUsage",
        "ec2:DescribeInstances",
        "ec2:DescribeTags",
        "rds:DescribeDBInstances",
        "rds:ListTagsForResource",
        "s3:GetBucketTagging",
        "s3:ListAllMyBuckets"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateTags",
        "rds:AddTagsToResource",
        "s3:PutBucketTagging"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": ["us-east-1", "us-west-2"]
        }
      }
    }
  ]
}
```

### Azure

**Role**: Cost Management Reader + Tag Contributor (limited scope)

### GCP

**Roles**:
- `roles/cloudasset.viewer` (read tags)
- `roles/resourcemanager.tagUser` (write tags, conditional)
- `roles/billing.viewer` (cost data)

---

## Performance Requirements

- **Tag Compliance Check**: <5 seconds for 1000 resources
- **Cost Attribution Query**: <10 seconds across 3 clouds
- **Bulk Tagging**: <30 seconds for 100 resources
- **Cache Hit Rate**: >80% for compliance queries
- **API Rate Limit Compliance**: Never exceed cloud provider limits

---

## Cost Estimate

**Infrastructure** (AWS ECS Fargate example):
- ECS Tasks: 2x Fargate containers (1 vCPU, 2GB RAM) = $50/month
- Redis Cache: ElastiCache t3.micro = $12/month
- PostgreSQL: RDS t3.small = $30/month
- Load Balancer: ALB = $20/month
- Data Transfer: ~$10/month

**Total**: ~$122/month

**ROI**: For a team of 20+ users querying tags regularly, saves $200-500/month in cloud API costs through caching. Pays for itself in reduced cost allocation manual effort.

---

## Success Metrics

- **Compliance Rate Improvement**: Target 80%+ compliance within 6 months
- **Cost Attribution Gap Reduction**: Target <10% unallocated spend
- **Time to Remediate Violations**: <1 week for bulk fixes (vs months manually)
- **User Adoption**: 50%+ of FinOps team using monthly within 3 months
- **Audit Pass Rate**: 100% compliance audits passed

---

## Implementation Roadmap

### Phase 1: MVP (Months 1-2)
- Core tools: `check_tag_compliance`, `find_untagged_resources`, `suggest_tags`
- Single cloud support (AWS only)
- Basic tagging policy validation
- Simple compliance reports

### Phase 2: Multi-Cloud (Months 3-4)
- Add Azure and GCP support
- Cross-cloud consistency checking
- Cost attribution gap analysis
- ML-powered tag suggestions

### Phase 3: Automation (Months 5-6)
- Bulk tagging with approval workflows
- Step-up authorization integration
- Scheduled compliance audits
- Executive dashboards

### Phase 4: Enterprise (Months 7-12)
- RBAC for multi-team deployments
- Custom tag schema per team
- Slack/Teams integration for approvals
- Advanced ML models for suggestions
- Anomaly detection (tag drift alerts)

---

## Community and Support

**Repository**: https://github.com/OptimNow/finops-tag-compliance-mcp
**Documentation**: https://docs.finops-tag-compliance-mcp.io
**Issues**: GitHub Issues
**Slack**: #finops-mcp-tag-compliance (FinOps Foundation Slack)

---

## License

Apache 2.0

---

## Contributors

- Jean Latiere (FinOps Foundation, OptimNow)
- [Your Name] (If building this)
- Community contributors welcome

---

## Changelog

**v1.0** (December 2024)
- Initial specification
- 15 tools, 5 resources, 3 prompts defined
- Multi-cloud support (AWS, Azure, GCP)
- Step-up authorization model
- Cost attribution gap analysis
