# Troubleshooting Notes

## Issue 1

No Event ID 10 displayed.

Cause

No Process Access activity had occurred recently or Event ID 10 was not enabled in the Sysmon configuration.

Resolution

Launch Process Explorer as Administrator and allow it to enumerate running processes.

Verify Event ID 10 again.

---

## Issue 2

No results in Wazuh Threat Hunting.

Cause

The Wazuh Agent had not yet forwarded the latest Sysmon events.

Resolution

Wait 30–60 seconds and refresh Threat Hunting.

---

## Issue 3

Search returned no results.

Cause

The search query was too specific or field mappings differed.

Resolution

Try the following searches:

```
data.win.system.eventID:10
```

```
GrantedAccess
```

```
ProcessAccess
```

```
lsass.exe
```

```
Process Explorer
```

---

## Issue 4

Multiple Event ID 10 records appeared.

Cause

Windows generates many legitimate Process Access events during normal operation.

Resolution

Filter the results by:

- Timestamp
- Source Process
- Target Process
- User

to identify the event generated during the lab.

---

## Useful Commands

Verify Services

```powershell
Get-Service Sysmon64
Get-Service WazuhSvc
```

View Event ID 10

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=10)]]"
```

View Complete Event

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=10)]]" `
-MaxEvents 1 |
Format-List
```

---

## Lessons Learned

- Sysmon Event ID 10 records Process Access activity.
- Wazuh successfully ingests Process Access telemetry for investigation.
- Reviewing SourceImage, TargetImage, and GrantedAccess helps distinguish normal administrative activity from suspicious credential access attempts.
- Event ID 10 is valuable for detecting techniques such as credential dumping and process injection when correlated with other endpoint telemetry.
