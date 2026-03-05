# Terraform Task – Create VPC (datacenter-vpc)

## 📘 Task Description

The Nautilus DevOps team is migrating infrastructure to AWS in incremental phases.  
For this task, the objective is to create a **VPC using Terraform** with the following requirements:

- **VPC Name:** `datacenter-vpc`
- **Region:** `us-east-1`
- **IPv4 CIDR Block:** Any valid CIDR block
- **Terraform Working Directory:** `/home/bob/terraform`
- All Terraform configuration must be inside **main.tf**

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl


resource "aws_vpc" "datacenter_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}
```
🔍 Explanation of the Configuration

1️⃣ AWS Provider

The `provider.tf` file already has region defined so no need to specify it again in the `main.tf` file


2️⃣ VPC Resource
resource "aws_vpc" "datacenter_vpc"

This block creates a new Virtual Private Cloud (VPC).

Parameters used:
cidr_block
`10.0.0.0/16`

Defines the IP range available inside the VPC.
This range provides 65,536 private IP addresses, which is common for production environments.


3️⃣ Tags
```hcl
tags = {
  Name = "datacenter-vpc"
}
```

Tags help identify resources easily in the AWS Console.

The VPC will appear in AWS as:
`datacenter-vpc`


🚀 Execution Steps

Step 1: Navigate to Terraform Directory

```bash
cd /home/bob/terraform
```

Step 2: Initialize Terraform
```bash
terraform init
```
This downloads required providers and prepares the Terraform environment.

Step 3: Validate Terraform Configuration
```bash
terraform validate
```
This checks syntax and configuration correctness.

Step 4: Review Execution Plan
```bash
terraform plan
```
Expected output:

Terraform will create 1 VPC resource.

Step 5: Apply Configuration
```bash
terraform apply
```
Type yes when prompted.

Terraform will create the VPC in AWS.

---

🎯 Final Outcome

✔ A new VPC named datacenter-vpc is created

✔ VPC deployed in us-east-1 region

✔ CIDR block allocated for internal networking

✔ Infrastructure managed using Terraform (IaC)

