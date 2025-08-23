# What kind of metrics do you scrape with prometheus in your current organization?

# Prometheus Metrics in Our Organization

## Question

**What types of metrics do you collect with Prometheus, and why are they important for your applications and infrastructure?**

---

### Short Explanation

This question explores your understanding of effective monitoring with Prometheus. It focuses on the categories of operational, application, and business metrics that are vital for ensuring performance, reliability, and capacity planning.

---

## Answer

In our organization, Prometheus scrapes multiple types of metrics including:

- **Node/Infrastructure Metrics** – CPU, memory, disk usage, network throughput
- **Application Metrics** – request rates, error rates, response latency, custom business metrics
- **Service Metrics** – service availability, connection counts, queue lengths
- **Kubernetes Metrics** – pod health, container resource usage, deployment statuses
- **Database Metrics** – query rates, cache hit/miss ratios, connection pool utilization

---

### Detailed Explanation

Prometheus uses exporters and instrumentation libraries to scrape metrics from diverse sources. These metrics provide real-time insights about system health, performance bottlenecks, and SLA adherence, enabling proactive issue detection and capacity planning.

---

## 📦 1. Node/Infrastructure Metrics

| Purpose           | Monitor underlying server or VM health                     |
|-------------------|-------------------------------------------------------------|
| Tools/Exporters  | node_exporter, cadvisor                                     |
| Common Metrics   | CPU usage, memory utilization, disk I/O, network traffic    |

**Use case:**  
- Detect resource saturation before it impacts applications

---

## 🌐 2. Application Metrics

| Purpose           | Track application performance and error rates              |
|-------------------|-------------------------------------------------------------|
| Tools/Frameworks  | Prometheus client libraries, OpenTelemetry SDKs             |
| Common Metrics   | HTTP request rate, error rate, latency histograms, business KPIs |

**Use case:**  
- Alert if response time exceeds thresholds or error rate spikes

---

## ☁️ 3. Service Metrics

| Purpose           | Observe dependencies between microservices                 |
|-------------------|-------------------------------------------------------------|
| Metrics Examples | Active connections, request queues, retries, uptime          |

**Use case:**  
- Ensure microservices handle traffic load and detect bottlenecks

---

## 🌍 4. Kubernetes Metrics

| Purpose           | Monitor containerized workloads and cluster health          |
|-------------------|-------------------------------------------------------------|
| Tools/Exporters  | kube-state-metrics, cAdvisor                                 |
| Common Metrics   | Pod CPU/memory, restart counts, deployment replicas, node status |

**Use case:**  
- Detect failing pods, resource overcommitment, or scaling issues

---

## 🧪 5. Database Metrics

| Purpose           | Ensure database performance and reliability                  |
|-------------------|-------------------------------------------------------------|
| Metrics Examples | Query rates, slow queries, cache hit/miss ratios, connection pool usage |

**Use case:**  
- Alert on high latency queries or connection exhaustion affecting app performance

---

## 🧠 Real-world Insight

> “We scrape NodeExporter metrics for server health, kube-state-metrics for pod and deployment monitoring, and instrument our applications to expose request latency, error rates, and business-specific counters. Prometheus alerts on threshold breaches ensure proactive issue resolution.”

---

## Summary Table

| Metric Type         | Tools/Exporters                  | Use Case                               |
|---------------------|---------------------------------|---------------------------------------|
| Node/Infrastructure  | node_exporter, cadvisor          | Resource monitoring, alert on saturation |
| Application         | Prometheus client, OpenTelemetry | Track request rate, errors, latency    |
| Service             | Custom instrumentation           | Service availability, connection stats |
| Kubernetes          | kube-state-metrics, cAdvisor     | Pod health, container resource usage   |
| Database            | postgres_exporter, mysqld_exporter | DB performance, cache and connections |

---

## Key Takeaway

**Scraping diverse metrics with Prometheus gives a holistic view of infrastructure, application, and business health — enabling proactive monitoring, faster debugging, and informed scaling decisions.**

