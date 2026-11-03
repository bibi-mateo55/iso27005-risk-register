# ISO 27005 Risk Register — CloudNova (fictional SaaS SME)

A practical information-security **risk register** built with the **ISO/IEC 27005** methodology for a fictional organisation. This is a portfolio project to demonstrate GRC skills: identifying threats and vulnerabilities, scoring risk, and recommending treatments mapped to controls.

> ⚠️ **CloudNova is fictional and all data is invented** for learning purposes. No real company or data is represented.

---

## 🏢 Context

**CloudNova** is a European SME (~45 employees) offering a **B2B project-management SaaS platform**, hosted on AWS in a multi-tenant architecture. It processes customers' personal data and documents, which makes it subject to **GDPR** and in scope of **NIS2** as a digital service provider.

**Critical assets:** customer data (PII and documents), the multi-tenant application, the AWS cloud infrastructure, privileged credentials, and the CI/CD pipeline.

---

## 📐 Methodology (ISO/IEC 27005)

Each risk is scored as **Risk level = Probability × Impact**, both on a 1–5 scale.

| Band (P×I) | Classification | Recommended action |
|---|---|---|
| 1–6 | 🟢 Low | Accept or basic controls |
| 7–12 | 🟡 Medium | Mitigate in a medium-term plan |
| 13–19 | 🟠 High | Mitigate as a priority |
| 20–25 | 🔴 Critical | Immediate action |

The full scales (probability and impact definitions) and a risk heat-map are in the [Excel register](CloudNova-registro-riesgos.xlsx).

---

## 📋 Risk register

| ID | Threat | Vulnerability | Asset | P | I | Level | Class | Treatment | Proposed control |
|---|---|---|---|:-:|:-:|:-:|---|---|---|
| R-01 | Phishing / theft of cloud admin credentials | No MFA on root/admin AWS account; weak privileged access | AWS infrastructure | 4 | 5 | **20** | 🔴 Critical | Mitigate | Mandatory MFA on privileged accounts; PAM; anomalous-login alerts |
| R-02 | Customer data leak via misconfigured storage bucket | Public, unencrypted S3 bucket; no config scanning | Customer data (PII) | 3 | 5 | **15** | 🟠 High | Mitigate | Restrictive bucket policies; encryption at rest; CSPM |
| R-03 | Exploitation of a web vulnerability (e.g. SQL injection) | No secure-coding practices; no SAST/DAST | SaaS application | 3 | 5 | **15** | 🟠 High | Mitigate | SAST/DAST in pipeline; code review; WAF |
| R-04 | Ransomware on internal systems | Unpatched endpoints, no EDR; non-isolated backups | Internal IT & backups | 3 | 4 | **12** | 🟡 Medium | Mitigate | EDR; patch management; immutable/offline backups |
| R-05 | Compromised third-party dependency (supply chain) | No SCA or SBOM | SaaS application | 3 | 4 | **12** | 🟡 Medium | Mitigate | Automated SCA; pinned versions; maintain an SBOM |
| R-06 | GDPR non-compliance (rights, breach notification) | No DPO or formal privacy processes | Legal & reputation | 3 | 4 | **12** | 🟡 Medium | Mitigate | Privacy programme; DPIA; 72h breach procedure |
| R-07 | Over-permissive cloud IAM roles | No least-privilege in AWS | AWS infra & data | 3 | 4 | **12** | 🟡 Medium | Mitigate | Periodic IAM review; least privilege; IaC scanning |
| R-08 | Tenant isolation failure (one customer sees another's data) | Multi-tenant design flaw | Customer data | 2 | 5 | **10** | 🟡 Medium | Mitigate | Isolation testing; row-level security; logical segregation |
| R-09 | Unable to restore backups | Backups never tested; no RPO/RTO | Customer data / availability | 2 | 5 | **10** | 🟡 Medium | Mitigate | Regular restore tests; define and measure RPO/RTO |
| R-10 | Denial-of-service (DDoS) attack | No anti-DDoS protection or autoscaling | Service availability | 3 | 3 | **9** | 🟡 Medium | Mitigate | CDN with DDoS protection; autoscaling; response plan |
| R-11 | Key-person dependency (single admin) | High bus factor; lack of documentation | Operations | 3 | 3 | **9** | 🟡 Medium | Mitigate | Document procedures; cross-training; second admin |
| R-12 | Customer account takeover | Weak password policy; no MFA option | Customer accounts | 3 | 3 | **9** | 🟡 Medium | Mitigate | Offer MFA; password policy; access-anomaly detection |
| R-13 | Insider threat: departing employee exfiltrates data | Excessive access; no offboarding process | Customer data / IP | 2 | 4 | **8** | 🟡 Medium | Mitigate | Least privilege; offboarding checklist; basic DLP |
| R-14 | Prolonged cloud provider outage (AWS region) | Single-region/zone deployment | Service availability | 2 | 4 | **8** | 🟡 Medium | Transfer | Multi-AZ; disaster-recovery plan; provider SLA |
| R-15 | Shadow IT / unsanctioned SaaS | No asset inventory or app control | Company data | 3 | 2 | **6** | 🟢 Low | Mitigate | Asset inventory; usage policy; CASB-style control |
| R-16 | Laptop theft or loss | Unencrypted disks; no device management | Endpoint data | 2 | 3 | **6** | 🟢 Low | Mitigate | Full-disk encryption; MDM; remote wipe |

---

## 🎯 Top 3 priorities

1. **R-01 — Enforce MFA and privileged access management (score 20, Critical).** A single admin credential protects the entire AWS estate. Enforcing MFA on all privileged accounts and adding a PAM solution is the highest-impact, lowest-cost action and should happen first.
2. **R-02 — Secure cloud storage and add configuration scanning (score 15, High).** A misconfigured bucket is one of the most common causes of mass data breaches. Enforce encryption and restrictive policies, and deploy continuous configuration scanning (CSPM) to catch drift automatically.
3. **R-03 — Build secure development into the pipeline (score 15, High).** As the product *is* the application, an app vulnerability is an existential risk. Add SAST/DAST and mandatory code review so flaws are caught before release, backed by a WAF as a safety net.

---

## 📌 Assumptions

- CloudNova is pre-certification: no ISO 27001 ISMS or formal risk process exists yet (which is why this register is the starting point).
- Probability reflects a small company with limited security maturity and a cloud-native stack.
- Impact is rated against the business: customer-data confidentiality and service availability are the crown jewels.
- The register is a point-in-time assessment; it should be reviewed at least annually and after any major change.

---

## 🔗 Related

- Companion project: **NIS2 / ISO 27001 gap analysis** for the same organisation *(coming soon)*.
