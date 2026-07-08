# Investigation Notes

## Alert Summary

Detection Type:

Process Access

Detection Source:

Sysmon Event ID 10

Platform:

Windows

Collected By:

Wazuh Agent

---

## Timeline

Process Explorer launched

↓

Process Access activity generated

↓

Sysmon logged Event ID 10

↓

Wazuh Agent forwarded the event

↓

Wazuh indexed the event

↓

SOC investigation performed

---

## Key Findings

Event Logged:

Sysmon Event ID 10

Detection Type:

Process Access

Source Process:

Process Explorer (or process shown in your event)

Target Process:

Target process recorded by Sysmon

User:

Current logged-in user

Collected By:

Wazuh Agent

Indexed Into:

Wazuh Threat Hunting

---

## Important Fields

SourceImage

TargetImage

GrantedAccess

CallTrace

SourceProcessGUID

TargetProcessGUID

SourceProcessId

TargetProcessId

User

UtcTime

---

## Security Analysis

Process Access events are normal Windows activity.

However, they become suspicious when:

- A process accesses **lsass.exe**
- Unsigned tools access sensitive processes
- Office applications access system processes
- PowerShell accesses credential-related processes
- Unusual GrantedAccess values are observed

Analysts should always review the source process, target process, access rights, and surrounding activity before determining malicious intent.

---

## MITRE ATT&CK

T1003

OS Credential Dumping

T1055

Process Injection (Related)

---

## Conclusion

Sysmon successfully generated Event ID 10.

Wazuh ingested the telemetry and made it available for investigation.

The lab demonstrated how Process Access events help SOC analysts identify credential access attempts and investigate process-to-process interactions.
