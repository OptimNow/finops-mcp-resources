# CUR-Anon: Secure AWS Cost Data Sharing for AI Analysis 🔒📊

**Source**: [GitHub - fcontrepois/cur-anon](https://github.com/fcontrepois/cur-anon/tree/main)  
**Status**: ✅ Open Source (MIT License)  
**Author**: Frank Contrepois

## The Critical FinOps Privacy Problem 🚨

** Sharing raw CUR data with public LLMs is a **massive security risk**:

- 💳 **Account IDs exposed** - Direct path to your AWS infrastructure
- 🏢 **Resource names reveal** business structure, project names, internal systems
- 📊 **Cost patterns show** competitive intelligence, business priorities
- 🔑 **ARNs contain** sensitive resource information and organizational structure
- 📈 **Usage data reveals** technical architecture and scaling patterns

**Never feed raw billing data to public AI models.**

## The CUR-Anon Solution ⚡

**Smart anonymization that preserves analytical value while removing sensitive data.**

### **What It Does**
```
❌ Raw CUR: account-id: 123456789012, arn: arn:aws:s3:::my-secret-bucket
✅ Anonymized: account-id: 456789123456, arn: arn:aws:s3:::resource-a8f9c2d1
```

### **Key Features**
- ✅ **Consistent anonymization** - Same account ID → Same fake ID across dataset
- ✅ **Preserves relationships** - Maintains data structure for analysis
- ✅ **Multiple formats** - CUR2, Legacy CUR, Focus (Azure) support  
- ✅ **Flexible actions** - Keep, remove, hash, or anonymize per column
- ✅ **No dependencies** - DuckDB only, no Spark/Java complexity

## Anonymization Options 🛠️

| Action | Use Case | Example |
|--------|----------|---------|
| `keep` | Safe data (regions, instance types) | `us-east-1` stays `us-east-1` |
| `remove` | Sensitive columns you don't need | Drops column entirely |
| `awsid_anonymise` | Account IDs | `123456789012` → `456789123456` |
| `awsarn_anonymise` | Resource ARNs | Rebuilds with fake account IDs |
| `hash` | Tags, resource names | `prod-webapp` → `a8f9c2d1e5b7` |
| `uuid` | Unique identifiers | Deterministic UUID replacement |

## Quick Implementation 🚀

### **Step 1: Install**
```bash
git clone https://github.com/fcontrepois/cur-anon.git
cd cur-anon
pip install -r requirements.txt
```

### **Step 2: Generate Config**
```bash
# For CUR2 (modern format)
python python/cur2anonymiser.py --input rawcur.parquet --create-config --config config.json

# For Legacy CUR  
python python/curanonymiser_legacy.py --input rawcur.parquet --create-config --config config.json
```

### **Step 3: Customize Config**
```json
{
  "_comment": "Column options: 'keep', 'remove', 'awsid_anonymise', 'awsarn_anonymise', 'hash', 'uuid'",
  "columns": {
    "line_item_usage_account_id": "awsid_anonymise",
    "bill_payer_account_id": "awsid_anonymise", 
    "line_item_resource_id": "awsarn_anonymise",
    "product_instance_type": "keep",
    "product_region": "keep",
    "resource_tags": "hash",
    "line_item_line_item_description": "remove"
  }
}
```

### **Step 4: Anonymize**
```bash
# Generate anonymized Parquet
python python/cur2anonymiser.py --input rawcur.parquet --output safe-cur.parquet --config config.json

# Or CSV for easier sharing
python python/cur2anonymiser.py --input rawcur.parquet --output safe-cur.csv --config config.json
```

## FinOps Use Cases 💡

### **1. Safe AI Analysis** 🤖
- **Share with public LLMs** - Claude, ChatGPT, Gemini for cost optimization
- **External consultants** - Provide anonymized data for analysis
- **Cross-team collaboration** - Share cost data without exposing sensitive details

### **2. Compliance & Governance** 🛡️
- **Regulatory requirements** - Remove PII/sensitive data before analysis
- **Audit trails** - Maintain data utility while meeting compliance
- **Third-party tools** - Use cost optimization SaaS without data exposure

### **3. Training & Testing** 📚
- **Team training** - Use real data patterns without real accounts
- **Tool testing** - Validate FinOps tools with realistic but safe data
- **Demo environments** - Show cost analysis without revealing real infrastructure

### **4. Multi-Cloud Analysis** 🌐
- **Azure Focus format** support for cross-cloud anonymization
- **Consistent methodology** across cloud providers
- **Unified privacy approach** for multi-cloud FinOps

## Why Anonymization Matters for FinOps 🎯

### **Business Intelligence Protection**
Your CUR data reveals:
- **Spending priorities** → Competitive intelligence
- **Architecture patterns** → Technical approach
- **Growth trajectories** → Business forecasts  
- **Resource naming** → Internal organization structure

### **Security Risks of Raw Data Sharing**
- **Account takeover** attempts using exposed account IDs
- **Social engineering** attacks using resource naming patterns
- **Vendor lock-in** risks from detailed usage analysis
- **Regulatory violations** in sensitive industries

### **AI/LLM Specific Risks**
- **Data retention** by LLM providers in training datasets
- **Inadvertent exposure** through LLM responses to other users
- **Model poisoning** potential with sensitive business data
- **Compliance violations** when using public AI services

## Implementation Best Practices 📋

### **Config Strategy**
```json
// Recommended FinOps anonymization config
{
  "columns": {
    // Always anonymize identifiers
    "line_item_usage_account_id": "awsid_anonymise",
    "bill_payer_account_id": "awsid_anonymise",
    "line_item_resource_id": "awsarn_anonymise",
    
    // Keep analytical data
    "product_region": "keep",
    "product_instance_type": "keep", 
    "line_item_usage_type": "keep",
    "line_item_blended_cost": "keep",
    "line_item_usage_amount": "keep",
    
    // Hash sensitive metadata
    "resource_tags": "hash",
    "line_item_line_item_description": "hash",
    
    // Remove unnecessary sensitive data
    "line_item_availability_zone": "remove",
    "product_instance_type_family": "keep"
  }
}
```

### **Quality Validation**
```bash
# Check anonymized output
duckdb -c "SELECT DISTINCT line_item_usage_account_id FROM 'safe-cur.parquet' LIMIT 10;"

# Verify cost totals match (should be identical)
duckdb -c "SELECT SUM(line_item_blended_cost) FROM 'safe-cur.parquet';"
```

### **Integration with MCP Workflows**
1. **Anonymize CUR data** using cur-anon
2. **Upload to secure location** (S3, local files)
3. **Use anonymized data** with MCP servers for AI analysis
4. **Share results safely** without exposing sensitive infrastructure details

## Advanced Use Cases 🔬

### **Benchmarking & Industry Comparison**
- **Share with peers** for industry benchmarking
- **Vendor negotiations** using anonymized usage patterns
- **Cost optimization consulting** without revealing competitive data

### **Academic Research**
- **Cloud cost research** using real but anonymized datasets
- **FinOps methodology** development and validation
- **Tool development** and testing with realistic data

## Limitations & Considerations ⚠️

### **Security Notes**
- **Not cryptographically secure** - Uses MD5 hashing for anonymization
- **Consistent but not reversible** - Same input → same output
- **Data relationships preserved** - Maintains analytical value
- **Consider additional controls** for highly sensitive environments

### **Data Utility**
- **Cost totals unchanged** - Perfect for spend analysis
- **Usage patterns preserved** - Right-sizing analysis still valid
- **Time series intact** - Trend analysis unaffected
- **Regional data maintained** - Geographic analysis possible

## Bottom Line for FinOps Teams 💼

**Anonymization is essential for modern AI-powered FinOps.**

### **Without cur-anon:**
❌ Can't use public LLMs safely  
❌ Limited collaboration with external parties  
❌ Compliance risks with sensitive data  
❌ Reduced innovation due to data sharing constraints  

### **With cur-anon:**
✅ Safe AI-powered cost analysis  
✅ Secure collaboration and consulting  
✅ Compliance-friendly data sharing  
✅ Innovation without security compromise  

---

*Perfect complement to MCP servers - anonymize first, analyze safely with AI.*


