# Terraform Level-1 Task-13: Create Private S3 Bucket Using Terraform

## Task Description

The Nautilus DevOps team is creating S3 buckets on AWS as part of data migration. They need a private S3 bucket that blocks all public access.

**Requirements:**
- Create an S3 bucket named `datacenter-s3-24360`
- Block **all** public access (private bucket)
- Working directory: `/home/bob/terraform`
- File: `main.tf`

---

## Solution

### main.tf

```hcl
resource "aws_s3_bucket" "datacenter_bucket" {
  bucket = "datacenter-s3-24360"
}

resource "aws_s3_bucket_public_access_block" "datacenter_bucket_public_access" {
  bucket = aws_s3_bucket.datacenter_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

---

## Steps to Execute

```bash
cd /home/bob/terraform
cat > main.tf <<'EOF'
resource "aws_s3_bucket" "datacenter_bucket" {
  bucket = "datacenter-s3-24360"
}

resource "aws_s3_bucket_public_access_block" "datacenter_bucket_public_access" {
  bucket = aws_s3_bucket.datacenter_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
EOF

terraform init
terraform plan
terraform apply -auto-approve
```

---

## Explanation

| Resource | Purpose |
|----------|---------|
| `aws_s3_bucket` | Creates the S3 bucket with the specified name |
| `aws_s3_bucket_public_access_block` | Blocks all public access to make it fully private |

### Public Access Block Settings

| Setting | Value | Effect |
|---------|-------|--------|
| `block_public_acls` | `true` | Rejects any PUT requests that include a public ACL |
| `block_public_policy` | `true` | Rejects any bucket policy that grants public access |
| `ignore_public_acls` | `true` | Ignores any existing public ACLs on the bucket |
| `restrict_public_buckets` | `true` | Restricts access to the bucket to only AWS services and authorized users |

---

## Tricks & Notes

1. **Much simpler than public buckets** — Making a bucket private only requires **2 resources** (bucket + public access block), while public buckets need 4. All four block settings set to `true` = fully locked down.

2. **S3 buckets are private by default** — Even without the `aws_s3_bucket_public_access_block`, a new S3 bucket is private. However, explicitly adding the block ensures **no one can accidentally make it public later** (belt and suspenders approach).

3. **Why explicit block matters** — Without the public access block:
   - Someone could add a public bucket policy later
   - Someone could set a public ACL on an object
   - The block acts as a **guardrail** against human error

4. **No ACL resource needed** — Since we're keeping it private, we don't need `aws_s3_bucket_acl` or `aws_s3_bucket_ownership_controls`. The default behavior (private, no ACL) is exactly what we want.

5. **Comparison with Task-12 (Public Bucket):**

   | Aspect | Public (Task-12) | Private (Task-13) |
   |--------|------------------|-------------------|
   | Resources needed | 4 | 2 |
   | Public access block | All `false` | All `true` |
   | ACL | `public-read` | Not needed |
   | Ownership controls | `BucketOwnerPreferred` | Not needed |

6. **AWS Best Practice** — AWS recommends enabling all four public access block settings for any bucket that doesn't need public access. This is also enforced by many compliance frameworks (SOC2, HIPAA, etc.).

---

## Verify

```bash
terraform state list
# Expected:
# aws_s3_bucket.datacenter_bucket
# aws_s3_bucket_public_access_block.datacenter_bucket_public_access

terraform state show aws_s3_bucket.datacenter_bucket
terraform state show aws_s3_bucket_public_access_block.datacenter_bucket_public_access
```
