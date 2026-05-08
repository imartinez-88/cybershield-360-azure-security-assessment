# **CyberShield-360 Azure Security Assessment**
Enterprise-grade cloud security assessment of Microsoft Azure — built on a live Azure environment with real KQL audit logging, RBAC enforcement, STRIDE threat modeling, and a root cause analysis of the 2024 Midnight Blizzard nation-state breach. Produced as part of a comprehensive 60-page security report covering the full lifecycle from risk methodology to incident response.

# **What This Project Actually Does**
Most security portfolios describe Azure. This one uses it.
A real Azure for Students environment was provisioned from scratch — resource group isolation, Key Vault deployment, multi-principal RBAC configuration, diagnostic pipeline setup, and a 24-hour live monitoring window using KQL queries against Azure Diagnostics. Every finding in the gap analysis maps directly to a hardening step that was implemented and verified.

# Scope
Target: Microsoft Corporation — Azure Key Vault security architecture, identity governance, and NIST SP 800-171 compliance posture for government contractor tenants
Framework Coverage: NIST SP 800-30 · NIST SP 800-171 Rev.2 · FedRAMP · FIPS 140-2 · STRIDE · OWASP Threat Dragon

# Key Areas
**Risk Assessment & Methodology**
Compared NIST SP 800-30 against OCTAVE Allegro across four evaluation dimensions — technical depth, scalability, regulatory alignment, and resource overhead — to determine the appropriate framework for a large-scale cloud environment handling CUI. NIST SP 800-30 selected and justified against Microsoft's operational complexity and FedRAMP authorization requirements.
NIST SP 800-171 Gap Analysis
Evaluated four control families (AC, IA, SC, AU) against Microsoft's Azure Key Vault implementation. Identified gaps across contractor tenant configurations including inconsistent HSM-backed key storage, absence of standardized RBAC templates, non-enforced MFA across legacy accounts, and unstandardized audit log retention. Mapped each gap to a prioritized remediation roadmap with 0–90 day timelines.
Identified Gaps:

HSM-backed key storage not mandated for CUI tenants — software-protected tiers do not satisfy FIPS 140-2
RBAC least privilege enforcement inconsistent across contractor tenants
Audit log retention periods not standardized; no baseline 365-day policy enforced
Key Vault access permission reviews rely entirely on manual customer process

# Threat Modeling (STRIDE + OWASP Threat Dragon)
Built a full data flow diagram of the Azure Key Vault authentication and access system using Yourdon and Coad notation. Modeled five components across the Azure Cloud Trust Boundary: external User/VM, Azure Entra ID, Azure Key Vault, Azure Monitor/Sentinel, and the API/identity flow layer.
STRIDE CategoryComponentThreatSpoofingAPI / Identity FlowOAuth token forgery and JWT replay attacks against Azure API ManagementTamperingAzure Key VaultUnauthorized rotation or deletion of cryptographic keys and KEK/DEK materialRepudiationAzure Monitor/SentinelAudit log manipulation to conceal unauthorized access activityInformation DisclosureAzure Key VaultSecret exposure via misconfigured access policies or compromised identitiesDenial of ServiceAPI / Key Vault EndpointEndpoint flooding to disrupt Key Vault availabilityElevation of PrivilegeAzure Entra IDPrivilege escalation via misconfigured Active Directory policies
Midnight Blizzard Breach Analysis (APT29 / NOBELIUM)
Full root cause analysis of Microsoft's January 2024 breach by Russian SVR-backed threat group Midnight Blizzard. Traced the full attack chain:

Password spraying against legacy test accounts lacking MFA
Microsoft Entra ID manipulation to add attacker credentials to compromised accounts
Custom OAuth POST requests to generate authenticated tokens
Lateral movement into corporate email systems via full_access_as_app Exchange permission scope

Cross-referenced with the 2023 Hewlett Packard Enterprise breach (same threat actor, same initial vector) to identify the pattern. Findings directly informed the RBAC hardening, MFA enforcement, and audit logging steps performed in Section 6.
Live Azure Hardening Demonstration
Hardening performed directly inside Microsoft Azure on a real provisioned environment (cybershield-kv resource group, test-api-key Key Vault, CyberSH-law Log Analytics Workspace).
Steps Implemented:

Resource Group Isolation — Scoped all IAM policies, diagnostic settings, and access controls to a dedicated resource group, eliminating subscription-level inheritance risks
Multi-Principal RBAC Configuration — Four distinct role assignments demonstrating separation of duties: Owner (admin), Key Vault Administrator (admin), Key Vault Reader scoped to single secret (external user), Key Vault Secrets User (service principal)
Diagnostic Logging Pipeline — Configured read/write/delete operation capture (GetSecret, SecretSet, SecretDelete) routed to Log Analytics Workspace, satisfying NIST SP 800-171 AU control requirements
KQL Live Query Monitoring — Executed AzureDiagnostics | where ResourceType == "VAULTS" | take 10 against active Key Vault data plane. Verified SecretGet, SecretList, VaultGet, and SecretListVersions operations with ResultType: Success across 24-hour window

# Web Application Security Scan
HTTP security header assessment of Microsoft's public-facing web applications using header scanner grading (A+ to F). Evaluated CSP, HSTS, X-Frame-Options, X-XSS-Protection, Referrer-Policy, and Permissions-Policy configurations against recommended hardening baselines.

# Live Environment
ResourceValueResource Groupcybershield-kvKey Vaulttest-api-keyLog Analytics WorkspaceCyberSH-lawSubscriptionAzure for StudentsEndpointmacOS Sonoma 14.5 · Apple M1 · FileVault enabled

# Technologies & Platforms
Microsoft Azure · Azure Key Vault · Microsoft Entra ID · Azure Monitor · Microsoft Sentinel · Azure Policy · Microsoft Defender for Cloud · OWASP Threat Dragon · Kusto Query Language (KQL) · FIPS 140-2 HSM

**Screenshots**

/screenshots — KQL query output, IAM role assignments, OWASP Threat Dragon DFD, STRIDE threat results, Log Analytics workspace diagnostic ingestion, macOS audit log output


# Full Report
The complete 60-page assessment is available in Microsoft_Azure_Security_Assessment.pdf, covering all sections in full detail including data classification schema, compliance gap tables, STRIDE manual threat inputs, and the complete remediation roadmap.
