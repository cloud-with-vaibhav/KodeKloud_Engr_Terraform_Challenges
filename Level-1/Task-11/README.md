# Terraform Level-1 Task-11: Create Alarm Using Terraform

## Task Description

The Nautilus DevOps team is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm.

**Requirements:**
- Create a CloudWatch alarm named `xfusion-alarm`
- Monitor CPU utilization of an EC2 instance
- Trigger the alarm when CPU utilization exceeds **80%**
- Set the evaluation period to **5 minutes**
- Use a **single** evaluation period
- Working directory: `/home/bob/terraform`
- File: `main.tf`

---

## Solution

### main.tf

```hcl
resource "aws_cloudwatch_metric_alarm" "xfusion_alarm" {
  alarm_name          = "xfusion-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "This alarm monitors EC2 CPU utilization"
}
```

---

## Steps to Execute

```bash
cd /home/bob/terraform
cat > main.tf <<EOF
resource "aws_cloudwatch_metric_alarm" "xfusion_alarm" {
  alarm_name          = "xfusion-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "This alarm monitors EC2 CPU utilization"
}
EOF

terraform init
terraform plan
terraform apply -auto-approve
```

---

## Explanation

| Parameter | Value | Why |
|-----------|-------|-----|
| `alarm_name` | `xfusion-alarm` | As specified in the task |
| `comparison_operator` | `GreaterThanThreshold` | Trigger when CPU **exceeds** 80% |
| `evaluation_periods` | `1` | Single evaluation period as required |
| `metric_name` | `CPUUtilization` | Standard AWS metric for CPU monitoring |
| `namespace` | `AWS/EC2` | EC2 metrics live under this namespace |
| `period` | `300` | 5 minutes = 300 seconds |
| `statistic` | `Average` | Average CPU usage over the period |
| `threshold` | `80` | Alarm fires when CPU > 80% |

---

## Tricks & Notes

1. **Period is in seconds** — 5 minutes = 300 seconds. Don't put `5` thinking it's minutes.

2. **evaluation_periods vs period** — These are different:
   - `period` = duration of each evaluation window (in seconds)
   - `evaluation_periods` = how many consecutive periods must breach the threshold before alarming

3. **comparison_operator values:**
   - `GreaterThanThreshold` — strictly greater than (>)
   - `GreaterThanOrEqualToThreshold` — (>=)
   - `LessThanThreshold` — (<)
   - `LessThanOrEqualToThreshold` — (<=)

4. **"Exceeds 80%"** means use `GreaterThanThreshold` (not `GreaterThanOrEqualToThreshold`).

5. **No `dimensions` block needed** — The task doesn't specify a particular EC2 instance ID, so omitting dimensions means the alarm is created without targeting a specific instance. LocalStack accepts this.

6. **No `actions_enabled` or SNS topic needed** — The task only asks to create the alarm, not to send notifications.

---

## Verify

```bash
terraform state list
# Expected: aws_cloudwatch_metric_alarm.xfusion_alarm

terraform state show aws_cloudwatch_metric_alarm.xfusion_alarm
```
