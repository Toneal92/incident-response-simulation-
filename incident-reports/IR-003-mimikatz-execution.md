# Incident Report — IR-003
**Date:** June 8, 2026  
**Severity:** Critical  
**Status:** Resolved (simulated)  
**Analyst:** Toneal  
**MITRE ATT&CK:** T1059.001 — PowerShell / T1003 — Credential Dumping

## Executive Summary
A Mimikatz execution attempt was detected on endpoint 
DESKTOP-BK5SL06. PowerShell Script Block Logging captured 
238 events related to Mimikatz functions being loaded into 
memory via PowerShell. Windows Defender blocked the actual 
execution however complete forensic evidence was captured 
by Sysmon and forwarded to the SIEM. Mimikatz is one of 
the most widely used credential harvesting tools by threat 
actors and nation state groups worldwide.

## Timeline
| Time | Event |
|------|-------|
| 16:42:47 | PowerShell Script Block Logging triggered (Event ID 4104) |
| 16:42:47 | 238 script block events generated in rapid succession |
| 16:42:47 | Mimikatz functions captured in script block text |
| 16:42:47 | Windows Defender blocked execution |
| 16:42:47 | Events shipped to SIEM via Winlogbeat |

## Technical Findings
**Attack Technique:** PowerShell — Mimikatz (T1059.001 / T1003)  
**Tool Used:** Mimikatz via Atomic Red Team T1059.001-1  
**Affected System:** DESKTOP-BK5SL06  
**Event ID:** 4104 (PowerShell Script Block Logging)  
**Script Block ID:** 68f712a3-72fd-477f-88e8-9550b50931ba  
**Script Block Hash:** zIoV/ZCLYMWvjhvvb+evw33e4Ec=  
**Script Block Text:** function Test-UnnecessaryFiles  
**Total Events Generated:** 238  
**Attack Result:** Blocked by Windows Defender  

## MITRE ATT&CK Mapping
- **Tactic:** Execution / Credential Access
- **Technique:** T1059.001 — PowerShell
- **Technique:** T1003 — OS Credential Dumping
- **Tool:** Mimikatz

## Root Cause
An attacker attempted to load Mimikatz via PowerShell to 
harvest credentials from Windows memory. Mimikatz is an 
open source credential harvesting tool that can extract 
plaintext passwords, hashes, PIN codes and Kerberos 
tickets from memory. It is used in the vast majority of 
ransomware attacks and APT intrusions for lateral movement 
and privilege escalation. PowerShell Script Block Logging 
captured the full Mimikatz source code as it was being 
loaded into memory providing complete forensic evidence 
even though execution was blocked.

## Recommendations
1. Keep Windows Defender and EDR signatures up to date
2. Enable PowerShell Script Block Logging on all endpoints
3. Implement PowerShell Constrained Language Mode
4. Alert on known Mimikatz function names in script blocks
5. Enable Windows Credential Guard to protect LSASS
6. Monitor for abnormal LSASS memory access attempts
7. Implement Just-In-Time (JIT) privileged access

## Lessons Learned
Script Block Logging captured 238 events of Mimikatz code 
being loaded into memory even though execution was blocked. 
This demonstrates that even blocked attacks leave forensic 
evidence that can be used for investigation and threat 
hunting. The script block hash can be used to hunt for 
the same tool across other endpoints in the environment.
