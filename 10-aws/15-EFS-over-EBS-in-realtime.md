# When will you go for EFS over EBS in realtime ?

# AWS Storage Choice: EFS vs EBS

## Question

**When would you choose Amazon EFS over Amazon EBS in real-time scenarios?**

### Short Overview

This question evaluates your understanding of AWS storage options. Both EBS and EFS provide persistent storage, but differ in architecture, scalability, and access patterns. Choosing the right one depends on workload requirements.

---

## Answer

- Use **EFS (Elastic File System)** when you need **shared, scalable, concurrent access** across multiple EC2 instances.
- Use **EBS (Elastic Block Store)** when you need **low-latency, high-performance storage** attached to a single EC2 instance.

---

## Detailed Explanation

Both EBS and EFS are persistent storage options in AWS, but they serve different purposes.

---

### 💽 1. Amazon EBS (Elastic Block Store)

| Feature     | Description                                      |
|-------------|------------------------------------------------|
| Purpose     | Block storage that attaches to one EC2 instance at a time |
| Access      | Single EC2 instance (except Multi-Attach for some volumes) |
| Performance | High IOPS, low latency                           |
| Scaling     | Fixed size (manual resize required)             |
| Durability  | Data replicated within the same Availability Zone (AZ) |

**Use case:**
- Databases (e.g., MySQL, MongoDB) requiring fast block storage
- Applications needing single-instance attachment
- Boot volumes for EC2 instances

---

### 📂 2. Amazon EFS (Elastic File System)

| Feature     | Description                                               |
|-------------|-----------------------------------------------------------|
| Purpose     | Fully managed NFS file system providing shared storage    |
| Access      | Multiple EC2 instances (across AZs) can mount concurrently |
| Performance | Scales automatically with storage size and throughput    |
| Scaling     | Elastic — grows and shrinks as files are added or removed |
| Durability  | Data replicated across multiple AZs                        |

**Use case:**
- Web/app servers needing shared content (e.g., media for WordPress)
- Big Data & analytics workloads with multiple compute nodes accessing the same data
- Container workloads in ECS/EKS requiring shared volumes
- User home directories for multiple users

---

## 🧠 Real-world Insight

> “In our project, we used EBS for database storage due to its high IOPS and low latency. For a fleet of web servers serving static content, we switched to EFS since multiple servers needed concurrent read/write access to the same files.”

---

## 📊 Summary Table

| Feature     | EBS                         | EFS                                    |
|-------------|-----------------------------|----------------------------------------|
| Type        | Block Storage               | Network File System (NFS)               |
| Access      | Single EC2 (Multi-Attach limited) | Multiple EC2 instances, cross-AZ    |
| Scaling     | Manual (resize required)     | Automatic (elastic growth/shrink)      |
| Performance | High IOPS, low latency       | Scales with throughput                  |
| Durability  | Within a single AZ           | Across multiple AZs                     |
| Best For    | Databases, single-instance apps | Shared storage, web/app content, HPC  |

---

## Key Takeaway

> “Choose EBS when you need high-performance storage for a single instance (like databases). Choose EFS when multiple EC2 instances require shared, scalable storage (like web farms or containerized applications).”

---

👉 **Would you like me to also prepare the same formatted answer for EFS vs S3 (another common interview question)?**
