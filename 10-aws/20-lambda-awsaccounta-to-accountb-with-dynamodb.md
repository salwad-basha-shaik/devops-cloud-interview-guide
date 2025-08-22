# How a Lambda function in AWS Account A interact with Dynamodb in Account B ?

# AWS Lambda to DynamoDB Cross-Account Access Guide

This guide explains how an AWS Lambda function in Account A can securely access a DynamoDB table in Account B. It covers different approaches to configure cross-account access using IAM roles, resource policies, and STS AssumeRole.

---

## Question

**How can a Lambda function in AWS Account A securely access a DynamoDB table in Account B?**

---

## Short Explanation

Cross-account access requires explicit configuration of IAM roles, resource-based policies, or trust relationships. Lambda in one account does not automatically have permissions to access resources in another account.

---

## Answer Overview

A Lambda in Account A can interact with DynamoDB in Account B via:

- **Option 1** – IAM role in Account B trusted by Lambda’s role in Account A  
- **Option 2** – Resource-based policy on the DynamoDB table allowing Account A’s Lambda role  
- **Option 3** – Lambda calls STS AssumeRole to assume a role in Account B with DynamoDB permissions  

---

## Detailed Explanation

In multi-account setups, AWS enforces least privilege and requires explicit trust. You must configure cross-account IAM access for the Lambda function to interact securely with DynamoDB in another account.

---

## 🔑 1. IAM Role in Account B (Trusted Cross-Account Role)

- **Purpose:** Create a role in Account B with DynamoDB permissions and trust Account A’s Lambda role to assume it  
- **Who owns role:** Account B  
- **Trust policy:** Allows `sts:AssumeRole` from Account A’s Lambda execution role  

### Steps

1. In Account B, create an IAM role with `dynamodb:*` or least-privilege permissions.  
2. Add a trust policy allowing Account A’s Lambda role to assume it.  
3. In Account A, update Lambda’s execution role or code to call `sts:AssumeRole`.

✅ Best for centralized control in Account B.

---

## 📜 2. DynamoDB Resource-based Policy (Direct Access)

- **Purpose:** Attach a resource policy directly to the DynamoDB table in Account B  
- **Who owns policy:** DynamoDB table owner in Account B  

### Example Resource Policy


```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::<AccountA_ID>:role/LambdaExecutionRole"
    },
    "Action": "dynamodb:*",
    "Resource": "arn:aws:dynamodb:us-east-1:<AccountB_ID>:table/MyTable"
  }
}
```


✅ Simpler if you want to grant direct access without using STS.

---

## 🔄 3. AssumeRole via STS from Lambda Code

- **Purpose:** Lambda in Account A explicitly calls STS to assume a role in Account B  
- **Who manages it:** Account A developer (code modification required)  

### Example Python Code

```python
import boto3

sts_client = boto3.client("sts")

# Assume role in Account B
assumed_role = sts_client.assume_role(
    RoleArn="arn:aws:iam::<AccountB_ID>:role/DynamoDBAccessRole",
    RoleSessionName="LambdaSession"
)

# Use temporary credentials
dynamodb = boto3.client(
    "dynamodb",
    aws_access_key_id=assumed_role['Credentials']['AccessKeyId'],
    aws_secret_access_key=assumed_role['Credentials']['SecretAccessKey'],
    aws_session_token=assumed_role['Credentials']['SessionToken']
)

# Query DynamoDB table in Account B
response = dynamodb.list_tables()
print(response)
```


✅ Useful when fine-grained temporary credentials are preferred.

---

## Real-world Insight

> “In enterprise multi-account setups, STS AssumeRole into Account B is the most secure and scalable pattern. It avoids hardcoded permissions, enforces least privilege, and centralizes IAM governance.”

---

## Summary Table

| Approach               | Where Policy Lives               | Best Use Case                         |
|------------------------|---------------------------------|-------------------------------------|
| IAM Role in B (Trust)  | Account B IAM                   | Centralized control, scalable        |
| DynamoDB Resource Policy| DynamoDB Table (Account B)     | Simple, direct table-level sharing   |
| STS AssumeRole         | Lambda code in Account A        | Fine-grained, temporary credentials  |

---

## Key Takeaway

Cross-account access in AWS requires explicit trust setup. The recommended method is to configure an IAM role in Account B with DynamoDB permissions and allow Lambda in Account A to assume that role securely via STS.

---

Do you want me to create a step-by-step diagram/flow (Lambda → STS → Role in B → DynamoDB) for use in presentations?
