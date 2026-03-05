# PowerShell Commands (Lab Evidence)

> Commands used during the simulated attack + investigation.
## Scenario A — Logging & Audit Setup (Windows)


### Enable Credential Validation auditing
```powershell
auditpol /set /subcategory:"Credential Validation" /success:enable /failure:enable
```
Verify audit policy (examples)
```powershell
auditpol /get /subcategory:"Credential Validation"
auditpol /get /category:*
```
# Scenario B — Encoded PowerShell Execution + Investigation
> Generate an EncodedCommand (example workflow)
```
$cmd = "Get-LocalUser | Select Name,Enabled"
$bytes = [System.Text.Encoding]::Unicode.GetBytes($cmd)
$enc = [Convert]::ToBase64String($bytes)
powershell.exe -NoProfile -ExecutionPolicy Bypass -EncodedCommand $enc
```
# Scenario C — Local User Creation + Privilege Escalation Attempt
### Create a standard local user
```
net user standard1 "UserP@ss123!" /add
```
### Attempt to add user to local Administrators (expected: Access Denied)
```
net localgroup administrators standard1 /add
```
### Check recent Security events (examples)
```
Get-WinEvent -FilterHashtable @{LogName="Security"; StartTime=(Get-Date).AddMinutes(-30)} |
Select-Object -First 25 TimeCreated, Id, ProviderName, Message
```
### Filter for admin/group related activity
```
Get-WinEvent -FilterHashtable @{LogName="Security"; StartTime=(Get-Date).AddMinutes(-30)} |
Where-Object { $_.Message -like "*localgroup*" -or $_.Message -like "*administrators*" -or $_.Message -like "*standard1*" } |
Select-Object -First 10 TimeCreated, Id, ProviderName
```
# Scenario D — Network / Recon Prep (Windows + Linux)
### Identify Windows IP address
```
ipconfig
```
# Allow inbound ICMPv4 (ping) on Windows Defender Firewall
```
netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=icmpv4 dir=in action=allow
```
### Linux connectivity test (Ubuntu)
```
ping -c 2 192.168.64.7
```
# Linux port scan (Ubuntu)
```
sudo nmap -sS -T3 -p 22,80,135,139,445,3389 192.168.64.7
```
---
