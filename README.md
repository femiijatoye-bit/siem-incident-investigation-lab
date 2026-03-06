# SIEM Incident Investigation Lab
## Skills Demonstrated

- SIEM Investigation (Splunk)
- Windows Event Log Analysis
- PowerShell Security Monitoring
- Threat Hunting
- Detection Engineering
- Network Reconnaissance Analysis

---

A hands-on cybersecurity lab simulating multiple attacker behaviors in a controlled environment and investigating them using **Splunk SIEM**.

This project demonstrates how common attack techniques appear in Windows logs and how a SOC analyst can investigate them using centralized log analysis.

---

# Lab Overview

This lab simulates several stages of attacker activity including:

- Security logging configuration
- Encoded PowerShell execution
- Local account creation
- Privilege escalation attempts
- Network reconnaissance

The events are ingested into **Splunk** and investigated using **custom detection queries**.

---

# Lab Architecture

Environment used in the investigation:

| System | Role |
|------|------|
| Windows VM | Target system generating logs |
| Ubuntu VM | Attacker simulation |
| Splunk | SIEM platform for log analysis |

Tools used:

- Splunk SIEM
- Windows Event Logs
- PowerShell
- Linux (Ubuntu)
- Nmap

---

# Attack Scenarios Simulated

## Scenario A — Security Logging Setup

Security auditing and PowerShell logging were enabled on the Windows system to ensure visibility of suspicious activity.

Events monitored:

- Process creation
- Credential validation
- PowerShell operational logs

---

## Scenario B — Encoded PowerShell Execution

A simulated attacker executed an **encoded PowerShell command** to mimic obfuscated script execution.

Detection focused on:

- `EncodedCommand` usage
- PowerShell operational logs

---

## Scenario C — Local Account Creation & Privilege Escalation Attempt

A new local user was created and an attempt was made to add the user to the **Administrators group**.

Example commands:


net user standard1 /add
net localgroup administrators standard1 /add


Relevant Windows event IDs:

| Event ID | Description |
|------|------|
| 4720 | User account created |
| 4688 | Process creation |
| 4728 / 4732 | User added to privileged group |

---

## Scenario D — Network Reconnaissance

A Linux system performed reconnaissance against the Windows host.

Activities included:

- ICMP connectivity testing
- Nmap port scanning

Example scan:


sudo nmap -sS -T3 -p 22,80,135,139,445,3389 192.168.64.7


This simulates the **reconnaissance phase of an attack**.

---

# Investigation Process

The investigation was conducted using **Splunk SIEM**.

Key techniques used:

- Windows Event Log analysis
- PowerShell activity monitoring
- Authentication log investigation
- Process creation monitoring
- Event correlation through Splunk queries

Example query:


index=main sourcetype="WinEventLog:Security" EventCode=4688
| stats count by New_Process_Name
| sort - count


---

# Key Skills Demonstrated

This lab demonstrates practical SOC analyst skills including:

- SIEM investigation
- Windows log analysis
- threat hunting
- attacker behavior simulation
- PowerShell investigation
- network reconnaissance analysis

---

# Key Takeaways

This lab demonstrates how multiple stages of an attack can be detected using Windows event logs and SIEM analysis.

Important lessons include:

- Proper logging configuration is essential for security monitoring
- PowerShell is a common attacker tool
- Privilege escalation attempts can be detected through event logs
- SIEM tools help correlate events across systems
- Network reconnaissance often occurs early in an attack

---

# Future Improvements

Potential enhancements to this lab include:

- detection rules and alert creation
- automated correlation searches
- MITRE ATT&CK technique mapping
- additional attack simulations

---

# MITRE ATT&CK Mapping

| Scenario | Technique | MITRE ID |
|--------|--------|--------|
| Encoded PowerShell Execution | Command and Scripting Interpreter | T1059 |
| Encoded PowerShell | Obfuscated Files or Information | T1027 |
| Failed Logon Attempts | Brute Force | T1110 |
| Privilege Escalation Attempt | Account Manipulation | T1098 |
| Network Reconnaissance | Network Service Discovery | T1046 |

---
# Author

Femi Ijatoye  
Security+ Certified  
Cybersecurity / SOC Analyst Portfolio Project
