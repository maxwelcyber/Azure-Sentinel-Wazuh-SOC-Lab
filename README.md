![rbac_assignment](https://github.com/user-attachments/assets/fdc7fd49-14f0-4b4f-ba4a-b9e6469f7994)# 🛡️ Hybrid Cloud SOC & SIEM Lab (Azure + Wazuh)

## 📌 Project Overview
This project demonstrates the end-to-end deployment of a modern **Security Operations Center (SOC)**. By integrating **Microsoft Sentinel** (SIEM) with **Wazuh** (XDR), I created a centralized monitoring environment that captures both high-level cloud infrastructure changes and deep-level endpoint telemetry.

## 🏗️ Architecture & Implementation

### 1. Endpoint Detection & Response (XDR)
I deployed a **Wazuh Manager** on Ubuntu 22.04 and successfully enrolled a **Kali Linux** endpoint (Host: `bluey`). This setup provides real-time visibility into system calls, file integrity, and rootkit detection.

* **Key Achievement:** Successfully configured the Wazuh agent to communicate over a secure virtual network, ensuring consistent telemetry flow.
![wazuh_dashboard](https://github.com/user-attachments/assets/9ea9bf52-aa3d-4599-96fc-691362007af3)


### 2. SIEM & Infrastructure Auditing
Utilizing **Microsoft Sentinel**, I established a "Single Pane of Glass" for the cloud environment. By connecting the **Azure Activity Logs** connector, the SIEM automatically ingests administrative actions, providing a full audit trail of resource modifications.

* **Key Achievement:** Correlated platform-level changes (like patch installations) directly within the Sentinel Log Analytics workspace.
![log_telemetry](https://github.com/user-attachments/assets/8fb5b83e-4dbf-46d2-aa7d-c6d312204f3a)


### 3. Continuous Vulnerability Management
To maintain a hardened security posture, I implemented **Azure Update Manager**. This ensures the SOC infrastructure itself is not a point of failure by automating security patch assessments.

* **Capabilities:** Periodic assessments (AutomaticByPlatform) are enabled to flag critical security updates for the Ubuntu Manager.
![update_manager](https://github.com/user-attachments/assets/dbbaf24a-7d4d-4d29-9e01-2775dd87eb7e)


### 4. Identity & Governance (RBAC)
Following the **Principle of Least Privilege (PoLP)**, I managed a multi-user environment using **Azure Role-Based Access Control (RBAC)**. I assigned granular roles (Virtual Machine Contributor) to collaborators to ensure operational efficiency without compromising root-level security.
![rbac_assignment](https://github.com/user-attachments/assets/388f975b-08f8-479e-94f9-f0583bcaec96)



---

## 🔍 Log Investigation & KQL
A core component of this lab involved performing deep-dive investigations using **Kusto Query Language (KQL)**. I analyzed metadata to track the origin of administrative actions, including **Caller IP Addresses** and **Correlation IDs**.

**Example Audit Query:**
```kusto
AzureActivity
| where OperationNameValue has "VIRTUALMACHINES"
| project TimeGenerated, Caller, CallerIpAddress, OperationNameValue, ActivityStatusValue
| order by TimeGenerated desc<img width="1366" height="768" alt="Screenshot_2026-04-22_08_56_33" src="https://github.com/user-attachments/assets/a0bc38b7-7b76-48dd-83df-17493508639b" />
