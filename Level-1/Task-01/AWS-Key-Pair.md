# Terraform Task – Create AWS Key Pair (datacenter-kp)

The Nautilus DevOps team is migrating infrastructure to AWS in incremental phases.  
For this task, we need to create an AWS key pair using Terraform with the following requirements:


## 📌 Objective

Create an AWS key pair using Terraform

- Key name: `datacenter-kp`
- Key type: `RSA`
- Private key saved at: `/home/bob/datacenter-kp.pem`
- Terraform working directory: `/home/bob/terraform`
- All configuration inside `main.tf`

---

## 🛠️ Solution

### main.tf

```hcl

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
```
🔍 Explanation

1️⃣ TLS Private Key Resource
Generates an RSA 2048-bit key pair.

2️⃣ AWS Key Pair Resource
Uploads the generated public key to AWS as:
`datacenter-kp`

3️⃣ Local File Resource
Stores the private key securely at:
```
/home/bob/datacenter-kp.pem
```
Permission set to `0400` for SSH security.

🚀 Execution Steps

```bash
cd /home/bob/terraform
terraform init
terraform plan
terraform apply
```

✅ Verification

```ls -l /home/bob/datacenter-kp.pem```

Expected permission:

```-r--------```


🎯 Outcome

✔ RSA key pair generated using Terraform

✔ Public key uploaded to AWS

✔ Private key securely stored locally

✔ Fully automated Infrastructure as Code implementation
