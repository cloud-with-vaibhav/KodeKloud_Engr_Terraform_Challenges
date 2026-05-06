# Terraform Task – Create AMI from EC2 Instance (datacenter-ec2-ami)

## 📘 Task Description

The Nautilus DevOps team is creating reusable machine images as part of their AWS migration strategy.

For this task, the objective is to create an **AMI (Amazon Machine Image)** from an existing EC2 instance using Terraform with the following requirements:

- **Source Instance Name:** `datacenter-ec2`
- **AMI Name:** `datacenter-ec2-ami`
- **AMI State:** Must be in `available` state
- **Region:** `us-east-1`
- **Terraform Working Directory:** `/home/bob/terraform`
- Update configuration inside **main.tf** only (do not create a new file)

---

## 🛠️ Terraform Solution

Update existing main.tf with below resource

### 📄 main.tf

```hcl
# Existing EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  vpc_security_group_ids = [
    "sg-226178779315e5962"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

# Create AMI from EC2 instance
resource "aws_ami_from_instance" "datacenter_ami" {
  name               = "datacenter-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

AMI Creation from Instance

```hcl
resource "aws_ami_from_instance" "datacenter_ami"
```
Creates an AMI from the EC2 instance


Uses:
```hcl
source_instance_id = aws_instance.ec2.id
```

Dependency Handling

Terraform automatically ensures:

- EC2 instance is created first
- AMI creation happens after

This is handled implicitly through:

```bash
aws_instance.ec2.id
```

No need to define `depends_on`.

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

Expected:
- EC2 instance (if not already created)
- AMI creation
  
Step 5: Apply Configuration

`terraform apply`

Type yes when prompted.
