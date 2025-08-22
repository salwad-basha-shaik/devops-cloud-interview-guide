# How to Schedule a Script to Run Every Day on Azure

## Question

**How can you schedule a script to automatically run daily on Azure, and which services can be used to achieve this?**

### Short Overview

This question tests your knowledge of automation options in Azure. It evaluates whether you understand how Azure services like Automation Accounts, Logic Apps, Functions, and Data Factory can be used to schedule recurring tasks, similar to cron jobs on Linux.

---

## Answer

Azure provides multiple ways to schedule scripts:

- **Azure Automation Account with Runbooks** – ideal for PowerShell/Python scripts
- **Azure Logic Apps** – low-code workflow automation with built-in scheduler
- **Azure Functions with Timer Trigger** – serverless scheduling of code
- **Azure Data Factory** – data pipelines with time-based triggers

---

## Detailed Explanation

Azure supports several scheduling methods depending on your use case, scripting language, and automation requirements.

---

### ⚙️ 1. Azure Automation Account with Runbooks

| Purpose   | Run PowerShell or Python scripts on a schedule                  |
|-----------|-----------------------------------------------------------------|
| Languages | PowerShell, Python                                              |
| Cost      | Pay per job runtime                                             |

**Example Runbook (PowerShell):**

```text
Write-Output “Hello! Script executed successfully.”
```


**Steps:**

1. Create an Automation Account  
2. Import or create a Runbook (script)  
3. Attach a Schedule (daily, hourly, cron-based)

**Use case:**  
Rotate secrets, clean up resources, generate daily reports.

---

### 🔄 2. Azure Logic Apps (Recurrence Trigger)

| Purpose | Low-code workflow automation with scheduling                      |
|---------|-------------------------------------------------------------------|
| Languages | No code / connector-based                                        |
| Cost    | Pay per execution                                                 |

**Sample recurrence trigger (JSON):**

```json
{
  "triggers": {
    "Recurrence": {
      "type": "Recurrence",
      "recurrence": {
        "frequency": "Day",
        "interval": 1
      }
    }
  }
}
```


**Use case:**  
Automate workflows like sending daily emails, kicking off jobs, invoking REST APIs.

---

### ⚡ 3. Azure Functions (Timer Trigger)

| Purpose  | Serverless code execution on a schedule                          |
|----------|------------------------------------------------------------------|
| Languages| C#, Python, JavaScript, PowerShell                               |
| Cost     | Consumption-based (per execution)                               |

**Example function (C#):**

```c#
public static void Run([TimerTrigger("0 0 9 * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Daily script executed at: {DateTime.Now}");
}
```


**Note:** CRON syntax is used for schedule definition.

**Use case:**  
Lightweight scripts, data synchronization, monitoring, daily reports.

---

### 📊 4. Azure Data Factory (Trigger Pipelines)

| Purpose  | Orchestration of data movement & ETL scripts                      |
|----------|------------------------------------------------------------------|
| Languages| JSON-based pipeline definitions                                  |
| Cost     | Pay per pipeline activity                                        |

**Sample JSON trigger:**

```json
{
  "triggers": {
    "dailyTrigger": {
      "type": "ScheduleTrigger",
      "typeProperties": {
        "recurrence": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }
  }
}
```


**Use case:**  
Schedule ETL/ELT pipelines daily, move data from storage to databases.

---

## 🧠 Real-world Insight

> “We use Azure Automation Runbooks for infrastructure scripts like nightly VM shutdowns, Azure Functions for lightweight serverless tasks such as sending usage metrics, and Azure Data Factory for enterprise ETL jobs.”

---

## Summary Table

| Service             | Best For                     | Languages Supported         | Cost Model          |
|---------------------|------------------------------|----------------------------|---------------------|
| Automation Account  | Infrastructure tasks, PowerShell/Python scripts | PowerShell, Python      | Runtime-based       |
| Logic Apps          | Workflows & connectors        | Low-code                   | Per execution       |
| Functions           | Serverless code execution     | C#, Python, JS, PowerShell | Per execution       |
| Data Factory        | Data pipelines & ETL          | JSON pipelines             | Activity runtime    |

---

## Key Takeaway

> “Azure offers multiple ways to schedule scripts. Choose Automation Accounts for traditional scripting, Logic Apps for low-code workflows, Functions for lightweight serverless jobs, and Data Factory for complex data pipelines.”
