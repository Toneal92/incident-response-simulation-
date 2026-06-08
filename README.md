# Incident Response Simulation Lab

**Tools:** Atomic Red Team | Elastic SIEM | Sysmon | Winlogbeat | MITRE ATT&CK Navigator  
**Techniques Simulated:** T1003.001, T1547.001, T1059.001  
**Author:** Toneal | Former MDDR Analyst Intern — Varonis Systems

---

## Overview
Simulated 3 real-world attack techniques using Atomic Red Team 
in an isolated home lab environment. Investigated each attack 
using Elastic SIEM and Sysmon telemetry. Produced formal incident 
reports following professional SOC standards and built an IR 
playbook modeled on enterprise incident response procedures.

---

## Lab Environment
Built on top of the SIEM Home Lab project:
- Windows 10 VM — target endpoint with Sysmon + Winlogbeat
- Ubuntu Server VM — Elastic Stack SIEM
- Atomic Red Team — attack simulation framework

---

## Attack Simulations

| ID | Technique | MITRE TTP | Tool Used | Result | Report |
|----|-----------|-----------|-----------|--------|--------|
| IR-001 | LSASS Credential Dumping | T1003.001 | ProcDump | Blocked + Detected | [View](incident-reports/IR-001-credential-dumping.md) |
| IR-002 | Registry Run Key Persistence | T1547.001 | reg.exe | Success + Detected | [View](incident-reports/IR-002-registry-persistence.md) |
| IR-003 | Mimikatz via PowerShell | T1059.001 | Mimikatz | Blocked + Detected | [View](incident-reports/IR-003-mimikatz-execution.md) |

---

## Key Findings

- **Attack IR-001:** Windows Defender blocked LSASS memory dump 
  however Sysmon captured 63 forensic events proving the attempt
- **Attack IR-002:** Registry persistence fully established using 
  native reg.exe binary — Living off the Land technique detected 
  by Sysmon SwiftOnSecurity ruleset (T1060,RunKey)
- **Attack IR-003:** 238 PowerShell Script Block events captured 
  Mimikatz source code loading into memory despite execution being blocked

---

## Artifacts Produced

- 3 formal incident reports with timeline, evidence, and recommendations
- 1 ransomware IR playbook — see [playbooks/](playbooks/)
- MITRE ATT&CK Navigator coverage layer — see [mitre-attack/](mitre-attack/)
- Kibana evidence screenshots — see [screenshots/](screenshots/)

---

## MITRE ATT&CK Coverage
![MITRE ATT&CK Navigator](mitre-attack/navigator-screenshot.png)

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Atomic Red Team | Safe attack simulation framework |
| Elastic Stack | SIEM for log collection and analysis |
| Sysmon | Enhanced Windows endpoint telemetry |
| Winlogbeat | Log shipping to Elasticsearch |
| MITRE ATT&CK Navigator | Threat coverage visualization |

---

## Related Projects
- [SIEM Home Lab](https://github.com/Toneal92/siem-home-lab) — 
  The detection infrastructure used in this project
