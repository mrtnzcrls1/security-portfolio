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

The primary storage environment used multiple security controls, including identity-based access and private networking. However, a separate backup storage account exposed data publicly.

This demonstrated that securing the primary resource does not protect additional copies of the same data.
