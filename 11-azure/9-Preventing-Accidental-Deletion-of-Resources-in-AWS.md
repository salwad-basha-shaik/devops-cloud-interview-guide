# Your team has access to delete resources. How do you prevent accidental deletion

# Preventing Accidental Deletion of Resources in AWS

## Question

**Your team has access to delete resources. How do you prevent accidental deletion?**

### Short Overview

This question evaluates your knowledge of governance, access control, and safety mechanisms in AWS that protect critical resources from being unintentionally deleted by users with elevated privileges.

---

## Answer

AWS provides several mechanisms to prevent accidental deletion:

- IAM Policies & Least Privilege – restrict delete permissions  
- Resource-based Protections – S3 bucket/object locks, DynamoDB & RDS deletion protection  
- Service-specific Safeguards – EC2 termination protection, CloudFormation termination protection  
- Tag-based Policies – prevent deletion of resources with specific tags (e.g., Environment=Production)  
- AWS Config Rules & Service Control Policies (SCPs) – enforce organization-wide prevention rules  
- Backups & Versioning – enable recovery in case of accidental deletion  

---

## Detailed Explanation

When multiple team members have deletion access, accidental removal of critical resources (S3, EC2, RDS, etc.) is a serious risk. AWS offers several preventive and protective controls:

---

### 🔐 1. IAM Policies & Least Privilege

| Purpose | Grant only minimum necessary permissions                      |
|---------|--------------------------------------------------------------|
| Level   | User or Role level                                           |

**Example policy to deny bucket deletion:**

```json
{
  "Effect": "Deny",
  "Action": "s3:DeleteBucket",
  "Resource": "*"
}
```


✅ Use case: Developers can read/write S3 objects but cannot delete buckets.

---

### 🛑 2. Resource-based Deletion Protection

- **S3:** Enable Object Lock & Versioning  
- **RDS/DynamoDB:** Enable deletion protection flag  
- **EBS/EC2:** Enable termination protection  

✅ Use case: Prevent accidental termination of a production database or instance.

---

### 📛 3. Tag-based Guardrails

Apply tags like `DoNotDelete=true` or `Environment=Production`. Use IAM condition in policies to block deletion if these tags exist:


```json
{
  "Effect": "Deny",
  "Action": "ec2:TerminateInstances",
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "aws:ResourceTag/DoNotDelete": "true"
    }
  }
}
```


✅ Use case: Developers cannot delete EC2 instances tagged as production.

---

### 🏛 4. Organization-wide Controls (SCPs & Config Rules)

- **Service Control Policies (SCPs):** Prevent deletions at AWS Organization account level  
- **AWS Config Rules:** Detect deletion attempts on protected resources and trigger alerts or automated remediation  

✅ Use case: Enforce “no deletion of S3 buckets in Prod OU” across accounts.

---

### 💾 5. Backups & Versioning

Mistakes happen; ensure recovery by enabling:

- S3 Versioning (to restore deleted objects)  
- RDS automated backups and snapshots  
- AWS Backup service for centralized backup management  

✅ Use case: Rollback accidentally deleted objects or database tables.

---

## 🧠 Real-world Insight

> “In our enterprise AWS accounts, we combine deletion protection flags and tag-based IAM conditions to prevent accidental termination of production resources. AWS Config sends immediate alerts if any protected resource is modified.”

---

## Summary Table

| Method                 | Protection Type          | Example Use Case                            |
|------------------------|--------------------------|--------------------------------------------|
| IAM Least Privilege    | Preventive              | Deny delete actions                        |
| Resource Deletion Protection | Preventive         | EC2, RDS, DynamoDB termination protection  |
| Tag-based IAM Conditions | Preventive             | Block deletion of production-tagged resources|
| SCPs & AWS Config Rules | Organization-wide Guard | Prevent deletions across accounts          |
| Backups & Versioning    | Recovery                | Restore accidentally deleted data          |

---

## Key Takeaway

> “Preventing accidental deletion requires layered safeguards — restrict permissions, enable resource protections, enforce organization-wide guardrails, and always maintain backups for recovery.”
