# Week 6 — Looting Buckets

## Investigation Overview

**Platform:** Microsoft Azure  
**Course:** Mad Hat Labs — Azure Fundamentals  
**Focus:** Cloud Storage Security, Identity, Private Networking, and Key Vault

## Environment

- **Platform:** Microsoft Azure
- **Environment:** Live multi-user Azure training tenant
- **Tools:** Azure Portal, Firefox (Private Browsing)
- **Services Investigated:**
  - Azure Storage Accounts
  - Azure Blob Storage
  - Azure Role-Based Access Control (RBAC)
  - Azure Managed Identities
  - Azure Private Endpoints
  - Azure Key Vault

### Objective

Investigate how an Azure application securely accesses its data through storage accounts, managed identities, private endpoints, and Azure Key Vault.

During the investigation, I examined five components of the data-access chain and discovered a separate backup storage account that allowed anonymous public access.

### Investigation Scope

1. **The Data:** Identify the primary storage account and its Blob endpoint.
2. **The Key Posture:** Examine shared-key authentication and Role-Based Access Control (RBAC).
3. **The Network:** Verify the private endpoint and its IP address.
4. **The Secret:** Investigate how application secrets are stored in Azure Key Vault.
5. **The Loot:** Identify an exposed backup container accessible without authentication.

### Key Finding

The primary storage environment used multiple security controls, including identity-based access and private networking. However, a separate backup storage account publicly exposed data.

This demonstrated that securing the primary resource does not protect additional copies of the same data.

## Stop 1 — The Data

**Objective:** Identify the primary Azure Storage account and its Blob service endpoint.

**Resource Group:** `rg-madhat-lab-chap6`  
**Storage Account:** `stmadhatlabchap6`

### Investigation

1. Opened the storage account in Azure Portal.
2. Navigated to **Overview → JSON View**.
3. Located the Blob service endpoint.
4. Removed `https://` and the trailing `/` to isolate the hostname.

**Finding:** `stmadhatlabchap6.blob.core.windows.net`

### Screenshot Evidence

*Screenshot 01 — Azure Storage Account Overview / Blob Endpoint*

### Security Significance

Identifying the Blob endpoint establishes where the application's data is stored and provides the starting point for investigating authentication and network access.
