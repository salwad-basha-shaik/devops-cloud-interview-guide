# Have you used AWS EFS ? If yes, what issues did you run into ?

# AWS Elastic File System (EFS)

## Question

**Have you used AWS EFS? If yes, what issues did you run into?**

### Short Overview

This question evaluates your practical experience with Amazon EFS — a fully managed NFS file system for AWS. While it offers scalability and high availability, real-world use often presents performance, cost, and networking challenges.

---

## Answer

Yes, I have worked with AWS EFS, and while it is highly useful for shared storage across multiple EC2 instances or containers, I encountered issues such as:

- Mounting problems due to missing NFS utilities or security group misconfiguration
- High latency for small, frequent read/write operations
- Unexpected costs when filesystems were left active with provisioned throughput
- IAM and security group restrictions causing access problems
- Throughput limits leading to degraded performance under heavy workloads

---

## Detailed Explanation

AWS EFS provides elastic, shared, POSIX-compliant file storage. Unlike EBS (block storage) or S3 (object storage), EFS allows multiple EC2 instances, containers, or Lambda functions to mount the same filesystem concurrently.

However, common real-world issues include:

---

### ⚡ 1. Mounting & Connectivity Issues

| Issue              | Description                                                       | Fix                                         |
|--------------------|-------------------------------------------------------------------|---------------------------------------------|
| Missing NFS utils   | Without `amazon-efs-utils` or `nfs-utils`, mount commands fail    | Install these utilities before mounting     |
| Security groups     | EC2 and EFS security groups must allow NFS port 2049             | Open inbound/outbound NFS port 2049 in SGs  |
| VPC setup          | EFS requires mount targets in each AZ where EC2 instances run     | Create EFS mount targets in all relevant AZs|

**Example mount command:**

sudo yum install -y amazon-efs-utils 
sudo mount -t efs fs-12345678:/ /mnt/efs


---

### 📉 2. Performance & Latency

| Problem           | Why it Happens                                   | Mitigation                        |
|-------------------|-------------------------------------------------|---------------------------------|
| High latency      | EFS is network-attached storage → slower for small files | Use EFS for shared content, avoid DB workloads |
| Throughput limits | Bursting mode may throttle sustained workloads  | Switch to Provisioned Throughput mode |
| IOPS constraints  | Not ideal for workloads needing low latency & high IOPS | Use EBS for latency-sensitive applications |

---

### 💰 3. Cost Challenges

- EFS Standard is relatively expensive for large data sets compared to S3.
- Provisioned throughput can generate unexpected costs.
- Keeping old data active inflates bills.

**Mitigation:**  
Use EFS Infrequent Access (EFS-IA) lifecycle policies to move cold data to lower-cost storage.

---

### 🔐 4. Security Considerations

- IAM role policies must allow access, or mounting will fail.
- Misconfigured security groups can lead to blocked connections or unauthorized access.
- Encryption at rest and in transit should be enabled for compliance and security.

---

## 🧠 Real-world Insight

> “In one project, we used EFS for containerized workloads in ECS Fargate to share logs. Initially, tasks failed to mount EFS due to missing IAM roles and restrictive security groups. Later, performance degraded under many small writes, so we shifted hot data to EBS and reserved EFS for shared static files.”

---

## Summary Table

| Issue Area   | Common Problems                   | Mitigation                     |
|--------------|----------------------------------|--------------------------------|
| Mounting     | Missing utils, SG rules, AZ mount targets | Install utils, open port 2049, create mount targets |
| Performance  | High latency, IOPS & throughput limits    | Use provisioned throughput, avoid DB workloads |
| Cost         | Expensive storage                         | Enable EFS-IA lifecycle          |
| Security     | IAM & SG misconfigurations                | Apply least privilege IAM; enable encryption |

---

## Key Takeaway

> “EFS is excellent for shared storage and scaling without capacity management, but comes with performance trade-offs and cost considerations. Use it for workloads needing multi-instance access; avoid latency-sensitive applications.”

---

👉 **Would you like me to create a side-by-side comparison (EFS vs EBS vs S3) as a quick interview-ready reference?**


