# 🛡️ Cloud SOC Incident Report: SSH Brute Force Detection

## 1. Executive Summary
This report documents a simulated **Credential Access** attack against a cloud-hosted Ubuntu 22.04 server. The incident was detected through a multi-layered security stack involving **Wazuh (XDR)** and **Microsoft Sentinel (SIEM)**. The investigation confirmed a brute-force attempt originating from a known testing endpoint (`bluey`).

## 2. Attack Lifecycle & Methodology
The attack followed the **MITRE ATT&CK Framework** technique **T1110 (Brute Force)**.

### Phase 1: Offensive Execution
The attacker utilized a Kali Linux instance to perform multiple rapid SSH login attempts using an invalid username to trigger authentication failure logs.

<img width="726" height="515" alt="Terminal" src="https://github.com/user-attachments/assets/0c435bb1-49d2-4d51-ab05-51af3d781b18" />

---

## 3. Detection & Telemetry Analysis

### XDR Layer: Wazuh
The Wazuh agent installed on the Ubuntu manager captured the real-time telemetry. It successfully flagged the high-frequency failed logins and mapped them to **MITRE T1110.001**.

<img width="1366" height="651" alt="DashboardWa" src="https://github.com/user-attachments/assets/1be99242-a1a3-41f0-b4ad-77824ade1c5f" />

### SIEM Layer: Microsoft Sentinel (KQL)
Logs were ingested into the Azure Log Analytics Workspace via the **Azure Monitor Agent (AMA)**. I utilized **Kusto Query Language (KQL)** to verify the ingestion of these logs and to isolate the attacker's IP address.

```kusto
Syslog
| where ProcessName == "sshd"
| where SyslogMessage has "Failed password"
```
<img width="1365" height="682" alt="AzureDefender" src="https://github.com/user-attachments/assets/d89687fe-2de0-4a32-b3f1-606a9f752a67" />
