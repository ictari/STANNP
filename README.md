# CIS Controls Audit Mapping for Stannp

This document maps **CIS Critical Security Controls (v8)** to Stannp’s current Azure infrastructure audit. For each control, current **Findings** are noted where assessed, and relevant **Microsoft best-practice links** are provided. Controls not yet assessed are left blank for future work.

---

## CIS Control 1: Inventory and Control of Enterprise Assets
**Findings:**
- VM inventory extracted from Azure (14 VMs identified).
- Inconsistent naming conventions across resources (mix of functional names and themed names).
- One VM located outside primary region (North Europe).

**Microsoft References:**
- [Azure resource naming conventions](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming)
- [Resource tagging best practices](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources)

---

## CIS Control 2: Inventory and Control of Software Assets
**Findings:**
- VM Extensions inconsistent (Azure Policy, Defender, OMSAgent, Key Vault agent).
- No baseline to ensure consistent monitoring and security agents across VMs.

**Microsoft References:**
- [Azure Policy overview](https://learn.microsoft.com/en-us/azure/governance/policy/overview)
- [Defender for Cloud monitoring agents](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-servers-introduction)

---

## CIS Control 3: Data Protection
**Findings:**
- Disk encryption at rest confirmed on some VMs, not yet verified for all.
- Key Vaults in use, but inconsistent deployment of KeyVaultForLinux extension.

**Microsoft References:**
- [Encryption of Azure resources](https://learn.microsoft.com/en-us/azure/security/fundamentals/encryption-overview)
- [Azure Key Vault best practices](https://learn.microsoft.com/en-us/azure/key-vault/general/best-practices)

---

## CIS Control 4: Secure Configuration of Enterprise Assets and Software
**Findings:**
- Shared Image Gallery exists, but patch/update consistency of VM images not confirmed.
- No central baseline configuration policy detected.

**Microsoft References:**
- [Azure Compute Gallery (Shared Image Gallery)](https://learn.microsoft.com/en-us/azure/virtual-machines/shared-image-galleries)
- [Azure Security Benchmark](https://learn.microsoft.com/en-us/security/benchmark/azure/overview)

---

## CIS Control 5: Account Management
**Findings:**
- 5 Global Administrators in Entra, including non-technical staff.
- 7 Subscription Owners, assigned directly to individuals, not groups.
- Break-glass accounts not yet created.

**Microsoft References:**
- [Emergency access (break-glass) accounts](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access)
- [Best practices for admin accounts](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/best-practices)

---

## CIS Control 6: Access Control Management
**Findings:**
- Excessive permanent privileged roles (Global Admin, Subscription Owner).
- No use of role-assignable groups.
- No Privileged Identity Management (Entra Free).

**Microsoft References:**
- [Least privilege access](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/least-privilege)
- [Azure RBAC best practices](https://learn.microsoft.com/en-us/azure/role-based-access-control/best-practices)

---

## CIS Control 7: Continuous Vulnerability Management
**Findings:**
- VM OS patch status not yet audited.
- Defender for Cloud deployment incomplete across VMs.

**Microsoft References:**
- [Defender for Cloud – Recommendations](https://learn.microsoft.com/en-us/azure/defender-for-cloud/recommendations-reference)
- [Update management](https://learn.microsoft.com/en-us/azure/automation/update-management/overview)

---

## CIS Control 8: Audit Log Management
**Findings:**
- Subscription Activity Logs present, but not exported for long-term retention.
- No alerts configured for RBAC role changes (e.g. new Global Admin or Owner).

**Microsoft References:**
- [Azure Monitor Activity Log](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/activity-log)
- [Monitor role assignments](https://learn.microsoft.com/en-us/azure/role-based-access-control/change-history-report)

---

## CIS Control 11: Data Recovery
**Findings:**
- Snapshots and Restore Point Collections present but appear ad hoc.
- Recovery Services Vault exists but unclear if all critical VMs enrolled.

**Microsoft References:**
- [Azure Backup best practices](https://learn.microsoft.com/en-us/azure/backup/backup-azure-best-practices)

---

## CIS Control 12: Network Infrastructure Management
**Findings:**
- NSGs deployed but rule review pending.
- Some VMs may have public IPs assigned directly.
- Private Endpoints and WAF policies exist.

**Microsoft References:**
- [NSG best practices](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [Azure Front Door WAF](https://learn.microsoft.com/en-us/azure/web-application-firewall/afds/afds-overview)

---

## CIS Control 13: Network Monitoring and Defense
**Findings:**
- Monitoring agents deployed inconsistently across VMs.
- No centralised NSG flow log or traffic analytics reviewed.

**Microsoft References:**
- [NSG flow logs and traffic analytics](https://learn.microsoft.com/en-us/azure/network-watcher/traffic-analytics)

---

## CIS Control 14: Security Awareness and Skills Training
**Findings:**
- Non-technical staff currently hold GA/Owner roles → indicates lack of awareness of risks tied to privileged roles.

**Microsoft References:**
- [Microsoft Security Awareness Training](https://learn.microsoft.com/en-us/security/cybersecurity-awareness-training/training-resources)

---

## CIS Control 15: Service Provider Management
**Findings:**
- External integrations (e.g. Sentry, Mailgun) in use, review of third-party permissions pending.

**Microsoft References:**
- [Manage third-party app access](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/manage-apps)

---

## CIS Control 16: Application Software Security
**Findings:**
- App Service and Logic Apps in use, inconsistent enforcement of secure connections (TLS 1.2+).
- Secrets management partially implemented with Key Vault.

**Microsoft References:**
- [App Service best practices](https://learn.microsoft.com/en-us/azure/app-service/app-service-best-practices)

---

## CIS Control 17: Incident Response Management
**Findings:**
- No documented incident response plan confirmed.
- No alerting for privileged role usage or break-glass account use.

**Microsoft References:**
- [Azure incident response reference guide](https://learn.microsoft.com/en-us/security/incident-response/incident-response-playbook-overview)

---

## CIS Control 18: Penetration Testing
**Findings:**
- Not yet reviewed.

**Microsoft References:**
- [Penetration testing in Azure](https://learn.microsoft.com/en-us/azure/security/fundamentals/pen-testing)

---

# Notes
- Controls **9 (Email/Web), 10 (Malware Defenses)** are out of current scope (cloud infra only) but could be reviewed later.

---

# Optional Improvements (Licensing / Paid Features)

These improvements require licensing or financial decisions and are **not mandatory recommendations**, but are highlighted for awareness:

- **Entra ID P1/P2**
  - Privileged Identity Management (PIM) → just-in-time admin rights, MFA & approval for role activation.
  - Access Reviews → periodic revalidation of privileged access.
  - Conditional Access → enforce MFA, block legacy auth.
  - Ref: [Privileged Identity Management](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-configure)

- **Microsoft Defender for Cloud P2**
  - Advanced security posture management, regulatory compliance dashboard, just-in-time VM access.
  - Ref: [Defender for Cloud overview](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)

- **Azure Monitor Premium / Log Analytics Retention**
  - Extended retention of activity and audit logs beyond 90 days.
  - Enables long-term compliance, investigations, and historical trending.
  - Ref: [Azure Monitor log data retention](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/data-retention-archive)

- **Azure Policy Guest Configuration Add-on**
  - Advanced guest OS configuration compliance.
  - Ref: [Azure Policy Guest Configuration](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/guest-configuration)

---

This section ensures Stannp is aware of stronger governance and monitoring options available with paid Azure/Entra features.

