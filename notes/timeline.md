# Investigation Timeline

This timeline documents the sequence of events observed during the simulated attack scenarios.  
Events were reconstructed using Windows Event Logs and Splunk SIEM queries.

---

## Phase 1 — Logging Configuration (Preparation)

**Action**
Security auditing and PowerShell logging were enabled on the Windows system.

**Events Observed**
- Windows audit policies configured
- Process creation logging enabled
- PowerShell operational logs enabled

**Purpose**
Ensure sufficient visibility for security monitoring.

---

## Phase 2 — Encoded PowerShell Execution

**Action**
An encoded PowerShell command was executed to simulate suspicious script execution.

**Detection Method**
Splunk query searching for:


EncodedCommand


**Logs Observed**
- PowerShell Operational logs
- Encoded command activity recorded in event logs

**Security Insight**
Encoded PowerShell commands are commonly used by attackers to hide malicious scripts.

---

## Phase 3 — Local Account Creation

**Action**
A new local user account was created.

Example command used:


net user standard1 /add


**Logs Observed**

| Event ID | Description |
|------|------|
| 4720 | User account created |

**Security Insight**

Account creation events should be monitored because attackers may create persistence accounts.

---

## Phase 4 — Privilege Escalation Attempt

**Action**
An attempt was made to add the user to the local Administrators group.

Command used:


net localgroup administrators standard1 /add


**Logs Observed**

| Event ID | Description |
|------|------|
| 4688 | Process creation |
| 4728 / 4732 | User added to privileged group |

**Security Insight**

Monitoring group membership changes is critical for detecting privilege escalation attempts.

---

## Phase 5 — Network Reconnaissance

**Action**
A Linux machine performed reconnaissance against the Windows system.

Activities included:

- ICMP connectivity testing (`ping`)
- Port scanning using `nmap`

Example command:


sudo nmap -sS -T3 -p 22,80,135,139,445,3389 192.168.64.7


**Security Insight**

Port scanning is commonly performed by attackers during the reconnaissance phase to identify exposed services.

---

## Investigation Summary

The simulated attack demonstrated several common attacker behaviors:

- encoded PowerShell execution
- account creation
- attempted privilege escalation
- network reconnaissance

These activities were detectable through Windows event logs and investigated using Splunk SIEM queries.

This exercise reinforced the importance of proper logging configuration and centralized log analysis in security monitoring.
