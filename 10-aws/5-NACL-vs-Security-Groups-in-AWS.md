# NACL vs Security Groups in AWS

## Question

**What is the difference between Network ACLs (NACLs) and Security Groups (SGs) in AWS, and which one should you use?**

### Short Overview

This question evaluates your knowledge of AWS networking and security controls. Both NACLs and SGs manage inbound and outbound traffic, but differ in scope, behavior, and usage. They work together to secure workloads effectively.

---

## Answer

AWS provides two main mechanisms for traffic control:

- **Security Groups (SGs):** Stateful, instance-level firewalls
- **Network ACLs (NACLs):** Stateless, subnet-level firewalls

---

## Detailed Explanation

### 🛡️ 1. Security Groups (SG)

| Property          | Detail                                                                                  |
|-------------------|-----------------------------------------------------------------------------------------|
| Scope             | Attached to Elastic Network Interfaces (ENIs) → EC2 instances                          |
| Stateful          | ✅ Yes (inbound allowed → outbound response auto-allowed)                              |
| Rules             | Allow rules only (no explicit deny rules)                                              |
| Evaluation Order   | All rules evaluated; if any rule allows traffic → traffic permitted                    |
| Default Behavior   | Deny all inbound and outbound until explicitly allowed                                 |

**Use Case:**  
Protect EC2 instances (e.g., allow SSH only from a jump host, allow HTTP from ALB, block everything else).

**Example Rules:**  
- Inbound: Allow TCP 22 from 10.0.0.10/32  
- Inbound: Allow TCP 80 from 0.0.0.0/0  

---

### 🌐 2. Network ACLs (NACLs)

| Property          | Detail                                                                                      |
|-------------------|---------------------------------------------------------------------------------------------|
| Scope             | Applied at subnet level → affects all instances within the subnet                           |
| Stateful          | ❌ No (return traffic must be explicitly allowed)                                          |
| Rules             | Supports both Allow and Deny rules                                                         |
| Evaluation Order   | Rules evaluated in order of rule number (lowest first), first match determines action      |
| Default Behavior   | Default NACL allows all traffic; custom NACL denies all until configured                    |

**Use Case:**  
Add extra subnet-wide restrictions (e.g., block malicious IP ranges, deny specific ports).

**Example Rules:**  
- Inbound Rule #100: Allow TCP 80 from 0.0.0.0/0  
- Inbound Rule #110: Deny TCP 22 from 0.0.0.0/0  

---

## 🧠 Real-World Insight

> "In our organization, we rely primarily on Security Groups for instance-level protection because they are stateful and easier to manage. NACLs are used sparingly — mostly as an additional safeguard at the subnet layer, such as blocking specific IP ranges across multiple workloads. This layered approach ensures both fine-grained control (SG) and broad network policies (NACL)."

---

## Summary Table

| Feature         | Security Group (SG)    | Network ACL (NACL)          |
|-----------------|-----------------------|-----------------------------|
| Scope           | Instance / ENI        | Subnet                      |
| Stateful        | ✅ Yes                | ❌ No                      |
| Rules Type      | Allow only            | Allow + Deny                |
| Evaluation      | All rules checked     | Ordered (lowest number first)|
| Default Behavior| Deny all inbound/outbound | Default allow (custom: deny all) |
| Best For        | Instance-level protection | Subnet-level filtering       |

---

## Key Takeaway

> "Use Security Groups as your first line of defense for EC2 and services, and apply NACLs for broader subnet-level control or as an additional layer of network security."

---

👉 **Would you like a diagram-style visual comparing Security Groups vs NACLs as a quick interview reference?**
