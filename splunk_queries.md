# Splunk Queries (Lab Evidence)

Queries used during investigation + hunting in Splunk.

---

## Scenario A — Brute Force / Failed Logons (Windows Security Logs)

### Failed logons (4625) for victim
```spl
index=main sourcetype="WinEventLog:Security" EventCode=4625 Account_Name=victim
```
### Failed logons over time (trend)
```
index=main sourcetype="WinEventLog:Security" EventCode=4625 Account_Name=victim
| timechart span=5m count
```
### Top source IPs attempting logon
```
index=main sourcetype="WinEventLog:Security" EventCode=4625 Account_Name=victim
| stats count by IpAddress
| sort - count
```
### Success logons (4624) for victim (check if attacker succeeded)
```
index=main sourcetype="WinEventLog:Security" EventCode=4624 Account_Name=victim
```
## Scenario B — Encoded PowerShell Detection (PowerShell Operational)
### Hunt for EncodedCommand usage
```
index=main sourcetype="XmlWinEventLog:Microsoft-Windows-PowerShell/Operational"
("EncodedCommand" OR "-EncodedCommand")
| table _time host EventCode Message
| sort - _time
```
### Focus on a specific host (optional)
```
index=main sourcetype="XmlWinEventLog:Microsoft-Windows-PowerShell/Operational"
("EncodedCommand" OR "-EncodedCommand") host="WIN-Q2FC3G99SLJ"
| table _time host EventCode Message
| sort - _time
```
## Scenario C — Local Account / Privilege Change Attempt (Security)
### Events mentioning standard1 (quick pivot)
```
index=main sourcetype="WinEventLog:Security" standard1
| stats count by EventCode
| sort - count
```
### Hunt for localgroup / administrators commands (process creation)
```
index=main sourcetype="WinEventLog:Security"
(standard1 OR "standard1")
("net localgroup" OR "localgroup administrators" OR "net1.exe" OR "net.exe")
| table _time host EventCode Account_Name User CommandLine New_Process_Name Process_Name Message
| sort - _time
```
### Process creation counts for EventCode 4688 (what’s noisy vs suspicious)
```
index=main sourcetype="WinEventLog:Security" EventCode=4688
| stats count by New_Process_Name
| sort - count
```
## Scenario D — Network Recon From Linux (Nmap / Connectivity)

Note: This scenario is performed from Linux (attacker host). If Linux logs are not ingested to Splunk, evidence is captured via screenshots instead.

### If Linux logs are ingested (optional placeholder)
```
index=* (nmap OR "Starting Nmap" OR "Nmap scan report")
