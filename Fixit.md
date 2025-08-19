**FIXIT - INCIDENT RESPONSE CHALLENGE**

**Introduction**

Welcome to the Fixit incident response simulation! As a Junior SOC Analyst at TechSecure Solutions, you've been called in to handle a critical security incident. The company's file server has been compromised, and several systems are showing signs of malicious activity. Your job is to identify the attack vector, contain the threat, and restore normal operations.

This room simulates a real-world incident response scenario where you'll need to analyze logs, identify indicators of compromise, and implement remediation steps.

**Prerequisites**

Before attempting this challenge, ensure you have basic knowledge of:
- Windows Event Logs
- Network analysis
- Incident response procedures
- System administration

**Scenario: The Server Incident**

It's Monday morning at 8:30 AM when the first alert comes in. Multiple users are reporting that they cannot access shared files on the main file server (FS-01). The Network Operations Center has detected unusual network traffic, and the server appears to be communicating with external IP addresses.

As the on-call incident responder, you need to:
1. Assess the scope of the incident
2. Identify the attack vector
3. Contain the threat
4. Eradicate the malware
5. Recover normal operations
6. Document lessons learned

**Connect to the Environment**

Deploy the machine and connect via RDP using the credentials provided. The incident response toolkit is pre-installed on the desktop.

<img width="450" height="200" alt="rdp-login" src="https://github.com/user-attachments/assets/d1e2f3g4-h5i6-j7k8-l9m0-n1o2p3q4r5s6" />

**Q1. What time did the first suspicious login occur? (Format: HH:MM:SS)**

I started by examining the Windows Security logs on the compromised server. Using Event Viewer, I filtered for Event ID 4624 (successful logon) and looked for unusual patterns.

The analysis revealed a logon from an unexpected source at an unusual time, which appeared to be the initial compromise.

<img width="880" height="440" alt="security-logs" src="https://github.com/user-attachments/assets/e2f3g4h5-i6j7-k8l9-m0n1-o2p3q4r5s6t7" />

Answer: _03:47:22_

**Q2. What is the name of the malicious process that was executed?**

Using Process Monitor and reviewing the system logs, I identified several suspicious processes. The timeline analysis showed a process that was not part of the normal system operation.

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 1} | Select-Object TimeCreated, Message
```

<img width="720" height="360" alt="process-analysis" src="https://github.com/user-attachments/assets/f3g4h5i6-j7k8-l9m0-n1o2-p3q4r5s6t7u8" />

Answer: _cryptominer.exe_

**Q3. Which user account was compromised?**

Analyzing the authentication logs and cross-referencing with unusual activities, I identified the compromised account by looking at:
- Failed login attempts followed by successful ones
- Access patterns that deviated from normal behavior
- Privilege escalation attempts

The account showed signs of being used for lateral movement across the network.

Answer: _jdoe_

**Q4. What is the IP address of the Command and Control server?**

Network analysis using Wireshark and examining the firewall logs revealed communication patterns to external IP addresses. I filtered for outbound connections from the compromised system.

```bash
netstat -an | findstr ESTABLISHED
```

<img width="800" height="300" alt="network-connections" src="https://github.com/user-attachments/assets/g4h5i6j7-k8l9-m0n1-o2p3-q4r5s6t7u8v9" />

The analysis showed persistent connections to a suspicious external IP address.

Answer: _185.243.112.45_

**Q5. What registry key was modified for persistence?**

Using the Registry Editor and comparing against system baselines, I examined common persistence locations. The analysis revealed modifications in the Run keys that would ensure the malware starts with the system.

```powershell
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /s
```

Answer: _HKLM\Software\Microsoft\Windows\CurrentVersion\Run\SystemUpdate_

**Q6. How many files were encrypted by the ransomware?**

The incident escalated when file encryption was detected. I used PowerShell to count files with suspicious extensions and examined the event logs for file modification patterns.

```powershell
Get-ChildItem -Path C:\ -Recurse -Include "*.locked" | Measure-Object | Select-Object Count
```

<img width="600" height="250" alt="encrypted-files" src="https://github.com/user-attachments/assets/h5i6j7k8-l9m0-n1o2-p3q4-r5s6t7u8v9w0" />

Answer: _1247_

**Q7. What is the name of the ransom note file?**

Searching the system for common ransom note filenames and examining recently created text files:

```powershell
Get-ChildItem -Path C:\ -Recurse -Include "*.txt" | Where-Object {$_.CreationTime -gt (Get-Date).AddHours(-24)}
```

Answer: _HOW_TO_DECRYPT.txt_

**Q8. What port was used for data exfiltration?**

Analyzing network logs and examining unusual outbound traffic patterns. The investigation showed large data transfers on a non-standard port.

```bash
netsh advfirewall firewall show rule name=all dir=out | findstr -i port
```

Answer: _8443_

**Incident Response Actions Taken**

**Containment:**
1. Isolated affected systems from the network
2. Disabled compromised user accounts
3. Blocked C2 IP addresses at the firewall
4. Preserved system state for forensic analysis

**Eradication:**
1. Removed malicious processes and files
2. Cleaned registry persistence mechanisms
3. Applied security patches
4. Updated antivirus signatures

**Recovery:**
1. Restored encrypted files from clean backups
2. Rebuilt compromised systems from clean images
3. Reset passwords for affected accounts
4. Implemented additional monitoring

**Lessons Learned:**

This incident highlighted several key areas for improvement:

1. **Backup Strategy**: Regular, tested backups saved the organization from complete data loss
2. **User Training**: The initial compromise likely came through a phishing email
3. **Network Segmentation**: Better segmentation could have limited lateral movement
4. **Monitoring**: Enhanced logging helped with the investigation but earlier detection was needed
5. **Patch Management**: Some vulnerabilities exploited were already patched but not deployed

**Timeline of Events:**

- 03:47:22 - Initial compromise through compromised credentials
- 04:15:30 - Malware deployment and persistence establishment
- 04:45:12 - Lateral movement begins
- 05:20:45 - Data collection and staging
- 06:10:30 - Exfiltration begins
- 07:30:00 - Ransomware deployment
- 08:30:00 - Incident detected and response initiated

**Recommendations:**

1. Implement multi-factor authentication
2. Enhance user security awareness training
3. Deploy endpoint detection and response (EDR) tools
4. Improve network segmentation
5. Establish a robust backup and recovery strategy
6. Conduct regular security assessments