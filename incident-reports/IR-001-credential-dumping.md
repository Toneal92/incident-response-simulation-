# Incident Report — IR-001
**Date:** June 8, 2026  
**Severity:** Critical  
**Status:** Resolved (simulated)  
**Analyst:** Toneal  
**MITRE ATT&CK:** T1003.001 — LSASS Memory

## Executive Summary
A credential dumping attempt was detected on endpoint 
DESKTOP-BK5SL06. Sysmon telemetry captured lsass.exe 
making registry modifications consistent with the 
ProcDump credential harvesting technique. Windows Defender 
blocked the actual memory dump however the attempt was 
fully logged providing complete forensic evidence. This 
technique is used by attackers to harvest credential 
hashes for lateral movement across the network.

## Timeline
| Time | Event |
|------|-------|
| 16:19:19.154 | lsass.exe registry modification detected |
| 16:19:19.260 | Second registry event generated |
| 16:19:19.355 | Final registry event captured |
| 16:22:33 | Events ingested into SIEM via Winlogbeat |
| 16:22:33 | Investigation initiated |

## Technical Findings
**Attack Technique:** OS Credential Dumping — LSASS Memory (T1003.001)  
**Tool Used:** ProcDump via Atomic Red Team T1003.001-1  
**Affected System:** DESKTOP-BK5SL06  
**Affected Process:** lsass.exe (PID 588)  
**User Context:** SYSTEM  
**Event ID:** 13 (Sysmon Registry Value Set)  
**Rule Name:** T1101  
**Registry Hive:** HKLM  
**Channel:** Microsoft-Windows-Sysmon/Operational  
**Total Events Generated:** 63  
**Attack Result:** Blocked by Windows Defender  

## MITRE ATT&CK Mapping
- **Tactic:** Credential Access
- **Technique:** T1003 — OS Credential Dumping
- **Sub-technique:** T1003.001 — LSASS Memory

## Root Cause
An attacker simulated using ProcDump to access LSASS 
process memory in order to extract credential hashes. 
LSASS (Local Security Authority Subsystem Service) 
temporarily stores credential data in memory to support 
Windows authentication. Attackers target this process 
to steal hashes that can be cracked offline or used 
in Pass-the-Hash attacks for lateral movement.

## Recommendations
1. Enable Windows Credential Guard to protect LSASS memory
2. Enable Protected Process Light (PPL) for LSASS
3. Alert on any process attempting to access LSASS memory
4. Monitor for ProcDump and Mimikatz on all endpoints
5. Implement privileged access workstations for admin tasks
6. Enable MFA to reduce impact of stolen credentials

## Lessons Learned
Even when the attack is blocked by endpoint security the 
attempt is still fully logged by Sysmon. This demonstrates 
the importance of layered defenses — EDR blocks the attack 
while SIEM captures the forensic evidence for investigation.
The registry modification events (Event ID 13) provided 
clear indicators of the credential dumping attempt.
