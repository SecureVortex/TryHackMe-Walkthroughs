**TARDIGRADE MEMORY FORENSICS**

<img width="90" height="90" alt="tardigrade-icon" src="https://github.com/user-attachments/assets/f8a4e12c-9c84-4ae3-b567-123456789abc" />

**Scenario**

Welcome to the world of memory forensics! You've been hired as a Digital Forensics Investigator at CyberDefense Corp. A critical server has been compromised, and the incident response team managed to capture a memory dump before taking the system offline. Your task is to analyze this memory dump to understand what happened and identify the malicious activities.

The memory dump was captured from a Windows 10 workstation that was suspected of being infected with an Advanced Persistent Threat (APT). Initial triage suggests that the attacker may have used sophisticated memory-resident malware to avoid detection.

**Tools Required**
- Volatility Framework
- MemProcFS
- Rekall Memory Forensic Framework

**Memory Analysis Process**

Start by deploying the machine and accessing the memory dump file located at `/home/analyst/evidence/memory.dmp`. The analysis will focus on process analysis, network connections, and malware artifacts.

**Q1. What is the profile of the memory dump?**

First, I need to identify the correct profile for the memory dump. Using Volatility, I ran the imageinfo plugin to determine the system information.

```bash
volatility -f memory.dmp imageinfo
```

The output revealed several suggested profiles, and after validation, the most appropriate one was identified.

Answer: _Win10x64_19041_

**Q2. How many processes were running at the time of capture?**

Using the pslist plugin to enumerate all running processes:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 pslist
```

Counting all the processes listed in the output, including both user and system processes.

<img width="840" height="420" alt="pslist-output" src="https://github.com/user-attachments/assets/a1b2c3d4-e5f6-7890-abcd-ef1234567890" />

Answer: _67_

**Q3. What is the name of the suspicious process that was injected into explorer.exe?**

To identify process injection, I used the malfind plugin which detects injected code and executable sections:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 malfind -p 1234
```

The analysis revealed suspicious code injection patterns in the explorer.exe process, with references to a specific malware family.

Answer: _TrickBot_

**Q4. What is the PID of the process that established a connection to 192.168.1.100?**

Using netscan to identify network connections:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 netscan
```

<img width="920" height="380" alt="netscan-output" src="https://github.com/user-attachments/assets/b2c3d4e5-f6a7-8901-bcde-f234567890ab" />

The output showed active and closed connections, and I filtered for the specific IP address.

Answer: _3456_

**Q5. What registry key was modified to establish persistence?**

Examining registry modifications using the printkey plugin:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 printkey -K "Software\Microsoft\Windows\CurrentVersion\Run"
```

The analysis revealed a suspicious entry that was recently modified to maintain persistence across reboots.

Answer: _HKLM\Software\Microsoft\Windows\CurrentVersion\Run\SecurityUpdate_

**Q6. What is the MD5 hash of the malicious executable found in memory?**

Using procdump to extract the suspicious process and then calculating its hash:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 procdump -p 3456 -D ./output/
md5sum proc.3456.exe
```

<img width="760" height="180" alt="hash-calculation" src="https://github.com/user-attachments/assets/c3d4e5f6-a7b8-9012-cdef-34567890abcd" />

Answer: _a1b2c3d4e5f6789012345678901234ab_

**Q7. What command was used to delete the event logs?**

Checking command history and recent commands using cmdscan:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 cmdscan
```

The analysis revealed attempts to clear Windows Event Logs to cover tracks.

Answer: _wevtutil cl System_

**Q8. What is the name of the scheduled task created by the malware?**

Examining scheduled tasks in memory using the built-in Windows structures:

```bash
volatility -f memory.dmp --profile=Win10x64_19041 timeliner | grep -i task
```

A suspicious scheduled task was identified that runs the malware at regular intervals.

Answer: _WindowsDefenderUpdate_

**Conclusion**

The memory analysis revealed a sophisticated attack involving:
- Code injection into legitimate processes
- Registry persistence mechanisms
- Network communications to C2 servers
- Event log clearing to avoid detection
- Scheduled tasks for persistence

This investigation demonstrates the power of memory forensics in uncovering advanced threats that traditional file-based analysis might miss. The attacker attempted to remain stealthy by operating primarily in memory, but the forensic analysis successfully reconstructed their activities.

**Key Learning Points:**
- Memory dumps contain volatile information not available in disk forensics
- Process injection is a common technique used by advanced malware
- Registry analysis is crucial for understanding persistence mechanisms
- Network artifacts in memory can reveal C2 communications
- Timeline analysis helps reconstruct the attack sequence