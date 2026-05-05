# Terraform Task – Create EC2 Instance (datacenter-ec2)

## 📘 Task Description

The Nautilus DevOps team is provisioning compute resources in AWS as part of their migration.

For this task, the objective is to create an **EC2 instance using Terraform** with the following requirements:

- **Instance Name (Tag):** `datacenter-ec2`
- **AMI ID:** `ami-0c101f26f147fa7fd` (Amazon Linux)
- **Instance Type:** `t2.micro`
- **Key Pair Name:** `datacenter-kp` (RSA)
- **Security Group:** Default security group
- **Terraform Working Directory:** `/home/bob/terraform`
- All configuration must be inside **main.tf**

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

data "aws_security_group" "default_sg" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}

resource "tls_private_key" "datacenter_key" {
  algorithm = "RSA"
  rsa_bits  = 2048
}

resource "aws_key_pair" "datacenter_kp" {
  key_name   = "datacenter-kp"
  public_key = tls_private_key.datacenter_key.public_key_openssh
}

resource "local_file" "private_key" {
  content         = tls_private_key.datacenter_key.private_key_pem
  filename        = "/home/bob/datacenter-kp.pem"
  file_permission = "0400"
}

resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  key_name = aws_key_pair.datacenter_kp.key_name

  vpc_security_group_ids = [data.aws_security_group.default_sg.id]

  tags = {
    Name = "datacenter-ec2"
  }
}
```
---

🔍 Explanation of the Configuration

1️⃣ AWS Provider
```hcl
provider "aws" {
  region = "us-east-1"
}
```
Defines AWS as the cloud provider
Ensures all resources are created in us-east-1


2️⃣ Fetch Default VPC
```hcl
data "aws_vpc" "default" {
  default = true
}
```

Retrieves the default VPC dynamically
Avoids hardcoding VPC ID

3️⃣ Fetch Default Security Group
```hcl
data "aws_security_group" "default_sg" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}
```
Fetches the default security group within the default VPC
Required to attach EC2 instance to default SG

4️⃣ Generate RSA Private Key
```hcl
resource "tls_private_key" "datacenter_key"
```

Generates a 2048-bit RSA key pair
Used for SSH access to EC2 instance

5️⃣ Create AWS Key Pair
```hcl
resource "aws_key_pair" "datacenter_kp"
```
Uploads public key to AWS
Creates key pair named:
datacenter-kp

6️⃣ Store Private Key Locally
```hcl
resource "local_file" "private_key"
```
Saves private key to:

`/home/bob/datacenter-kp.pem`
Sets permission to: `0400`

Ensures secure SSH usage.

7️⃣ EC2 Instance Creation
```hcl
resource "aws_instance" "datacenter_ec2"
```

Creates an EC2 instance with:

AMI: Amazon Linux (ami-0c101f26f147fa7fd)
Instance Type: t2.micro
Key Pair: datacenter-kp
Security Group: Default security group

Tag applied:

Name = datacenter-ec2

---

🚀 Execution Steps

Step 1: Navigate to Terraform Directory

`cd /home/bob/terraform`

Step 2: Initialize Terraform

`terraform init`

Step 3: Validate Configuration

`terraform validate`

Step 4: Review Plan

`terraform plan`

Step 5: Apply Configuration

`terraform apply`

Type yes when prompted.


💡 DevOps Best Practice Insight

- Use data sources to fetch existing resources
- Avoid manual key creation
- Secure private keys properly
- Use tagging for better visibility
- Maintain infrastructure as code for consistency
