# 🔐 AWS MCP Servers — Least-Privilege IAM Policy (Read-Only)

Create a **dedicated IAM user or role** for MCP clients and attach the policy below.  
Scope access only to the accounts/roles you truly need. All actions are **read-only**.

> Covers:
> - AWS **Pricing MCP** (Price List API)
> - **Cost Explorer MCP**
> - **CloudWatch MCP** (metrics/alarms/logs read)
> - **Billing & Cost Management MCP**
> - **CFM Tips MCP** (Cost Optimization Hub, Compute Optimizer, TA, PI, CUR listing)

---

## ✅ Single Merged Policy: `AWS-MCP-ReadOnly`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PricingRead",
      "Effect": "Allow",
      "Action": [
        "pricing:GetProducts"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CostExplorerRead",
      "Effect": "Allow",
      "Action": [
        "ce:GetCostAndUsage",
        "ce:GetCostAndUsageWithResources",
        "ce:GetDimensionValues",
        "ce:GetTags",
        "ce:GetCostForecast",
        "ce:GetCostAndUsageComparisons",
        "ce:GetCostComparisonDrivers"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchRead",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:GetMetricData",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "cloudwatch:DescribeAlarms",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:GetLogEvents"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CfmTipsRead",
      "Effect": "Allow",
      "Action": [
        "cost-optimization-hub:ListEnrollmentStatuses",
        "cost-optimization-hub:ListRecommendations",
        "cost-optimization-hub:GetRecommendation",
        "cost-optimization-hub:ListRecommendationSummaries",
        "compute-optimizer:GetEC2InstanceRecommendations",
        "compute-optimizer:GetEBSVolumeRecommendations",
        "compute-optimizer:GetLambdaFunctionRecommendations",
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "rds:DescribeDBInstances",
        "lambda:ListFunctions",
        "cloudwatch:GetMetricStatistics",
        "s3:ListBucket",
        "s3:ListObjectsV2",
        "support:DescribeTrustedAdvisorChecks",
        "support:DescribeTrustedAdvisorCheckResult",
        "pi:GetResourceMetrics"
      ],
      "Resource": "*"
    }
  ]
}

📝 Notes

Billing & Cost Management MCP: this policy uses read APIs (Cost Explorer, CUR, etc.) rather than console-only aws-portal:* actions, which are less portable and only work for IAM users in the payer account.

CFM Tips MCP: requires additional service Describe* calls (EC2, RDS, Lambda, DynamoDB, etc.) to fetch optimization data.

CUR files: CFM Tips lists s3:List* permissions. If your runbooks actually read CUR objects, add:📝 Notes

Billing & Cost Management MCP: this policy uses read APIs (Cost Explorer, CUR, etc.) rather than console-only aws-portal:* actions, which are less portable and only work for IAM users in the payer account.

CFM Tips MCP: requires additional service Describe* calls (EC2, RDS, Lambda, DynamoDB, etc.) to fetch optimization data.

CUR files: CFM Tips lists s3:List* permissions. If your runbooks actually read CUR objects, add:

{
  "Effect": "Allow",
  "Action": [ "s3:GetObject" ],
  "Resource": "arn:aws:s3:::<your-cur-bucket>/*"
}

For multi-account setups, prefer assuming a read-only IAM role in each linked account rather than widening permissions in a single account.

🚀 How to use

Go to IAM Console → Policies → Create policy.

Choose JSON, paste the policy above.

Name it AWS-MCP-ReadOnly and save.

Attach the policy to a dedicated IAM user or role used by your MCP client (e.g. for AWS_PROFILE in your mcp.json).

If you use Cost and Usage Reports (CUR) stored in S3, don’t forget to extend the policy with s3:GetObject on the CUR bucket.


---

👉 This file is now fully self-contained: it has the merged JSON policy, the notes, and clear usage instructions.  

Do you also want me to prepare a **stricter variant** (scoping `Resource` to specific ARNs like `arn:aws:cloudwatch:region:account:alarm/*` or your CUR bucket ARN), or do you prefer to keep it broad (`Resource: "*"`) for now?

