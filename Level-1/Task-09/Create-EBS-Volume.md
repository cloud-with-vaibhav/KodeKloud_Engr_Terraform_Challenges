# Terraform Task – Create EBS Volume (datacenter-volume)

## 📘 Task Description

The Nautilus DevOps team is provisioning storage resources in AWS as part of their infrastructure setup.

For this task, the objective is to create an **EBS (Elastic Block Store) volume** using Terraform with the following requirements:

- **Volume Name:** `datacenter-volume`
- **Volume Type:** `gp3`
- **Volume Size:** `2 GiB`
- **Region:** `us-east-1`
- **Terraform Working Directory:** `/home/bob/terraform`
- All configuration must be inside **main.tf**

---

## 🛠️ Terraform Solution

### 📄 main.tf

```hcl

resource "aws_ebs_volume" "datacenter_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"

  tags = {
    Name = "datacenter-volume"
  }
}
```

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


🎯 Final Outcome

✔ EBS volume datacenter-volume created

✔ Volume type gp3 configured

✔ Size set to 2 GiB

✔ Deployed in us-east-1

✔ Managed using Terraform (IaC)
