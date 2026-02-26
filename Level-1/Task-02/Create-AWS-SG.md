# Terraform Task – Create Security Group (datacenter-sg)

## 📘 Task Description

The Nautilus DevOps team is migrating infrastructure to AWS in incremental phases.

For this task, we need to create a **Security Group** in the **default VPC** using Terraform with the following requirements:

- **Security Group Name:** `datacenter-sg`
- **Description:** Security group for Nautilus App Servers
- **Region:** us-east-1
- **Inbound Rules:**
  - HTTP → Port 80 → 0.0.0.0/0
  - SSH → Port 22 → 0.0.0.0/0
- Terraform working directory: `/home/bob/terraform`
- All configuration must be inside `main.tf`

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl

# Fetch Default VPC
data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"] #
  }
}
```
🔍 Explanation of the Configuration

1️⃣ AWS Provider
```hcl
provider "aws" {
  region = "us-east-1"
}
```
- Specifies AWS as provider
- Sets region to us-east-1 as required

2️⃣ Fetch Default VPC
```hcl
data "aws_vpc" "default" {
  default = true
}
```
- Retrieves the default VPC ID dynamically
- Avoids hardcoding VPC ID
- Ensures security group is created inside default VPC

3️⃣ Security Group Resource
```hcl
resource "aws_security_group" "datacenter_sg"
```

Creates a security group with:
- Name: datacenter-sg
- Description: Security group for Nautilus App Servers
- Attached to default VPC

4️⃣ Inbound Rules (Ingress)
HTTP Rule
- Port: 80
- Protocol: TCP
- Source: 0.0.0.0/0 (public internet)

SSH Rule
- Port: 22
- Protocol: TCP
- Source: 0.0.0.0/0 (public internet)

5️⃣ Outbound Rule (Egress)
```hcl
egress {
  from_port   = 0
  to_port     = 0
  protocol    = "-1"
  cidr_blocks = ["0.0.0.0/0"]
}
```

- Allows all outbound traffic
- This is AWS default behavior, but explicitly defining it is good practice



🚀 Execution Steps

Step :Navigate to Terraform Directory

```bash
cd /home/bob/terraform

#Initialize Terraform
terraform init

#Validate Configuration (Recommended)
terraform validate

#Review Plan
terraform plan

#Apply configuration
terraform apply
```

---



🎯 Final Outcome

✔ Security group created in default VPC

✔ HTTP access allowed from anywhere

✔ SSH access allowed from anywhere

✔ Infrastructure managed using Terraform

✔ No manual AWS Console configuration

---

💡 DevOps Best Practice Insight

- Always use data sources to fetch existing infrastructure (like default VPC)

- Avoid hardcoding resource IDs

- Use Infrastructure as Code for repeatability

- Define egress rules explicitly for clarity

----

🏷️ Tags

@Terraform AWS Security Group Infrastructure as Code DevOps KodeKloud Engineer
