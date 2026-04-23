<img width="1366" height="668" alt="IMG_1061" src="https://github.com/user-attachments/assets/9734b9d3-f0d1-4455-aba1-d33696190e76" />

# 🛡️ Hybrid Cloud SOC & SIEM Lab (Azure + Wazuh)

## 📌 Project Overview
This project demonstrates the end-to-end deployment of a modern **Security Operations Center (SOC)**. By integrating **Microsoft Sentinel** (SIEM) with **Wazuh** (XDR), I created a centralized monitoring environment that captures both high-level cloud infrastructure changes and deep-level endpoint telemetry.

## 🏗️ Architecture & Implementation

### 1. Endpoint Detection & Response (XDR)
I deployed a **Wazuh Manager** on Ubuntu 22.04 and successfully enrolled a **Kali Linux** endpoint (Host: `bluey`). This setup provides real-time visibility into system calls, file integrity, and rootkit detection.

* **Key Achievement:** Successfully configured the Wazuh agent to communicate over a secure virtual network, ensuring consistent telemetry flow.

### 2. SIEM & Infrastructure Auditing
Utilizing **Microsoft Sentinel**, I established a "Single Pane of Glass" for the cloud environment. By connecting the **Azure Activity Logs** connector, the SIEM automatically ingests administrative actions, providing a full audit trail of resource modifications.

* **Key Achievement:** Correlated platform-level changes (like patch installations) directly within the Sentinel Log Analytics workspace.


### 3. Continuous Vulnerability Management
To maintain a hardened security posture, I implemented **Azure Update Manager**. This ensures the SOC infrastructure itself is not a point of failure by automating security patch assessments.

* **Capabilities:** Periodic assessments (AutomaticByPlatform) are enabled to flag critical security updates for the Ubuntu Manager.


### 4. Identity & Governance (RBAC)
Following the **Principle of Least Privilege (PoLP)**, I managed a multi-user environment using **Azure Role-Based Access Control (RBAC)**. I assigned granular roles (Virtual Machine Contributor) to collaborators to ensure operational efficiency without compromising root-level security.


---

## 🔍 Log Investigation & KQL
A core component of this lab involved performing deep-dive investigations using **Kusto Query Language (KQL)**. I analyzed metadata to track the origin of administrative actions, including **Caller IP Addresses** and **Correlation IDs**.

**Example Audit Query:**
```kusto
AzureActivity
| where OperationNameValue has "VIRTUALMACHINES"
| project TimeGenerated, Caller, CallerIpAddress, OperationNameValue, ActivityStatusValue
| order by TimeGenerated desc<img width="1366" height="768" alt="Screenshot_2026-04-22_08_56_33" src="https://github.com/user-attachments/assets/a0bc38b7-7b76-48dd-83df-17493508639b" />
