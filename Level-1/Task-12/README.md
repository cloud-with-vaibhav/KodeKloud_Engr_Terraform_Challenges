# Terraform Level-1 Task-12: Create Public S3 Bucket Using Terraform

## Task Description

The Nautilus DevOps team is creating S3 buckets on AWS as part of data migration. They need both private and public buckets for storing relevant data.

**Requirements:**
- Create a public S3 bucket named `nautilus-s3-28386`
- Ensure the bucket is publicly accessible using proper ACL
- Region: `us-east-1`
- Working directory: `/home/bob/terraform`
- File: `main.tf`

---

## Solution

### main.tf

```hcl
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-28386"
}

resource "aws_s3_bucket_ownership_controls" "nautilus_bucket_ownership" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

resource "aws_s3_bucket_public_access_block" "nautilus_bucket_public_access" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "nautilus_bucket_acl" {
  depends_on = [
    aws_s3_bucket_ownership_controls.nautilus_bucket_ownership,
    aws_s3_bucket_public_access_block.nautilus_bucket_public_access,
  ]

  bucket = aws_s3_bucket.nautilus_bucket.id
  acl    = "public-read"
}
```

---

## Steps to Execute

```bash
cd /home/bob/terraform
cat > main.tf <<'EOF'
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-28386"
}

resource "aws_s3_bucket_ownership_controls" "nautilus_bucket_ownership" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

resource "aws_s3_bucket_public_access_block" "nautilus_bucket_public_access" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "nautilus_bucket_acl" {
  depends_on = [
    aws_s3_bucket_ownership_controls.nautilus_bucket_ownership,
    aws_s3_bucket_public_access_block.nautilus_bucket_public_access,
  ]

  bucket = aws_s3_bucket.nautilus_bucket.id
  acl    = "public-read"
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
| `aws_s3_bucket_ownership_controls` | Enables ACLs by setting ownership to `BucketOwnerPreferred` |
| `aws_s3_bucket_public_access_block` | Disables all public access blocking (required for public ACL) |
| `aws_s3_bucket_acl` | Sets `public-read` ACL to make bucket publicly accessible |

---

## Tricks & Notes

1. **Why 4 resources instead of just 1?** — In AWS provider v4+, S3 bucket ACLs, public access settings, and ownership controls are all **separate resources**. The old `acl` argument directly inside `aws_s3_bucket` is deprecated.

2. **Order matters — use `depends_on`** — The ACL resource **must** wait for both ownership controls and public access block to be created first. Without `depends_on`, Terraform may try to set the ACL before public access is allowed, causing an `AccessControlListNotSupported` error.

3. **`BucketOwnerPreferred` is required** — By default, AWS uses `BucketOwnerEnforced` which **disables ACLs entirely**. You must set it to `BucketOwnerPreferred` before applying any ACL.

4. **Public Access Block must allow public ACLs** — All four settings must be `false`:
   - `block_public_acls = false` → allows setting public ACLs
   - `block_public_policy = false` → allows public bucket policies
   - `ignore_public_acls = false` → respects public ACLs
   - `restrict_public_buckets = false` → allows public access

5. **`public-read` vs `public-read-write`** — The task says "publicly accessible" which means read access. Use `public-read` (not `public-read-write` which would allow anyone to write).

6. **Common ACL values:**
   | ACL | Description |
   |-----|-------------|
   | `private` | Owner gets full control (default) |
   | `public-read` | Everyone can read |
   | `public-read-write` | Everyone can read and write |
   | `authenticated-read` | Authenticated AWS users can read |

---

## Verify

```bash
terraform state list
# Expected:
# aws_s3_bucket.nautilus_bucket
# aws_s3_bucket_acl.nautilus_bucket_acl
# aws_s3_bucket_ownership_controls.nautilus_bucket_ownership
# aws_s3_bucket_public_access_block.nautilus_bucket_public_access

terraform state show aws_s3_bucket.nautilus_bucket
```
