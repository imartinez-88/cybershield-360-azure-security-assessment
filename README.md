# 🛡️ CyberShield-360 — Azure Security Assessment

> Enterprise cloud security assessment of Microsoft Azure — live environment, real KQL logs, nation-state breach analysis.

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![NIST](https://img.shields.io/badge/NIST_SP_800--171-grey?style=flat)
![FedRAMP](https://img.shields.io/badge/FedRAMP-Aligned-blue?style=flat)
![STRIDE](https://img.shields.io/badge/Threat_Model-STRIDE-red?style=flat)

---

## 📋 Overview

60-page security assessment covering Azure Key Vault architecture, NIST SP 800-171 gap analysis, STRIDE threat modeling, and a full root cause analysis of the **2024 Midnight Blizzard (APT29) breach**. Built on a real Azure for Students environment — not a simulation.

📄 **[View Full Report (PDF)](./Microsoft_Azure_Security_Assessment.pdf)**

---

## 🔍 What's Inside

| Section | Description |
|---|---|
| **Risk Assessment** | NIST SP 800-30 vs OCTAVE Allegro comparison for large-scale cloud environments |
| **Gap Analysis** | NIST SP 800-171 Rev.2 — AC, IA, SC, AU control families mapped against Azure Key Vault |
| **Threat Modeling** | STRIDE analysis + OWASP Threat Dragon DFD of Azure Key Vault auth workflow |
| **Breach Analysis** | APT29 / Midnight Blizzard root cause — password spraying → OAuth token abuse → lateral movement |
| **Live Hardening** | Real Azure environment: RBAC, diagnostic logging, KQL monitoring, MFA enforcement |
| **Web App Scan** | HTTP security header assessment of Microsoft public-facing applications |

---

## Live Azure Environment

Hardening performed on a real provisioned Azure environment:

```
Resource Group:        cybershield-kv
Key Vault:             test-api-key
Log Analytics:         CyberSH-law
Subscription:          Azure for Students
```

**RBAC principals configured:**
- `Isaac Martinez` — Owner + Key Vault Administrator (inherited)
- `IsaacMartinez@MartinezIsaac` — Key Vault Reader scoped to single secret (external user + MFA)
- `cybershield-app` — Key Vault Secrets User (service principal)

**KQL query used for live monitoring:**
```kql
AzureDiagnostics
| where ResourceType == "VAULTS"
| take 10
```

Operations verified over 24-hour window: `SecretGet` · `SecretList` · `VaultGet` · `SecretListVersions` · `SecretResourceGet`

---

##  STRIDE Threat Model

| Category | Component | Threat |
|---|---|---|
| **Spoofing** | API / Identity Flow | OAuth token forgery and JWT replay attacks |
| **Tampering** | Azure Key Vault | Unauthorized rotation/deletion of KEK and DEK material |
| **Repudiation** | Azure Monitor | Audit log manipulation to conceal access activity |
| **Info Disclosure** | Azure Key Vault | Secret exposure via misconfigured policies |
| **Denial of Service** | Key Vault Endpoint | Endpoint flooding to disrupt availability |
| **Elevation of Privilege** | Azure Entra ID | Privilege escalation via misconfigured AD policies |

---

##  Midnight Blizzard Breach — Attack Chain

```
Password spraying against legacy test accounts (no MFA)
        ↓
Entra ID manipulation — attacker adds own credentials to compromised accounts
        ↓
Custom OAuth POST requests → authenticated token generation
        ↓
full_access_as_app Exchange permission → corporate mailbox access
        ↓
Senior leadership email accounts compromised
```

Same vector used against HPE in 2023. Findings fed directly into hardening steps.

---

##  Identified Gaps & Remediation

| Risk | Gap | Timeline |
|---|---|---|
| 🔴 HIGH | HSM-backed key storage not mandated — software tiers fail FIPS 140-2 | 0–30 days |
| 🔴 HIGH | RBAC least privilege inconsistent across contractor tenants | 0–30 days |
| 🔴 HIGH | Audit log retention not standardized across Monitor/Sentinel | 30–60 days |
| 🟡 MEDIUM | MFA not enforced via Conditional Access on all accounts | 30–60 days |
| 🟡 MEDIUM | No automated Key Vault access recertification reviews | 60–90 days |

---

##  Tech Stack

`Microsoft Azure` `Azure Key Vault` `Microsoft Entra ID` `Azure Monitor` `Microsoft Sentinel` `Azure Policy` `Microsoft Defender for Cloud` `OWASP Threat Dragon` `KQL` `FIPS 140-2`

---

## Screenshots

See [`/screenshots`](./screenshots) — KQL output, IAM role assignments, STRIDE threat results, OWASP Threat Dragon DFD, Log Analytics diagnostic ingestion.

---

##  Author

**Isaac Martinez** — [@imartinez-88](https://github.com/imartinez-88)
