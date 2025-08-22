# How do you create resources on Azure ? Bicep or ARM or Terraform ?

# Creating Resources on Azure: ARM Templates, Bicep, or Terraform?

## Question

**How do you create resources on Azure? Should you use ARM templates, Bicep, or Terraform, and when is each appropriate?**

### Short Overview

This question evaluates your understanding of Infrastructure as Code (IaC) approaches in Azure. Microsoft provides ARM templates and Bicep, while Terraform is a third-party, cloud-agnostic tool. Each has pros, cons, and use cases.

---

## Answer

You can create resources in Azure using:

- **ARM Templates** – JSON-based, native Infrastructure as Code for Azure  
- **Bicep** – modern, simplified DSL that transpiles to ARM templates  
- **Terraform** – open-source, multi-cloud Infrastructure as Code tool

---

## Detailed Explanation

Infrastructure on Azure can be provisioned using different IaC approaches. The choice depends on ease of use, reusability, and multi-cloud needs.

---

### 📜 1. ARM Templates (Azure Resource Manager)

| Feature      | Description                                  |
|--------------|----------------------------------------------|
| Purpose      | JSON-based IaC native to Azure               |
| Cloud Scope  | Azure only                                   |
| Learning Curve | High (verbose JSON syntax)                   |

**Example snippet:**

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2021-04-01",
      "name": "mystorageacct",
      "location": "eastus",
      "kind": "StorageV2",
      "sku": {
        "name": "Standard_LRS"
      }
    }
  ]
}
```


**Use case:**  
- Traditional teams already invested in ARM templates  
- When direct native Azure support or features are required  

---

### 🪄 2. Bicep (Recommended by Microsoft)

| Feature      | Description                                  |
|--------------|----------------------------------------------|
| Purpose      | Simplified Domain Specific Language (DSL) that compiles to ARM templates |
| Cloud Scope  | Azure only                                   |
| Learning Curve | Easier; more readable and modular syntax     |

**Example snippet:**

```bash
resource storageAcct 'Microsoft.Storage/storageAccounts@2021-04-01' = {
  name: 'mystorageacct'
  location: 'eastus'
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}
```


**Use case:**  
- Modern IaC for Azure-native deployments  
- Easier readability, modularity, and reusability compared to ARM templates  
- Microsoft's recommended choice for new Azure projects  

---

### 🌍 3. Terraform

| Feature      | Description                                  |
|--------------|----------------------------------------------|
| Purpose      | Open-source, multi-cloud IaC tool            |
| Cloud Scope  | Multi-cloud (Azure, AWS, GCP, on-premises)  |
| Learning Curve | Medium                                    |

**Example snippet:**

```bash
resource "azurerm_storage_account" "example" {
  name                     = "mystorageacct"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

**Use case:**  
- Multi-cloud environments where one tool manages AWS, Azure, and GCP  
- When leveraging strong community modules, state management, and orchestration capabilities  

---

## 🧠 Real-world Insight

> “In many enterprises, Terraform is adopted for multi-cloud governance, but for Azure-only projects, Bicep provides a cleaner, Microsoft-backed solution. ARM templates are still around but mostly used under the hood or for legacy automation.”

---

## Summary Table

| Tool      | Azure Native | Multi-cloud | Ease of Use | Best For                    |
|-----------|--------------|-------------|-------------|-----------------------------|
| ARM       | ✅ Yes       | ❌ No       | ❌ Hard     | Legacy/JSON-based deployments |
| Bicep     | ✅ Yes       | ❌ No       | ✅ Easy     | Azure-only IaC (Microsoft recommended) |
| Terraform | ✅ Yes       | ✅ Yes      | ⚡ Medium   | Multi-cloud, reusable modules |

---

## Key Takeaway

> “Use Bicep for Azure-native projects due to its simplicity and Microsoft support. Choose Terraform if you require multi-cloud or advanced orchestration. ARM templates remain valid but are better replaced with Bicep for new projects.”
