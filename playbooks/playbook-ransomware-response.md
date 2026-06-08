# IR Playbook — Ransomware Response
**Version:** 1.0  
**Author:** Toneal  
**Last Updated:** June 8, 2026  
**Severity:** Critical

---

## Overview
This playbook defines the response procedures for a 
confirmed or suspected ransomware incident. Ransomware 
encrypts victim files and demands payment for decryption. 
Speed of response is critical — every minute of delay 
allows more files to be encrypted and more systems to 
be compromised.

---

## Detection Triggers
- Mass file rename or encryption events (Sysmon Event ID 11)
- Known ransomware process names in alerts
- Shadow copy deletion commands detected
  - vssadmin delete shadows /all
  - wmic shadowcopy delete
- Unusual volume of file modification events
- C2 beacon activity from endpoint

---

## Severity: CRITICAL — Immediate Response Required

---

## Response Phases

### Phase 1 — Containment (First 15 Minutes)
**Goal: Stop the spread immediately**

1. Isolate affected machine from network immediately
   - Disconnect ethernet cable physically if possible
   - Disable WiFi adapter via device manager
   - Do NOT turn off the machine (preserves memory evidence)
2. Identify blast radius — check for lateral movement
   - Review authentication logs for unusual logins
   - Check other endpoints for similar file activity
3. Notify security team lead and management immediately
4. Document: affected hostname, IP address, user logged in, 
   time discovered, initial symptoms observed

### Phase 2 — Investigation
**Goal: Understand what happened and how**

1. Pull Kibana timeline going back 72 hours before detection
2. Identify patient zero — first machine affected
3. Determine initial access vector:
   - Check email logs for phishing attempts
   - Review RDP authentication logs
   - Check for USB device insertion events
4. Map lateral movement path to other systems
5. Identify what data was encrypted
6. Determine if data was exfiltrated before encryption
7. Identify ransomware variant using file extensions and 
   ransom note — check nomoreransom.org for decryptors
8. Preserve memory dump before powering down:
   - Use WinPmem or Magnet RAM Capture

### Phase 3 — Eradication
**Goal: Remove the threat completely**

1. Identify all persistence mechanisms:
   - Check registry run keys (T1547.001)
   - Check scheduled tasks
   - Check startup folder
   - Check services
2. Remove all malicious files and registry entries
3. Reset credentials for ALL accounts on affected systems
4. Patch the vulnerability used for initial access
5. Block IOCs (IPs, domains, hashes) at firewall and proxy
6. Scan all other endpoints for same IOCs

### Phase 4 — Recovery
**Goal: Restore operations safely**

1. Verify clean backup exists before restoring
2. Restore from last known clean backup
3. Verify backup integrity before reconnecting to network
4. Reconnect to network in monitored segment
5. Monitor restored system for 72 hours intensively
6. Confirm all encrypted files are recovered
7. Do NOT pay the ransom without legal and management approval

### Phase 5 — Post-Incident
**Goal: Learn and improve**

1. Write formal incident report using standard IR template
2. Conduct lessons learned meeting within 5 business days
3. Update detection rules based on new IOCs discovered
4. Update this playbook based on gaps identified
5. Brief affected users on phishing awareness
6. Report to relevant authorities if required (CISA, FBI)

---

## Key Contacts
| Role | Action |
|------|--------|
| Security Team Lead | Immediate notification |
| IT Management | Escalation and decisions |
| Legal Counsel | Ransom payment decisions |
| PR/Communications | External communications |
| Cyber Insurance | Claim notification |

---

## Evidence to Preserve
- Memory dump of affected systems
- Full disk image before remediation
- Network flow logs
- SIEM logs covering 72 hours before incident
- Ransom note and encrypted file samples
- All IOCs discovered during investigation
