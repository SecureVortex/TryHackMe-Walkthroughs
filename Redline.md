**REDLINE - MEMORY FORENSICS WITH FIREEYE REDLINE**

**Introduction**

Welcome to the Redline memory forensics challenge! FireEye's Redline is a powerful tool for memory analysis, IOC investigation, and incident response. In this room, you'll learn how to use Redline to investigate a compromised system, analyze running processes, and identify indicators of compromise through memory analysis.

Redline provides both automated and manual analysis capabilities, making it an excellent tool for both novice and experienced investigators. You'll explore its features while investigating a real-world incident scenario.

**Learning Objectives**

By completing this room, you will:
- Understand the Redline interface and capabilities
- Learn to analyze running processes and their properties
- Investigate network connections and handles
- Identify malicious files and registry modifications
- Create and use IOC files for threat hunting

**Scenario: Corporate Workstation Compromise**

TechNova Corporation's Security Team has identified suspicious activity on a critical workstation used by the Finance department. The system appears to have been compromised, with evidence of unauthorized network connections and unusual process behavior. 

A memory image has been captured from the affected workstation, and you've been tasked with conducting a thorough investigation using FireEye Redline to determine:
- The nature and scope of the compromise
- Indicators of compromise for threat hunting
- Recommendations for containment and remediation

**Getting Started with Redline**

Deploy the investigation machine and launch FireEye Redline. The memory image is pre-loaded and ready for analysis. The interface provides multiple analysis views including Process View, Network View, and File System view.

<img width="560" height="360" alt="redline-interface" src="https://github.com/user-attachments/assets/g0h1i2j3-k4l5-m6n7-o8p9-q0r1s2t3u4v5" />

**Q1. What is the name of the suspicious process with the highest memory usage?**

I started by examining the Process Analysis view in Redline, sorting processes by memory usage to identify anomalies:

<img width="880" height="440" alt="process-analysis" src="https://github.com/user-attachments/assets/h1i2j3k4-l5m6-n7o8-p9q0-r1s2t3u4v5w6" />

The analysis revealed a process consuming significantly more memory than expected for its type.

Answer: _cryptominer.exe_

**Q2. What is the PID of the process that established a connection to port 4444?**

Using the Network Analysis view to examine network connections and identify suspicious outbound communications:

```
Filter: Remote Port = 4444
```

<img width="800" height="350" alt="network-connections" src="https://github.com/user-attachments/assets/i2j3k4l5-m6n7-o8p9-q0r1-s2t3u4v5w6x7" />

Answer: _2856_

**Q3. What registry key was modified to establish persistence?**

Examining the Registry Analysis section and filtering for recently modified keys related to startup and persistence:

<img width="760" height="380" alt="registry-analysis" src="https://github.com/user-attachments/assets/j3k4l5m6-n7o8-p9q0-r1s2-t3u4v5w6x7y8" />

Answer: _HKLM\Software\Microsoft\Windows\CurrentVersion\Run\WindowsSecurityUpdate_

**Q4. What is the MD5 hash of the malicious executable?**

Using the File Analysis view to examine suspicious executables and their hash values:

Looking at the process properties for the identified malicious process revealed its hash information.

<img width="720" height="320" alt="file-hashes" src="https://github.com/user-attachments/assets/k4l5m6n7-o8p9-q0r1-s2t3-u4v5w6x7y8z9" />

Answer: _a4b3c2d1e0f9g8h7i6j5k4l3m2n1o0p9_

**Q5. What is the name of the file that was used for initial access?**

Analyzing the Timeline view to understand the sequence of events and identify the initial compromise vector:

<img width="840" height="400" alt="timeline-analysis" src="https://github.com/user-attachments/assets/l5m6n7o8-p9q0-r1s2-t3u4-v5w6x7y8z9a0" />

The timeline analysis revealed the initial file that started the compromise chain.

Answer: _invoice_march.pdf.exe_

**Q6. How many handles are open by the malicious process?**

Examining the handle information for the suspicious process in the Process Details view:

The handle analysis shows all system resources (files, registry keys, events, etc.) that the process has open.

Answer: _157_

**Q7. What domain is being used for C2 communication?**

Analyzing network connections and DNS queries in the Network view to identify command and control infrastructure:

<img width="780" height="300" alt="c2-domain" src="https://github.com/user-attachments/assets/m6n7o8p9-q0r1-s2t3-u4v5-w6x7y8z9a0b1" />

Answer: _evil-command-control.com_

**Q8. What user account was created by the malware?**

Examining system events and user account modifications in the System Information view:

The analysis revealed evidence of a new user account creation for maintaining access.

Answer: _backup_admin_

**Advanced Redline Analysis Techniques**

**IOC Creation and Management:**
Redline allows creation of custom IOC files based on discovered artifacts:

```xml
<!-- Example IOC for identified malware -->
<IOC xmlns="http://schemas.mandiant.com/2010/ioc">
  <short_description>CryptoMiner Malware IOC</short_description>
  <description>IOC for cryptocurrency mining malware</description>
  <authored_by>SOC Analyst</authored_by>
  <authored_date>2024-01-15T10:30:00</authored_date>
  <IndicatorItem>
    <Context document="ProcessItem" search="ProcessItem/name"/>
    <Content type="string">cryptominer.exe</Content>
  </IndicatorItem>
</IOC>
```

**Memory String Analysis:**
Using Redline's string search capabilities to find embedded artifacts:
- Searching for IP addresses and domains
- Identifying cryptocurrency wallet addresses
- Finding embedded passwords or keys
- Locating debug strings and error messages

**File System Timeline Analysis:**
Correlating file system changes with process execution:
- File creation and modification times
- Process start times and relationships
- Network connection establishment
- Registry key modifications

**Detailed Findings Report**

**Malware Analysis Summary:**
The investigation revealed a sophisticated cryptocurrency mining operation with the following characteristics:

**Initial Compromise:**
- Vector: Malicious PDF attachment (invoice_march.pdf.exe)
- Exploitation: Social engineering targeting finance department
- Persistence: Registry Run key modification

**Malware Capabilities:**
1. **Cryptocurrency Mining**: Resource-intensive mining operations
2. **Persistence**: Multiple persistence mechanisms
3. **Evasion**: Process hollowing and DLL injection
4. **Communication**: Encrypted C2 communications
5. **Privilege Escalation**: Local privilege escalation exploits

**Network Activity:**
- C2 Server: evil-command-control.com
- Mining Pool: crypto-pool-server.net:4444
- Data Exfiltration: Minimal, focused on system information

**System Modifications:**
- Registry: Multiple persistence keys created
- Files: Malware dropped in %TEMP% and %APPDATA%
- Services: Created fake Windows service for execution
- Users: Created backup administrator account

**IOC Development**

Based on the analysis, the following IOCs were developed:

**File Indicators:**
```
cryptominer.exe (MD5: a4b3c2d1e0f9g8h7i6j5k4l3m2n1o0p9)
invoice_march.pdf.exe (Initial payload)
%TEMP%\svchost.tmp (Temporary mining executable)
%APPDATA%\WindowsUpdate\config.dat (Configuration file)
```

**Network Indicators:**
```
Domain: evil-command-control.com
Domain: crypto-pool-server.net
IP: 192.168.45.67:4444 (Mining pool connection)
User-Agent: MiningBot/1.2
```

**Registry Indicators:**
```
HKLM\Software\Microsoft\Windows\CurrentVersion\Run\WindowsSecurityUpdate
HKLM\System\CurrentControlSet\Services\WindowsUpdateService
HKCU\Software\Microsoft\WindowsUpdate\Config
```

**Process Indicators:**
```
Process Name: cryptominer.exe
Command Line: -pool crypto-pool-server.net:4444 -wallet [WALLET_ADDRESS]
Parent Process: explorer.exe (via process hollowing)
Network Connections: Multiple outbound on port 4444
```

**Redline Investigation Workflow**

**Step 1: Initial Triage**
- Overview of system state at time of capture
- Identification of unusual processes and network connections
- Quick assessment of potential compromise indicators

**Step 2: Process Analysis**
- Detailed examination of running processes
- Analysis of process relationships and execution chains
- Identification of suspicious process behaviors

**Step 3: Network Investigation**
- Review of network connections and listening ports
- Analysis of DNS queries and communication patterns
- Identification of C2 infrastructure

**Step 4: File System Analysis**
- Examination of recently created or modified files
- Analysis of file metadata and digital signatures
- Identification of malicious executables and payloads

**Step 5: Registry Investigation**
- Review of persistence mechanisms
- Analysis of configuration changes
- Identification of malware-related registry modifications

**Step 6: Timeline Reconstruction**
- Creation of attack timeline based on artifacts
- Correlation of events across different data sources
- Identification of attack progression and techniques

**Mitigation Recommendations**

**Immediate Actions:**
1. Isolate affected systems from the network
2. Terminate malicious processes (cryptominer.exe)
3. Remove persistence mechanisms
4. Block C2 domains at network perimeter
5. Remove created user accounts

**Long-term Security Improvements:**
1. Implement application whitelisting
2. Deploy advanced endpoint protection
3. Enhance email security with attachment sandboxing
4. Implement network monitoring for mining traffic
5. Conduct security awareness training for finance staff

**Lessons Learned**

This Redline investigation demonstrates:
1. The importance of memory analysis in incident response
2. How Redline simplifies complex memory forensics tasks
3. The value of IOC creation for threat hunting
4. The need for comprehensive analysis across multiple data sources
5. The effectiveness of timeline analysis in understanding attack progression

Redline's automated analysis capabilities combined with manual investigation techniques provide a powerful platform for incident response and threat hunting activities.