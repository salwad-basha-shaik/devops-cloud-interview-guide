# What is trust policy in AWS and why is it used ?

# AWS Trust Policy Explained

This guide explains what a trust policy is in AWS, its purpose, and provides examples of common use cases. It clarifies how trust policies work alongside permission policies to enable secure IAM role usage.

---

## Question

**What is a trust policy in AWS, and why is it used?**

---

## Short Explanation

A trust policy defines which AWS principals (users, services, or accounts) are allowed to assume an IAM role. Without a proper trust policy, the role cannot be assumed by any entity.

---

## Answer Overview

A trust policy is a JSON policy document attached to an IAM role that:

- Controls who can assume the role  
- Establishes trust relationships between AWS accounts and services  
- Works alongside permission policies that define what actions the role can perform  

---

## Detailed Explanation

When you create an IAM role, you attach two policies:

1. **Permission Policy** – Defines what actions the role can perform (e.g., access S3 buckets).  
2. **Trust Policy** – Defines who or what can assume the role. This is evaluated when an entity calls `sts:AssumeRole`.

---

## 📦 Example 1: EC2 Instance Role

Trust policy allowing EC2 to assume the role:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }
}
```


**Use case:**  
Attach this role to an EC2 instance so applications running on it can call AWS APIs without embedding credentials.

---

## 🌐 Example 2: Cross-Account Role

Trust policy allowing another AWS account to assume the role:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::123456789012:root"
    },
    "Action": "sts:AssumeRole"
  }
}
```


**Use case:**  
Allow developers from a partner or another AWS account temporary access to your resources.

---

## ☁️ Example 3: Lambda Role

Trust policy allowing AWS Lambda to assume the role:


```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": {
      "Service": "lambda.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }
}
```


**Use case:**  
Lambda functions need temporary credentials to access AWS services like DynamoDB or S3.

---

## Real-world Insight

> “In our setup, we use trust policies extensively for cross-account access. For instance, a CI/CD pipeline in one AWS account assumes a role in another account to deploy infrastructure, avoiding long-term credential sharing.”

---

## Summary Table

| Trust Policy For | Principal Example                           | Use Case                             |
|------------------|--------------------------------------------|------------------------------------|
| EC2 Role         | `"Service": "ec2.amazonaws.com"`           | Let EC2 instances call AWS APIs    |
| Lambda Role      | `"Service": "lambda.amazonaws.com"`        | Let Lambda functions access resources |
| Cross-Account    | `"AWS": "arn:aws:iam::123456789012:root"` | Delegate access between AWS accounts |

---

## Key Takeaway

A **trust policy defines who can assume a role**, while a **permission policy defines what actions that role can perform**. Both policies are required for secure and effective role-based access control in AWS.

---

Would you like me to also create the same formatted explanation for **Permission Policy in AWS** to compare both side by side?
