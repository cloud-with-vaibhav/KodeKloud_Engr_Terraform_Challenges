# Terraform Task – Create IPv6 Enabled VPC (xfusion-vpc)

## 📘 Task Description

The Nautilus DevOps team is continuing their AWS migration in incremental steps.

For this task, the objective is to create a **VPC using Terraform** with the following requirements:

- **VPC Name:** `xfusion-vpc`
- **Region:** `us-east-1`
- **IPv6 Requirement:** Use Amazon-provided IPv6 CIDR block
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
   cidr_block                       = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true

  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "xfusion-vpc"
  }
}
```

Enable Amazon-Provided IPv6 CIDR
```hcl
assign_generated_ipv6_cidr_block = true
```
- Requests AWS to assign an IPv6 CIDR block automatically
- AWS provides a globally unique IPv6 range
- No need to manually define IPv6 CIDR

Even when using:

`assign_generated_ipv6_cidr_block = true`

You must still define an IPv4 CIDR block, because:

- AWS VPCs are IPv4-first constructs
- IPv6 is an additional (optional) layer, not a replacement

✔ Optional but Recommended
```hcl
enable_dns_support   = true
enable_dns_hostnames = true
```
These help with:

- DNS resolution inside VPC
- Public hostname assignment (important for EC2)

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
IPv6 CIDR block will be assigned

Step 5: Apply Configuration

`terraform apply`

⚠️ Key Learning (Important for Interviews)

If asked:

Can we create a VPC with only IPv6?

👉 Answer:

No.

AWS requires an IPv4 CIDR block
IPv6 is always added on top of IPv4, not standalone
