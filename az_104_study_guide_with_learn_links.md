# AZ-104: Microsoft Azure Administrator – In-Depth Self‑Study Guide (Intermediate)

> **Policy:** All hyperlinks point only to free **Microsoft Learn / Microsoft Docs** content. Verify the live AZ‑104 skills outline before scheduling (weights can change). Structured in the *same style* as the AZ‑500 guide for continuity.

---
## 0. Study Strategy & Mindset
**Goal:** Operational mastery of core Azure administration (identity, governance, storage, compute, networking, monitoring) with hands-on proficiency and troubleshooting clarity.

| Focus | What To Do | Linked References |
|-------|------------|------------------|
| Identity & Governance | Master RBAC vs Azure AD roles, Policy, Cost management | RBAC Overview: https://learn.microsoft.com/azure/role-based-access-control/overview • Identity Overview: https://learn.microsoft.com/entra/fundamentals/whatis |
| Storage Competence | Implement redundancy, access control (RBAC/SAS), lifecycle | Storage Intro: https://learn.microsoft.com/azure/storage/common/storage-introduction |
| Compute Administration | VM sizing, availability, scaling, images, updates, containers | VM Overview: https://learn.microsoft.com/azure/virtual-machines/overview |
| Networking | VNet design, subnets, NSGs, routing, name resolution, connectivity | VNet Overview: https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview |
| Monitoring & Maintenance | Azure Monitor, metrics/logs, alerts, backup, update mgmt | Monitor Overview: https://learn.microsoft.com/azure/azure-monitor/overview |
| Governance & Cost | Policy, Blueprints transition (Deployment Stacks), Cost mgmt | Policy Overview: https://learn.microsoft.com/azure/governance/policy/overview • Cost Mgmt: https://learn.microsoft.com/azure/cost-management-billing/cost-management-billing-overview |

**Suggested 4–5 Week Plan:**
- Week 1: Identity & Governance
- Week 2: Storage + Compute Fundamentals
- Week 3: Virtual Networking + Hybrid Connectivity
- Week 4: Monitoring, Backup/Recovery, Automation, Cost
- Week 5: Mixed Scenario Labs, Optimization, Practice Exam & Review

---
## 1. Manage Azure Identities & Governance
*(Skill weighting: ~15–20% – confirm current outline.)*

### 1.1 Microsoft Entra ID (Azure AD) Fundamentals
Tenants, users, groups (security vs M365), device identities, administrative units.
- Overview: https://learn.microsoft.com/entra/fundamentals/whatis
- Add Users: https://learn.microsoft.com/entra/identity/users/add-user

### 1.2 Azure AD Roles vs Azure RBAC
Different planes: Directory roles (tenant-wide objects) vs RBAC (resource scope). Use least privilege & role assignments at minimal necessary scope (subs → RG → resource).
- RBAC Overview: https://learn.microsoft.com/azure/role-based-access-control/overview
- Built-in Roles: https://learn.microsoft.com/azure/role-based-access-control/built-in-roles

### 1.3 Resource Organization (Management Groups, Subscriptions, Resource Groups)
Structure for policy inheritance & billing separation. Management groups form a hierarchy; root MG for global policy.
- Management Groups: https://learn.microsoft.com/azure/governance/management-groups/overview

### 1.4 Tags & Naming Conventions
Consistent naming (e.g., *prod-eus-vm-web01*) and tagging (CostCenter, Environment) enforced via Policy *Modify* effect.
- Tag Governance: https://learn.microsoft.com/azure/azure-resource-manager/management/tag-resources

### 1.5 Azure Policy & Initiatives
Enforce configuration (Deny), audit drifts (Audit), deploy missing settings (DeployIfNotExists), modify resources. Use initiatives for standards (Azure Security Benchmark, regulatory frameworks).
- Policy Effects: https://learn.microsoft.com/azure/governance/policy/concepts/effects
- Remediation: https://learn.microsoft.com/azure/governance/policy/how-to/remediate-resources

### 1.6 Role Assignments & Access Reviews (Basic Awareness)
Periodic access review (if using PIM or AAD governance SKU) to maintain least privilege (awareness; deeper PIM material is AZ‑500 oriented but basics apply).

### 1.7 Cost Management & Budgets
Budgets, alerts, cost analysis grouping by tags & resource types.
- Cost Management: https://learn.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices

### 1.8 Locking & Protection
Resource locks (CanNotDelete / ReadOnly) for critical infrastructure.
- Locks: https://learn.microsoft.com/azure/azure-resource-manager/management/lock-resources

**Practice:** Create MG hierarchy, assign initiative at MG, apply tag policy enforcing Environment tag, create budget & alert at subscription.

---
## 2. Implement & Manage Storage
*(Weighting: ~10–15%)*

### 2.1 Storage Account Types & Performance
Standard (GPv2), Premium (BlockBlob / File / Page), redundancy (LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS) selection based on durability & latency.
- Redundancy: https://learn.microsoft.com/azure/storage/common/storage-redundancy

### 2.2 Security & Access
Shared Key vs SAS (service/account/user delegation), Azure AD RBAC for Blob & Queue, network restrictions (Firewalls + Private Endpoints), immutability (WORM) for compliance.
- SAS Overview: https://learn.microsoft.com/azure/storage/common/storage-sas-overview
- Private Endpoint: https://learn.microsoft.com/azure/private-link/private-endpoint-overview
- Immutability: https://learn.microsoft.com/azure/storage/blobs/immutable-policy-configure-version-level-worm

### 2.3 Data Protection Features
Soft delete (blob, container, file share), versioning, point-in-time restore (blob), change feed & lifecycle management (tier transitions & deletion).
- Soft Delete: https://learn.microsoft.com/azure/storage/blobs/soft-delete-blob-overview
- Lifecycle Mgmt: https://learn.microsoft.com/azure/storage/blobs/lifecycle-management-overview

### 2.4 Azure Files & Azure File Sync
SMB shares, identity integration (AD DS / AAD Kerberos), tiering on Windows Servers via File Sync agent.
- Azure File Sync: https://learn.microsoft.com/azure/storage/file-sync/file-sync-deployment-guide

### 2.5 Encryption & Key Management
Service-side encryption (default), CMK via Key Vault, infrastructure encryption, customer-provided keys.
- Encryption: https://learn.microsoft.com/azure/storage/common/storage-service-encryption

### 2.6 Monitoring & Metrics
Capacity, transactions, latency, egress; enable logging (read/write/delete) to Log Analytics.
- Diagnostics: https://learn.microsoft.com/azure/storage/blobs/monitor-blob-storage

**Practice:** Deploy storage account with private endpoint, enable soft delete & versioning, create lifecycle rule moving logs to cool after 30 days.

---
## 3. Deploy & Manage Azure Compute
*(Weighting: ~25–30%)*

### 3.1 Virtual Machine Deployment Choices
Portal, CLI, ARM/Bicep, Cloud Shell. Reusable images (Shared Image Gallery) and Azure Compute Gallery replication.
- VM Quickstart: https://learn.microsoft.com/azure/virtual-machines/linux/quick-create-portal
- Compute Gallery: https://learn.microsoft.com/azure/virtual-machines/shared-image-galleries

### 3.2 VM Sizing & Availability
Size families (General D, Compute F, Memory E, Storage L, GPU NC/ND, Confidential, etc.). Availability sets (fault & update domains), Availability Zones, Proximity Placement Groups.
- Availability Options: https://learn.microsoft.com/azure/virtual-machines/availability

### 3.3 Scaling
VM Scale Sets (uniform vs flexible orchestration), autoscale rules, custom script & extensions.
- Scale Sets: https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview

### 3.4 OS & Data Disks
Managed disks (Standard HDD, Standard SSD, Premium SSD, Premium SSD v2, Ultra). Snapshots, disk encryption set, bursting.
- Managed Disks: https://learn.microsoft.com/azure/virtual-machines/managed-disks-overview

### 3.5 Images & Configuration
Custom images, Shared Image Gallery, Azure Image Builder.
- Image Builder: https://learn.microsoft.com/azure/virtual-machines/image-builder-overview

### 3.6 VM Extensions
Diagnostics, Custom Script, Desired State Configuration (Guest Configuration for policy alignment), Azure Defender agent.
- Extensions: https://learn.microsoft.com/azure/virtual-machines/extensions/overview

### 3.7 Containers & Container Instances (Administrator Perspective)
ACI for serverless containers, basic orchestrator awareness (AKS high-level). Administrators focus on deployment, networking, identity.
- ACI Overview: https://learn.microsoft.com/azure/container-instances/container-instances-overview

### 3.8 App Services (Admin Scope)
Plans (Free/Shared/Basic/Standard/Premium), deployment slots, scaling, backups, access restrictions, VNet integration, private endpoints.
- App Service Overview: https://learn.microsoft.com/azure/app-service/overview

### 3.9 Automation of Deployment
Bicep vs ARM templates, template specs, parameterization, idempotency.
- Bicep Overview: https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview

### 3.10 Update & Patch Management
Azure Update Manager (replacement evolution for Automation Update Management). Scheduling, compliance reports.
- Update Management: https://learn.microsoft.com/azure/update-center/overview

**Practice:** Create scale set w/ autoscale (CPU >70%), attach custom image from gallery, run update assessment.

---
## 4. Implement & Manage Virtual Networking
*(Weighting: ~30–35%)*

### 4.1 VNet Design & Addressing
CIDR planning, subnet purpose separation (App, Data, Gateway, Bastion). Avoid overlapping with on-prem (for future connectivity).
- VNets: https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview

### 4.2 Subnets, Service & Private Endpoints
Service endpoints (optimized route + firewall integration) vs private endpoints (private IP mapping). Choose private endpoints for strict isolation.
- Service Endpoints: https://learn.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
- Private Endpoint: https://learn.microsoft.com/azure/private-link/private-endpoint-overview

### 4.3 Name Resolution
Azure-provided DNS, custom DNS servers, Azure Private DNS zones (auto-registration for VMs, private endpoint records).
- Private DNS: https://learn.microsoft.com/azure/dns/private-dns-overview

### 4.4 Network Security Groups & Application Security Groups
Inbound/outbound rules, priority handling, effective security rules, ASGs for dynamic grouping.
- NSG Overview: https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview

### 4.5 Routing & User-Defined Routes (UDR)
System routes vs custom routes (forcing traffic through NVA / Firewall). 0.0.0.0/0 forced tunneling scenario.
- Routing: https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview

### 4.6 Peering & Hub-Spoke Architecture
Global VNet peering, gateway transit (shared VPN/ER gateway). Hub hosts shared services (Firewall, Bastion, DNS forwarders).
- VNet Peering: https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview

### 4.7 Load Balancing Options
Layer 4 Azure Load Balancer (public & internal), Application Gateway (Layer 7 + WAF), Azure Front Door (global HTTP/S), Traffic Manager (DNS-based). Choose based on scenario.
- Load Balancer: https://learn.microsoft.com/azure/load-balancer/load-balancer-overview
- Application Gateway: https://learn.microsoft.com/azure/application-gateway/overview

### 4.8 VPN Gateway & ExpressRoute
Site-to-Site (IPsec), Point-to-Site (OpenVPN/IKEv2), BGP, High-availability (active-active). ExpressRoute private dedicated circuit.
- VPN Gateway: https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
- ExpressRoute: https://learn.microsoft.com/azure/expressroute/expressroute-introduction

### 4.9 Bastion & Secure Remote Administration
Bastion host (no public IP on VMs). Just-in-time (Defender) if security extension considered (basic awareness).
- Bastion: https://learn.microsoft.com/azure/bastion/bastion-overview

### 4.10 Monitoring Network
Network Watcher (Topology, IP flow verify, Connection troubleshoot, Packet capture), NSG Flow Logs, metrics.
- Network Watcher: https://learn.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview

**Practice:** Build hub-spoke with peering, deploy Firewall or NVA (optional conceptual), implement private endpoint for storage, test DNS resolution.

---
## 5. Monitor & Maintain Azure Resources
*(Weighting: ~10–15%)*

### 5.1 Azure Monitor Architecture
Metrics (time-series), Logs (Kusto in Log Analytics), Data platforms (workspaces), ingestion control via Diagnostic settings & Data Collection Rules (AMA).
- Monitor Overview: https://learn.microsoft.com/azure/azure-monitor/overview

### 5.2 Log Analytics Workspace
Tables (Heartbeat, Perf, AzureActivity), retention settings, cost implications, workspace design (central vs per domain).
- Logs Overview: https://learn.microsoft.com/azure/azure-monitor/logs/log-query-overview

### 5.3 Metrics, Alerts & Action Groups
Metric alert (near real-time), log alert (scheduled query), action group (email, webhook, ITSM, automation Runbook / Logic App).
- Alerts: https://learn.microsoft.com/azure/azure-monitor/alerts/alerts-overview

### 5.4 Application Insights (Administrator Awareness)
Enable for App Service / Functions for end-to-end telemetry, availability tests.
- App Insights: https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview

### 5.5 Backup & Recovery
Recovery Services Vault, VM backup policy (frequency, retention), restore (full, file-level), soft delete.
- Azure VM Backup: https://learn.microsoft.com/azure/backup/backup-azure-vms-introduction

### 5.6 Site Recovery (Disaster Recovery Awareness)
Replicate VMs to another region, RPO/RTO, failover testing.
- Site Recovery: https://learn.microsoft.com/azure/site-recovery/site-recovery-overview

### 5.7 Update Management & Patching
Azure Update Manager scheduling, classification filters, compliance reporting.
- Update Center: https://learn.microsoft.com/azure/update-center/overview

### 5.8 Automation & Desired State
Automate tasks: Azure Automation (Runbooks, PS7), Desired State config alternatives (Guest Configuration Policy), Logic Apps for event-driven actions.
- Automation Overview: https://learn.microsoft.com/azure/automation/automation-intro

### 5.9 Health & Service Notifications
Service Health (planned maintenance, outages), Resource Health (per-resource availability status).
- Service Health: https://learn.microsoft.com/azure/service-health/service-health-overview

### 5.10 Cost Optimization Practices
Right-sizing VMs, reserved instances / savings plans, auto-shutdown dev/test, storage tiering, Advisor recommendations.
- Advisor: https://learn.microsoft.com/azure/advisor/advisor-overview

**Practice:** Configure VM backup, test file restore, create metric alert on CPU >80%, build workbook summarizing VM performance.

---
## 6. Scenario Mapping (Requirement → Control)
| Requirement | Control / Feature |
|-------------|------------------|
| Enforce tagging on all RG deployments | Azure Policy (Modify + Append) |
| Prevent accidental deletion of prod RG | Resource Lock (CanNotDelete) |
| Minimize admin standing privilege | Assign least privilege RBAC; temporary elevation (if PIM available) |
| Isolate backend services from Internet | Private Endpoints + NSG denies |
| Scale out web tier automatically | VM Scale Set / App Service autoscale |
| Reduce cross-region latency for images | Shared Image Gallery replication |
| Force encryption with customer-managed key | Disk Encryption Set + CMK |
| Migrate on-prem network securely | Site-to-Site VPN (later ExpressRoute) |
| Detect performance bottleneck on VM | Azure Monitor metrics / Insights |
| Restore accidentally deleted VM | VM Backup restore (Recovery Services Vault) |

---
## 7. Mini Labs Index (All Free Microsoft Docs)
| # | Lab Goal | Core Docs |
|---|----------|-----------|
| 1 | Create MG hierarchy + assign policy | https://learn.microsoft.com/azure/governance/policy/overview |
| 2 | Enforce tagging via Modify policy | https://learn.microsoft.com/azure/governance/policy/concepts/effects |
| 3 | Deploy storage w/ private endpoint & lifecycle | https://learn.microsoft.com/azure/storage/common/storage-network-security |
| 4 | Configure blob soft delete & versioning | https://learn.microsoft.com/azure/storage/blobs/soft-delete-blob-overview |
| 5 | VM w/ availability set + load balancer | https://learn.microsoft.com/azure/load-balancer/load-balancer-overview |
| 6 | Shared Image Gallery distribution | https://learn.microsoft.com/azure/virtual-machines/shared-image-galleries |
| 7 | VM Scale Set autoscale rule | https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview |
| 8 | Private DNS + private endpoint resolution | https://learn.microsoft.com/azure/dns/private-dns-overview |
| 9 | Hub-spoke peering + UDR route to NVA placeholder | https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview |
| 10 | Configure App Service VNet integration & slot swap | https://learn.microsoft.com/azure/app-service/networking-features |
| 11 | Log Analytics workspace + metric alert | https://learn.microsoft.com/azure/azure-monitor/overview |
| 12 | VM backup + file-level restore test | https://learn.microsoft.com/azure/backup/backup-azure-vms-introduction |
| 13 | Site Recovery enable + test failover (lab) | https://learn.microsoft.com/azure/site-recovery/site-recovery-overview |
| 14 | Bicep deployment of RG + storage + VM | https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview |
| 15 | Cost budget + alert + Advisor remediation | https://learn.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices |

---
## 8. Common Pitfalls & Avoidance
| Pitfall | Fix | Reference |
|---------|-----|-----------|
| Using classic deployment models | Always ARM/Bicep | https://learn.microsoft.com/azure/azure-resource-manager/management/overview |
| Over-privileged Owner on subscription | Granular RBAC roles | https://learn.microsoft.com/azure/role-based-access-control/built-in-roles |
| No tag governance | Policy Modify + audit untagged | https://learn.microsoft.com/azure/governance/policy/concepts/effects |
| Unplanned IP overlap | Early CIDR planning & documentation | https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview |
| Public storage endpoints open | Firewall + Private Endpoint | https://learn.microsoft.com/azure/storage/common/storage-network-security |
| Underutilized large VM size | Advisor right-size recommendation | https://learn.microsoft.com/azure/advisor/advisor-overview |
| Lack of backup verification | Schedule restore tests | https://learn.microsoft.com/azure/backup/backup-azure-vms-introduction |
| Missing autoscale | Implement metric-based scaling | https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview |
| Inconsistent configuration drift | Policy enforcement & remediation | https://learn.microsoft.com/azure/governance/policy/overview |
| Uncontrolled cost growth | Budgets + tagging + reserved instances | https://learn.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices |

---
## 9. Final 72‑Hour Checklist
- [ ] RBAC review: remove unnecessary Contributor/Owner roles
- [ ] Critical initiatives assigned & compliant
- [ ] Storage redundancy & protection verified (soft delete, versioning)
- [ ] VM availability constructs (Set/Zone) understood
- [ ] Scale set & autoscale rule tested
- [ ] Hub-spoke routing & private endpoint DNS resolution validated
- [ ] Backup restore drill completed
- [ ] Sample KQL queries executed (AzureActivity & Perf tables)
- [ ] Cost budget & alerts firing correctly
- [ ] Bicep deployment & parameterization rehearsed

**Memory Aid (Admin Chain):** *Organize & Govern → Secure & Store → Deploy & Scale → Connect & Segment → Observe & Recover → Optimize & Govern Again.*

---
## 10. Glossary (Selected)
| Term | Definition |
|------|------------|
| Management Group | Hierarchical container above subscriptions for governance |
| Initiative | Grouped Azure Policy definitions |
| RBAC | Role-Based Access Control for resource permissions |
| Resource Lock | Prevents delete (CanNotDelete) or write (ReadOnly) |
| LRS/ZRS/GRS | Local / Zone / Geo redundant storage replication |
| VM Scale Set | Group of VMs for elastic scaling |
| Availability Set | VM grouping across fault & update domains |
| UDR | User-Defined Route table overriding system routes |
| Peering | Direct VNet interconnection (low latency) |
| Private Endpoint | Private IP mapping to PaaS service |
| Log Analytics | Workspace for Azure Monitor logs (KQL) |
| Recovery Services Vault | Backup & DR metadata and storage container |
| Bicep | DSL for ARM template authoring |
| Azure Advisor | Recommendation engine for cost/perf/HA/security |
| Shared Image Gallery | Image distribution & versioning service |

---
## 11. Next Steps
1. Execute all 15 mini labs – document steps & issues.
2. Build personal *Scenario → Control* flash cards.
3. Attempt full-length practice exam; analyze misses.
4. Deep dive weak areas (especially networking & governance) with fresh labs.
5. Final day: review checklist; rest & maintain clarity.

**Success Metric:** You can state *what* to deploy, *why* it solves the requirement, and *how* to implement/verify – from memory.

Good luck – transform this reference into demonstrable operational skill!

