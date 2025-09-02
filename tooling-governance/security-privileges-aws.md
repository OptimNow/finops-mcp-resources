# Creating and Configuring AWS Access Keys

## Creating Access Keys for a User

**Before configuring AWS CLI, you need access keys for your user:**

### Step 1: Create access keys in AWS Console
- Log into AWS Console
- Go to IAM → Users → [Your Username]
- Click "Security credentials" tab
- Click "Create access key"
- Choose "Command Line Interface (CLI)"
- Copy the Access Key ID and Secret Access Key

### Step 2: Configure your user
```powershell
aws configure
```
When prompted, enter:
- AWS Access Key ID: Paste your access key
- AWS Secret Access Key: Paste your secret key
- Default region name: Your preferred region (e.g., us-east-1)
- Default output format: json

Why region and output format matter:
- Region: MCP queries will default to this region for pricing
- Output format: "json" works best with MCP tools
The keys are stored encrypted in the credentials file and won't show in plain text when checking configuration.


#  🔐 Using a Dedicated User for MCP (Recommended)

# Create the user and generate its keys

For better security, create a **dedicated AWS user** instead of using your personal credentials. First, check your current configuration with ```aws sts get-caller-identity``` to see which user is currently configured. Then create a new IAM user specifically for MCP operations, attach the pricing policy ( defined below), generate access keys for this user, and configure it using ```aws configure --<enter the name of the MCP user created>```. This approach limits permissions to only what the MCP server needs and keeps your personal credentials separate from automated tools.


# AWS MCP Servers — Least-Privilege IAM Policy (Read-Only)

 
Scope access only to the accounts/roles you truly need. All actions are **read-only**.

> Covers:
> - AWS **Pricing MCP** (Price List API)
> - **Cost Explorer MCP**
> - **CloudWatch MCP** (metrics/alarms/logs read)
> - **Billing & Cost Management MCP**
> - **CFM Tips MCP** (Cost Optimization Hub, Compute Optimizer, TA, PI, CUR listing)

Attach to the newly created user the policy below. 
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
```


## 🚀 How to use

1. Go to **IAM Console → Policies → Create policy**.  
2. Choose **JSON**, paste the policy above.  
3. Name it **`AWS-MCP-ReadOnly`** and save.  
4. Attach the policy to a **dedicated IAM user or role** used by your MCP client (e.g. for `AWS_PROFILE` in your `mcp.json`).  
5. If you use **Cost and Usage Reports (CUR)** stored in S3, extend the policy with:  
   ```json
   {
     "Effect": "Allow",
     "Action": [ "s3:GetObject" ],
     "Resource": "arn:aws:s3:::<your-cur-bucket>/*"
   }


## 📝 Notes

- **Billing & Cost Management MCP**: this policy uses **read APIs** (Cost Explorer, CUR, etc.) rather than console-only `aws-portal:*` actions, which are less portable and only work for IAM users in the payer account.  
- **CFM Tips MCP**: requires additional service `Describe*` calls (EC2, RDS, Lambda, DynamoDB, etc.) to fetch optimization data.  
- **CUR files**:  
  If your workflows need to **read** CUR objects from S3, add:  
  ```json
  {
    "Effect": "Allow",
    "Action": [ "s3:GetObject" ],
    "Resource": "arn:aws:s3:::<your-cur-bucket>/*"
  }

- For **multi-account setups**, prefer assuming a **read-only IAM role** in each linked account rather than widening permissions in a single account.
