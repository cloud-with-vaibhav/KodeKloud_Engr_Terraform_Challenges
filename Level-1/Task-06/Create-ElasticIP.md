# Terraform Task – Allocate Elastic IP (nautilus-eip)

## 📘 Task Description

The Nautilus DevOps team is continuing their AWS migration using incremental steps.

For this task, the objective is to allocate an **Elastic IP (EIP)** using Terraform with the following requirements:

- **Elastic IP Name:** `nautilus-eip`
- **Region:** `us-east-1`
- **Terraform Working Directory:** `/home/bob/terraform`
- All Terraform configuration must be inside **main.tf**

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl
resource "aws_eip" "nautilus_eip" {
  domain = "vpc"

  tags = {
    Name = "nautilus-eip"
  }
}
```
🚀 Execution Steps

Step 1: Navigate to Terraform Directory

`cd /home/bob/terraform`

Step 2: Initialize Terraform

`terraform init`

Step 3: Validate Configuration (Recommended)

`terraform validate`

Step 4: Review Execution Plan

`terraform plan`

Expected:
1 Elastic IP will be allocated

Step 5: Apply Configuration

`terraform apply`
Type yes when prompted.


💡 DevOps Best Practice Insight

- Always use domain = "vpc" for EIPs
- Tag resources for better visibility and management
- Avoid unused Elastic IPs to reduce AWS costs
- Manage infrastructure via Terraform for consistency and automation
