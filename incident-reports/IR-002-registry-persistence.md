# Incident Report — IR-002
**Date:** June 8, 2026  
**Severity:** High  
**Status:** Resolved (simulated)  
**Analyst:** Toneal  
**MITRE ATT&CK:** T1547.001 — Registry Run Keys

## Executive Summary
A registry-based persistence mechanism was detected on 
endpoint DESKTOP-BK5SL06. Sysmon captured reg.exe adding 
a malicious entry to the Windows CurrentVersion\Run 
registry key. This technique ensures malware survives 
system reboots by executing automatically when any user 
logs in. The attack completed successfully with exit 
code 0 indicating the persistence was fully established.

## Timeline
| Time | Event |
|------|-------|
| 14:56:01 | reg.exe executed to modify registry |
| 14:56:01 | Sysmon Event ID 13 generated (Registry Value Set) |
| 14:56:01 | Registry key written to CurrentVersion\Run |
| 14:56:01 | Sysmon rule T1060,RunKey automatically triggered |
| 14:56:01 | Event shipped to SIEM via Winlogbeat |

## Technical Findings
**Attack Technique:** Boot or Logon Autostart — Registry Run Keys (T1547.001)  
**Tool Used:** reg.exe (native Windows binary)  
**Affected System:** DESKTOP-BK5SL06  
**Affected User:** toneal  
**Process Name:** reg.exe (PID 7352)  
**Process Path:** C:\Windows\system32\reg.exe  
**Registry Hive:** HKU  
**Registry Key:** SOFTWARE\Microsoft\Windows\CurrentVersion\Run  
**Registry Value Name:** Atomic Red Team  
**Payload Path:** C:\Path\AtomicRedTeam.exe  
**Sysmon Rule Triggered:** T1060,RunKey  
**Event ID:** 13 (Sysmon Registry Value Set)  
**Attack Result:** Successful — persistence established  

## MITRE ATT&CK Mapping
- **Tactic:** Persistence
- **Technique:** T1547.001 — Boot or Logon Autostart Execution: Registry Run Keys
- **Living off the Land:** Yes — used native reg.exe binary

## Root Cause
An attacker used the native Windows reg.exe binary to 
write a malicious entry to the CurrentVersion\Run 
registry key. This key executes any program listed in 
it automatically when a user logs into Windows. By using 
a native Windows binary (reg.exe) instead of malware 
the attacker attempted to blend in with normal system 
activity — a technique known as Living off the Land 
(LotL). The Sysmon SwiftOnSecurity ruleset automatically 
flagged this as a known attack pattern (T1060,RunKey).

## Recommendations
1. Monitor all writes to CurrentVersion\Run registry keys
2. Alert on reg.exe modifying autostart registry locations
3. Implement application whitelisting to block unauthorized executables
4. Audit all registry run keys on endpoints regularly
5. Use Autoruns from Sysinternals to identify persistence mechanisms
6. Restrict registry modification permissions for standard users

## Lessons Learned
The Sysmon SwiftOnSecurity ruleset automatically tagged 
this event as a known attack technique demonstrating the 
value of using community-maintained detection rulesets. 
The attacker used a native Windows binary (reg.exe) 
highlighting how Living off the Land techniques can 
bypass security tools that only monitor for known malware.
