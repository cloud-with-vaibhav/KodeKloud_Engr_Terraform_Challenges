# Terraform Task – Create EBS Snapshot (devops-vol-ss)

## 📘 Task Description

The Nautilus DevOps team is implementing backup and recovery mechanisms for AWS storage resources.

For this task, the objective is to create a **snapshot of an existing EBS volume** using Terraform with the following requirements:

- **Existing Volume Name:** `devops-vol`
- **Snapshot Name:** `devops-vol-ss`
- **Description:** `Devops Snapshot`
- **Snapshot Status:** Must reach `completed` state
- **Region:** `us-east-1`
- **Terraform Working Directory:** `/home/bob/terraform`
- Update configuration inside **main.tf** only

---

## 🛠️ Terraform Solution

### Update the existing 📄 main.tf

```hcl
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name = "devops-vol"
  }
}

resource "aws_ebs_snapshot" "devops_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Devops Snapshot"

  tags = {
    Name = "devops-vol-ss"
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

