# EC2 Instance terminated unexpectedly, How will you troubleshoot ?

# EC2 Instance Terminated Unexpectedly – Troubleshooting

## Question

**How do you troubleshoot when an EC2 instance is terminated unexpectedly?**

### Short Overview

This question tests your ability to investigate root causes of unexpected EC2 terminations in AWS. Instances may terminate due to configuration issues, scaling policies, hardware failures, billing problems, or automation rules. Understanding AWS logs and service behaviors is key to identifying the reason.

---

## Answer

To troubleshoot an unexpectedly terminated EC2 instance, check the following:

- **CloudTrail Logs** – for termination events (manual or automated)
- **Auto Scaling Group (ASG) Policies** – instance replacement or scale-in activities
- **Instance Termination Protection** – whether it was disabled
- **Billing & Limits** – account suspensions or insufficient capacity
- **AWS Health Dashboard** – hardware or Availability Zone (AZ) issues
- **Instance Metadata & Logs** – review system and application logs

---

## Detailed Explanation

EC2 instances can be terminated by users, AWS automation, scaling actions, or AWS itself. A systematic investigation includes:

---

### 🔎 1. Check CloudTrail for Termination Events

Use AWS CloudTrail to find who or what triggered the termination:

aws cloudtrail lookup-events –lookup-attributes AttributeKey=ResourceName,AttributeValue=i-1234567890abcdef


This reveals if termination was manual (console/CLI) or automated (ASG, AWS service).

---

### ⚙️ 2. Review Auto Scaling Groups / Spot Instance Behavior

- If part of an ASG, scale-in policies or health checks might have terminated the instance.
- Spot Instances can be reclaimed by AWS when capacity is low.

Check instance details:

aws ec2 describe-instances –instance-ids i-1234567890abcdef


---

### 🔐 3. Verify Termination Protection

Termination protection prevents accidental termination. Confirm if it was disabled:


aws ec2 describe-instance-attribute –instance-id i-1234567890abcdef –attribute disableApiTermination


---

### 💰 4. Check Billing and Quotas

- Account suspensions due to billing issues can cause instance termination.
- Verify service quotas (e.g., running instance limits) have not been exceeded.

---

### 🏥 5. AWS Health Dashboard

AWS may retire or reclaim instances due to hardware failure or maintenance. Check the AWS Health Dashboard for relevant events.

---

### 📂 6. Review System & Application Logs

- Review system logs via EC2 console: Actions → Instance Settings → Get System Log.
- Check CloudWatch Logs (if configured) to identify application or OS issues before termination.

---

## 🧠 Real-World Insight

> “In one project, EC2 instances were terminated unexpectedly due to an aggressive Auto Scaling scale-in policy. CloudTrail confirmed ASG health checks triggered termination. Adjusting health check grace periods resolved the issue.”

---

## Summary Table

| Check Area           | What to Look For                             |
|---------------------|----------------------------------------------|
| CloudTrail Logs      | Who/what triggered termination               |
| ASG / Spot Instances | Scale-in events or spot reclamation          |
| Termination Protection | Disabled setting → accidental termination  |
| Billing & Quotas     | Account suspension or exceeded limits        |
| AWS Health Dashboard | Hardware failure or AWS-initiated retirement |
| System & App Logs    | Application/OS issues before termination      |

---

## Key Takeaway

> “Start troubleshooting EC2 termination by checking CloudTrail, then examine scaling policies, billing, and AWS health events. Enable termination protection and proactive monitoring to minimize accidental or unexpected terminations.”

---

👉 **Would you like a troubleshooting flowchart (step-by-step visual) to help remember this process in interviews?**


