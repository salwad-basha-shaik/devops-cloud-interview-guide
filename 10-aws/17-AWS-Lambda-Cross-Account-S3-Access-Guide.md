# How can AWS lambda in one AWS account connect to s3 bucket in other account ?

# AWS Lambda Cross-Account S3 Access Guide

This guide explains how an AWS Lambda function running in one AWS account (Account A) can access an S3 bucket located in another AWS account (Account B). It covers the two main approaches for enabling secure, cross-account access with examples and best practices.

---

## Question

**How can an AWS Lambda function running in Account A access an S3 bucket that resides in Account B?**

---

## Short Explanation

Lambda and S3 are in different AWS accounts, so you need to configure IAM roles, permissions, and bucket policies correctly to enable secure, cross-account access.

---

## Answer Overview

There are two main approaches to allow Lambda in one account to access an S3 bucket in another account:

1. **Cross-account IAM Role (Recommended)** – Lambda assumes an IAM role from the bucket’s account.
2. **Bucket Policy with Lambda’s IAM Role** – The bucket policy explicitly grants permissions to the Lambda execution role from another account.

---

## Detailed Explanation

AWS Lambda functions execute with an IAM execution role. To access an S3 bucket in a different account, explicit cross-account trust and permissions are required.

---

## 1. Using Cross-account IAM Role (Best Practice)

### Steps

- **In Account B (bucket owner):**
  - Create an IAM role (e.g., `S3AccessRole`) with the necessary S3 access permissions.
  - Add a trust policy allowing the Lambda's role from Account A to assume this role.

```json
// Account B - Trust Policy
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::<AccountA_ID>:role/LambdaExecutionRole"
    },
    "Action": "sts:AssumeRole"
  }
}
```

- **In Account A (Lambda owner):**
  - Modify the Lambda’s IAM execution role or code to assume the `S3AccessRole` in Account B.

### Example (Python boto3)

```bash
import boto3
sts_client = boto3.client(‘sts’)
assumed_role = sts_client.assume_role( RoleArn=“arn:aws:iam::<AccountB_ID>:role/S3AccessRole”, RoleSessionName=“LambdaCrossAccountSession” )
s3_client = boto3.client( ‘s3’, aws_access_key_id=assumed_role‘Credentials’‘AccessKeyId’, aws_secret_access_key=assumed_role‘Credentials’‘SecretAccessKey’, aws_session_token=assumed_role‘Credentials’‘SessionToken’ )
s3_client.get_object(Bucket=“my-crossaccount-bucket”, Key=“file.txt”)
```

✅ **Best for security:** No need to hardcode bucket policies for external accounts. Access is tightly controlled.

---

## 2. Using Bucket Policy with Lambda’s IAM Role

Alternatively, the bucket owner can grant direct access to the Lambda execution role by updating the bucket policy.

### Bucket Policy Example (Account B)

{ “Version”: “2012-10-17”, “Statement”: [ { “Effect”: “Allow”, “Principal”: { “AWS”: “arn:aws:iam::<AccountA_ID>:role/LambdaExecutionRole” }, “Action”: “s3:GetObject”, “s3:PutObject”, “Resource”: “arn:aws:s3:::my-crossaccount-bucket/*” } ] }


✅ **Simpler to implement.**

⚠️ **Less flexible for large-scale multi-account setups.**

---

## Real-world Insight

> “In enterprise setups with many accounts, we standardize on the STS assume-role method because it scales better and avoids cluttered bucket policies. For one-off cases, a bucket policy granting Lambda’s IAM role access is often sufficient.”

---

## Summary Table

| Approach            | How it Works                              | Pros                          | Cons                           |
|---------------------|------------------------------------------|-------------------------------|--------------------------------|
| Cross-account Role   | Lambda assumes IAM role in bucket’s account | More secure, scalable, centralized | Requires `sts:AssumeRole` setup |
| Bucket Policy       | Bucket policy grants access directly to Lambda’s role | Quick, simple                  | Harder to manage at scale, bucket policy grows |

---

## Key Takeaway

To connect Lambda in one AWS account to S3 in another:

- Use **cross-account IAM roles** for scalability and security.
- For smaller setups, a **bucket policy granting the Lambda role access** is acceptable.

---

Would you like a diagram-style explanation (architecture diagram) for this scenario to help visualize the flow during interviews?
