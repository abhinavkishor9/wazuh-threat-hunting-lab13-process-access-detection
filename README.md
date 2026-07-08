# wazuh-threat-hunting-lab13-process-access-detection
## Overview

This lab demonstrates how to detect and investigate Windows Process Access activity using Sysmon Event ID 10 and Wazuh SIEM.

Process Access events occur when one process attempts to open or interact with another process. While many of these events are legitimate, attackers commonly abuse this behavior to access sensitive processes such as **lsass.exe** for credential dumping.

The objective of this lab is to generate Process Access activity, verify Sysmon logging, investigate the event in Wazuh, and understand how SOC analysts identify potentially malicious process interactions.

---

## Lab Objectives

- Verify Sysmon and Wazuh services
- Generate Process Access activity
- Capture Sysmon Event ID 10
- Investigate Process Access events in Wazuh
- Analyze source and target processes
- Review GrantedAccess permissions
- Map findings to MITRE ATT&CK

---

## Lab Environment

Host Machine

- Windows 11

Monitoring

- Sysmon
- Wazuh Agent
- Wazuh Manager

Tools

- PowerShell
- Process Explorer (Sysinternals)

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Credential Access | OS Credential Dumping | T1003 |
| Defense Evasion | Process Injection (Related) | T1055 |

---

## Lab Steps

### 1. Verify Services

```powershell
Get-Service Sysmon64
Get-Service WazuhSvc
```

---

### 2. Generate Process Access Activity

Launch **Process Explorer** as Administrator.

Allow it to enumerate running processes for approximately 30–60 seconds.

---

### 3. Verify Sysmon Event ID 10

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=10)]]" `
-MaxEvents 20
```

---

### 4. View Complete Event

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=10)]]" `
-MaxEvents 1 |
Format-List
```

---

### 5. Investigate in Wazuh

Search:

```
data.win.system.eventID:10
```

Review:

- Source Image
- Target Image
- GrantedAccess
- User
- Rule Description
- CallTrace
- Timestamp

---

## Detection Logic

Sysmon generates Event ID 10 whenever one process opens another process with specific access rights.

Wazuh collects the event and makes it available for threat hunting and investigation.

---

## Indicators Observed

- Source Process
- Target Process
- Granted Access
- User
- Call Trace
- Process GUIDs
- Process IDs
- Timestamp

---

## MITRE ATT&CK Summary

Technique:

- T1003 – OS Credential Dumping
- T1055 – Process Injection (Related)

---

## Learning Outcomes

After completing this lab you can:

- Detect Process Access activity
- Investigate Sysmon Event ID 10
- Analyze GrantedAccess permissions
- Identify source and target processes
- Hunt endpoint telemetry in Wazuh
- Map activity to MITRE ATT&CK

---

