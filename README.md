# Portfolio Project: Healthcare GRC & Compliance Framework (TeleHealth Connect)

## Project Overview
This project establishes a comprehensive Governance, Risk, and Compliance (GRC) framework for **TeleHealth Connect**, a fictional digital health startup that matches patients with licensed therapists via secure video consultations. 

As a cloud-hosted platform managing sensitive patient data, TeleHealth Connect must strictly comply with the **Health Insurance Portability and Accountability Act (HIPAA)**. This framework defines the administrative, technical, and operational controls required to secure Protected Health Information (PHI) and mitigate enterprise risk, alongside the active technical detection logic required to defend it.

## Repository Structure
The compliance and security artifacts are organized into four core domains:
*   📁 `01_Policies_and_Controls/`: Contains the HIPAA Security Rule Matrix and the operational Incident Response Policy.
*   📁 `02_Risk_Assessment/`: Houses the Security Risk Assessment (SRA) identifying core technical vulnerabilities.
*   📁 `03_Vendor_Management/`: Contains the Third-Party Vendor Risk Matrix and sample Business Associate Agreement (BAA) provisions.
*   📁 `04_SOC_Detections_and_Logs/`: Contains SIEM detection logic (KQL) and Identity Threat Detection and Response (ITDR) workflows.

## Key Compliance & Security Artifacts Delivered

### 1. HIPAA Control Mapping (§ 164.308 & § 164.312)
Designed a compliance matrix implementing essential safeguards, including:
*   **Technical:** Automated session logoffs after 15 minutes of inactivity and AES-256 bit encryption for data at rest and in transit.
*   **Administrative:** Implementing Role-Based Access Control (RBAC) to ensure medical notes are restricted strictly to authorized clinicians.

### 2. Security Risk Assessment (SRA)
Performed a simulated risk assessment based on HHS standards. Identified, scored, and mitigated top infrastructure risks:
*   **Phishing & Credential Staining (Risk Score: 16):** Mitigated by enforcing mandatory multi-factor authentication (MFA) across all staff accounts.
*   **Cloud Misconfigurations (Risk Score: 15):** Mitigated by deploying continuous configuration monitoring to prevent public database exposure.

### 3. Incident Response Policy
Drafted a formal, 4-stage Incident Response Policy incorporating the **HIPAA Breach Notification Rule**. The policy establishes strict containment strategies and defines the legal notification timeline (e.g., notifying the Department of Health and Human Services and media outlets within 60 days if a breach impacts 500+ individuals).

### 4. Third-Party Vendor Risk Management
Evaluated external infrastructure dependencies to enforce compliance boundaries:
*   Executed **Business Associate Agreements (BAAs)** for core data processors handling PHI (**AWS** and **Twilio**).
*   Correctly isolated financial processors (**Stripe**) under **PCI-DSS compliance boundaries**, optimizing legal resources.

### 5. Threat Detection & ITDR (SOC Operations)
Developed active SIEM detection logic to defend against the highest-priority risks identified in the SRA:
*   **AiTM Phishing Detection:** Wrote custom Microsoft Sentinel queries (KQL) correlating `SigninLogs` and `AuditLogs` to detect session hijacking and malicious inbox forwarding rules.
*   **Containment Workflows:** Defined step-by-step ITDR containment procedures for credential compromise, including Entra ID session revocation and automated rule wiping.

## Skills Demonstrated
*   HIPAA Security & Privacy Rule Mapping
*   Healthcare Risk Identification & Mitigation (NIST SP 800-30)
*   SIEM Log Analysis & Threat Hunting (KQL / Microsoft Sentinel)
*   Identity Threat Detection & Response (ITDR)
*   Third-Party Risk Management (TPRM) & Vendor Vetting
