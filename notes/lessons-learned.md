# Lessons Learned (SOC Investigation Lab)

This lab simulated multiple attack behaviors in a controlled environment using Windows, Linux, and Splunk SIEM.  
The goal was to understand how suspicious activity appears in logs and how a SOC analyst would investigate these events.

---

## 1. Visibility Is Critical for Detection

One of the most important lessons learned is that proper logging must be enabled before meaningful detection can occur.

Examples from the lab:
- Enabling **Windows Security auditing**
- Enabling **PowerShell logging**
- Capturing **process creation events (Event ID 4688)**

Without these logs, it would be difficult for analysts to detect malicious activity such as encoded PowerShell execution or suspicious command usage.

---

## 2. PowerShell Is a Common Attack Vector

PowerShell is heavily used by administrators but is also abused by attackers.

In this lab I observed:
- Encoded PowerShell command execution
- PowerShell activity recorded in **Microsoft-Windows-PowerShell/Operational logs**

This demonstrated why SOC teams often monitor for:
- `EncodedCommand`
- `ExecutionPolicy Bypass`
- suspicious PowerShell command usage

---

## 3. Windows Event Logs Provide Valuable Investigation Data

During the investigation I analyzed several important event IDs including:

| Event ID | Description |
|------|------|
| 4624 | Successful logon |
| 4625 | Failed logon attempt |
| 4688 | Process creation |
| 4720 | User account created |
| 4728 / 4732 | User added to privileged group |

These logs provide insight into:
- authentication attempts
- account changes
- suspicious command execution

---

## 4. Network Reconnaissance Can Be Identified Through Behavior

Using the Linux system, I performed reconnaissance activities including:

- ICMP connectivity testing (`ping`)
- Network scanning with **Nmap**

These actions simulate how an attacker might enumerate services before attempting exploitation.

Although the scan itself was not ingested into Splunk in this lab, it demonstrates the types of network behavior that security monitoring tools should detect.

---

## 5. SIEM Tools Help Correlate Security Events

Using Splunk, I was able to:

- search Windows security logs
- detect suspicious commands
- identify process creation events
- analyze authentication attempts
- aggregate data using `stats` queries

SIEM platforms allow analysts to quickly pivot between events and identify suspicious patterns across systems.

---

## 6. Small Indicators Can Reveal Suspicious Activity

Simple commands such as:


net user
net localgroup administrators
net1.exe


can indicate potential privilege escalation attempts.

Monitoring these commands through **process creation logs (4688)** can help analysts identify suspicious behavior early in an attack.

---

## Conclusion

This investigation lab demonstrated how common attacker behaviors appear within Windows event logs and how a SIEM platform can be used to analyze those events.

Key takeaways:

- Proper logging configuration is essential
- PowerShell activity should be closely monitored
- Windows event IDs provide valuable investigation context
- Network reconnaissance is often an early stage of attacks
- SIEM tools are critical for correlating security data

Overall, this lab helped reinforce the investigative mindset required for a SOC analyst and improved my understanding of Windows security logging and SIEM-based threat detection.
