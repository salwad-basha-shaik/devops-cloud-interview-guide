# What is AWS STS and how does it work ?

# AWS Security Token Service (STS) Overview

This guide explains what AWS Security Token Service (STS) is, how it works, and its primary use cases. It provides a detailed breakdown of the main STS API actions with examples and real-world insights.

---

## Question

**What is AWS Security Token Service (STS), and how does it work?**

---

## Short Explanation

AWS STS is a core security service that generates temporary, limited-privilege credentials for IAM users, roles, or federated identities. It enables secure access to AWS resources without using long-term IAM credentials.

---

## Answer Overview

AWS STS provides temporary credentials that are:

- Short-lived (15 minutes to 36 hours).
- Used in cross-account access, federated login (SSO), and to enhance security by avoiding hardcoded credentials.
- Accessed via key API calls such as:
  - **AssumeRole** — cross-account or role-based access
  - **AssumeRoleWithSAML** — enterprise SSO with SAML
  - **AssumeRoleWithWebIdentity** — login via Google, Facebook, OIDC providers
  - **GetSessionToken** — MFA-based temporary credentials
  - **GetFederationToken** — temporary credentials for custom applications

---

## Detailed Explanation

AWS STS issues temporary security credentials consisting of:

- Access Key ID
- Secret Access Key
- Session Token

These credentials have a limited lifetime and automatically expire, reducing the risk of credential leakage compared to long-term IAM keys.

---

## 🔑 1. AssumeRole

- **Purpose:** Grants temporary credentials for an IAM role
- **Duration:** 15 minutes to 12 hours
- **Typical Use Case:** Cross-account access or least-privilege delegation

### Example

Account A allows a role in Account B to access its S3 bucket. A user in Account B calls STS AssumeRole to obtain temporary credentials.

```bash
aws sts assume-role 
–role-arn arn:aws:iam::111122223333:role/ReadOnlyRole 
–role-session-name DemoSession
```


---

## 🏢 2. AssumeRoleWithSAML

- **Purpose:** Enables enterprise SSO into AWS using SAML 2.0
- **Duration:** Up to 12 hours
- **Typical Use Case:** Corporate Active Directory integration with AWS

### Use Case

A corporate user logs in with company credentials (e.g., AD or Okta). Behind the scenes, STS issues temporary credentials for AWS access.

---

## 🌐 3. AssumeRoleWithWebIdentity

- **Purpose:** Grants credentials to users authenticated by OIDC providers
- **Duration:** Up to 1 hour
- **Typical Use Case:** Mobile/web apps signing in with Google, Facebook, or Cognito

### Use Case

A mobile app user signs in with Google. AWS Cognito exchanges this identity for STS credentials, allowing the app to upload files to S3 securely.

---

## 🔒 4. GetSessionToken

- **Purpose:** Provides temporary credentials for IAM users
- **Duration:** From 15 minutes up to 36 hours
- **Typical Use Case:** MFA-enabled access for additional security

### Use Case

An IAM user with MFA enabled calls GetSessionToken to obtain temporary credentials after successfully verifying MFA.

---

## 👥 5. GetFederationToken

- **Purpose:** Generates temporary credentials for federated (non-IAM) users
- **Duration:** Up to 12 hours
- **Typical Use Case:** Web applications providing controlled AWS access to customers

### Use Case

A company portal provides customers with temporary access to download logs from S3 without needing to create IAM users for each customer.

---

## Real-world Insight

> “We replaced long-term IAM keys with STS AssumeRole in our CI/CD pipelines. This eliminated static credentials in GitHub Actions and made the environment far more secure. Developers authenticate via SSO, then STS issues temporary credentials for deployments.”

---

## Summary Table

| STS Action             | Use Case                              | Duration         |
|-----------------------|-------------------------------------|------------------|
| AssumeRole            | Cross-account / delegated access    | 15 minutes–12 hours |
| AssumeRoleWithSAML    | Enterprise SSO with SAML IdPs        | Up to 12 hours   |
| AssumeRoleWithWebIdentity | Federated login with OIDC (Google, etc.) | Up to 1 hour    |
| GetSessionToken       | MFA-based temporary credentials      | 15 minutes–36 hours |
| GetFederationToken    | Temporary creds for federated/custom users | Up to 12 hours |

---

## Key Takeaway

AWS STS enhances security and flexibility by replacing long-term IAM credentials with short-lived, on-demand tokens, enabling safer cross-account access, federation, and MFA enforcement.

---

Would you like me to also create a diagram showing how STS works (user → STS → temporary credentials → AWS service)? It can help visualize this process clearly in interviews.
