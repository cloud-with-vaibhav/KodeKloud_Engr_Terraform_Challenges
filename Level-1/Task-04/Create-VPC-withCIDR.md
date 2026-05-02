# Terraform Task – Create VPC (xfusion-vpc)

## 📘 Task Description

The Nautilus DevOps team continues its AWS migration in incremental steps.

For this task, the objective is to create a **VPC using Terraform** with the following requirements:

- **VPC Name:** `xfusion-vpc`
- **Region:** `us-east-1`
- **IPv4 CIDR Block:** `192.168.0.0/24`
- **Terraform Working Directory:** `/home/bob/terraform`
- All Terraform configuration must be inside **main.tf**

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "xfusion-vpc"
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
1 VPC resource will be created

Step 5: Apply Configuration
`terrafrom apply`
