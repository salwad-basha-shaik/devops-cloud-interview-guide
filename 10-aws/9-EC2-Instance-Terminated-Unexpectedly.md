# EC2 Instance terminated unexpectedly, How will you troubleshoot ?

# EC2 Instance Terminated Unexpectedly – Troubleshooting

## Question

**How do you troubleshoot when an AWS EC2 instance terminates unexpectedly?**

### Short Overview

This question evaluates your knowledge of the AWS EC2 lifecycle, termination causes, and troubleshooting techniques. It checks whether you know where to look—logs, events, policies, and monitoring—to identify the root cause of unexpected terminations.

---

## Answer

Common troubleshooting steps for unexpected EC2 terminations include:

- Check AWS Console & CloudTrail for termination events
- Review Auto Scaling policies if the instance is part of an Auto Scaling Group (ASG)
- Inspect Spot Instance interruptions
- Check Scheduled Events in the EC2 console
- Analyze system & application logs
- Review Billing and Service Limits
- Check for root device, EBS, IAM, or security issues

---

## Detailed Explanation

An EC2 instance may terminate unexpectedly due to user actions, AWS events, scaling policies, or system failures. A systematic troubleshooting approach involves the following steps:

---

### 🔎 1. Check AWS Console / CloudTrail

- Navigate to EC2 → Instances → State Transition Logs
- Verify if termination was:
  - User-initiated (via CLI, Console, or API)
  - Triggered by Auto Scaling Group actions
  - Due to Spot Instance interruption
- Use CloudTrail to track `TerminateInstances` API calls for detailed info

---

### ⚖️ 2. Auto Scaling Group (ASG) Policies

- If the instance is part of an ASG, possible termination causes include:
  - Scaling policies (e.g., scale-in events)
  - Health check failures from ELB or EC2
- Review ASG activity history in the AWS console to confirm

---

### ☁️ 3. Spot Instance Interruption

- AWS can reclaim Spot Instances when capacity is needed
- Interruption notices are available in:
  - CloudTrail logs
  - Instance metadata service: `http://169.254.169.254/latest/meta-data/spot/termination-time`

---

### 📅 4. Scheduled Events by AWS

- AWS may terminate or retire instances due to:
  - Host hardware failures
  - Planned maintenance or updates
- Check EC2 → Scheduled Events for this information

---

### 📝 5. System and Application Logs

- Review system logs such as `/var/log/messages`, `/var/log/syslog`, or Windows Event Viewer
- Check CloudWatch Logs for application crashes or shutdown-related scripts

---

### 💰 6. Billing & Service Limits

- Verify the AWS account is not out of free tier or credits
- Check if any service limits or quotas (e.g., maximum running instances) have been exceeded

---

### 🔐 7. EBS, IAM, and Security Issues

- Corrupted root EBS volumes can cause failing health checks that trigger termination in ASG-managed instances
- IAM permission changes or security policies might trigger automated termination scripts

---

## 🧠 Real-World Insight

> “In one case, an EC2 instance in an Auto Scaling Group had a corrupted root EBS volume that caused it to fail the EC2 health check. The ASG automatically replaced it with a new instance. CloudTrail logs confirmed the ASG-driven termination.”

---

## Summary Table

| Cause                | How to Detect                     | Resolution                                 |
|----------------------|----------------------------------|--------------------------------------------|
| User/API termination | CloudTrail logs                  | Educate teams; enforce IAM least privilege |
| Auto Scaling event    | ASG activity logs                | Adjust scaling and health check policies   |
| Spot interruption     | Instance metadata, CloudTrail   | Use On-Demand instances or Spot fleets     |
| AWS scheduled event   | EC2 → Scheduled Events          | Stop/start or migrate instances            |
| System/application crash | Instance logs, CloudWatch     | Fix application or OS issues                |
| Billing/limits issue  | Billing dashboard, service quotas | Increase limits or resolve billing          |
| EBS/IAM/security issue | CloudWatch logs and monitoring | Restore from snapshot; fix IAM policies     |

---

## Key Takeaway

> “Always start troubleshooting EC2 termination by checking CloudTrail and EC2 State Transition logs to identify the initiator. Then move through ASG, Spot, or Scheduled Events. Narrow down whether the termination was AWS-driven, policy-driven, or system-driven.”

---

👉 **Would you like me to create a flowchart-style troubleshooting diagram (e.g., “Start → Check CloudTrail → ASG → Spot → Scheduled Events”) to help with interviews or presentations?**
