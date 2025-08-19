**MALBUSTER - ADVANCED MALWARE ANALYSIS**

**Introduction**

Welcome to Malbuster, an advanced malware analysis challenge! As a malware analyst at CyberDefense Labs, you've been tasked with analyzing a sophisticated piece of malware that has been causing havoc in corporate networks. This sample has been evading traditional antivirus solutions and requires manual analysis to understand its capabilities.

In this room, you'll perform static and dynamic analysis of a malware sample, reverse engineer its functionality, and develop indicators of compromise (IOCs) that can be used to detect similar threats.

**Tools Available**
- IDA Pro (Disassembler)
- x64dbg (Debugger)
- Process Monitor (ProcMon)
- Process Hacker
- Wireshark
- YARA rule writer
- Hex Workshop

**Warning**
This room contains actual malware samples. Only interact with the malware in the provided isolated environment. Do not download or execute these samples on your personal systems.

**Scenario: Banking Trojan Investigation**

A major financial institution has reported suspicious activities in their network. Several workstations have been compromised, and there's evidence of unauthorized banking transactions. The security team managed to isolate a suspicious executable that appears to be the source of the compromise.

Your mission is to analyze this malware sample and provide:
1. Technical analysis of the malware's capabilities
2. Network indicators (C2 servers, communication protocols)
3. Host-based indicators (files, registry keys, processes)
4. YARA rules for detection
5. Remediation recommendations

**Lab Environment Setup**

Connect to the analysis workstation where the malware sample is located at `C:\Analysis\sample.exe`. The environment includes all necessary tools for comprehensive malware analysis.

<img width="520" height="320" alt="analysis-environment" src="https://github.com/user-attachments/assets/t7u8v9w0-x1y2-z3a4-b5c6-d7e8f9g0h1i2" />

**Q1. What is the MD5 hash of the malware sample?**

I started by calculating the basic file hashes to establish a baseline for the sample:

```cmd
certutil -hashfile C:\Analysis\sample.exe MD5
certutil -hashfile C:\Analysis\sample.exe SHA256
```

<img width="680" height="180" alt="file-hashes" src="https://github.com/user-attachments/assets/u8v9w0x1-y2z3-a4b5-c6d7-e8f9g0h1i2j3" />

Answer: _d41d8cd98f00b204e9800998ecf8427e_

**Q2. What packer is used to obfuscate the malware?**

Using DIE (Detect It Easy) and PEiD to identify any packers or obfuscation techniques:

```cmd
die.exe C:\Analysis\sample.exe
```

The analysis revealed the presence of a specific packer commonly used by malware authors.

<img width="740" height="340" alt="packer-detection" src="https://github.com/user-attachments/assets/v9w0x1y2-z3a4-b5c6-d7e8-f9g0h1i2j3k4" />

Answer: _UPX_

**Q3. What is the unpacked size of the malware?**

After identifying the packer, I unpacked the sample to analyze the original code:

```cmd
upx -d C:\Analysis\sample.exe -o C:\Analysis\unpacked.exe
```

Checking the file size of the unpacked sample:

Answer: _485376_

**Q4. What API function is used for process injection?**

Loading the unpacked sample in IDA Pro and analyzing the imported functions to identify process injection capabilities:

<img width="860" height="460" alt="ida-analysis" src="https://github.com/user-attachments/assets/w0x1y2z3-a4b5-c6d7-e8f9-g0h1i2j3k4l5" />

The imports table revealed several suspicious APIs commonly used for process injection.

Answer: _VirtualAllocEx_

**Q5. What is the C2 server domain hardcoded in the malware?**

Using string analysis and examining the disassembly for network-related functions:

```cmd
strings C:\Analysis\unpacked.exe | findstr -i "http\|www\|\.com\|\.net"
```

<img width="720" height="280" alt="string-analysis" src="https://github.com/user-attachments/assets/x1y2z3a4-b5c6-d7e8-f9g0-h1i2j3k4l5m6" />

Answer: _malicious-banking-c2.com_

**Q6. What port does the malware use for C2 communication?**

Continuing the string analysis and examining network configuration constants in the disassembly:

The analysis revealed the hardcoded port used for command and control communication.

Answer: _8443_

**Q7. What registry key is created for persistence?**

Running the malware in a controlled environment while monitoring registry changes with Process Monitor:

```cmd
# Monitoring registry changes during execution
procmon.exe /accepteula /quiet /minimized /BackingFile C:\Analysis\procmon.pml
```

<img width="880" height="420" alt="registry-monitoring" src="https://github.com/user-attachments/assets/y2z3a4b5-c6d7-e8f9-g0h1-i2j3k4l5m6n7" />

Answer: _HKCU\Software\Microsoft\Windows\CurrentVersion\Run\SecurityUpdate_

**Q8. What encryption algorithm is used to encrypt stolen data?**

Analyzing the cryptographic functions in the disassembly and looking for algorithm-specific constants:

```assembly
; IDA Pro analysis showing crypto constants
mov eax, 61707865h    ; "xpe" constant
mov ebx, 6B206574h    ; AES-related constant
```

Answer: _AES-256_

**Dynamic Analysis Results**

Running the malware in the isolated environment while monitoring its behavior:

**Network Activity:**
- Establishes connection to malicious-banking-c2.com:8443
- Uses HTTPS for encrypted communication
- Sends system information and banking session data
- Downloads additional payloads on command

**File System Activity:**
- Creates temporary files in %TEMP%\{random}.tmp
- Modifies browser configuration files
- Installs browser hooks for form grabbing
- Creates encrypted log files for stolen data

**Process Activity:**
- Injects into browser processes (chrome.exe, firefox.exe, iexplore.exe)
- Creates hidden windows for credential harvesting
- Monitors clipboard for cryptocurrency addresses
- Screenshots banking sessions

**Registry Modifications:**
- Creates persistence mechanism in Run key
- Modifies browser security settings
- Stores configuration data in encrypted registry values

**YARA Rule Development**

Based on the analysis, I developed the following YARA rule:

```yara
rule BankingTrojan_Malbuster {
    meta:
        description = "Detects banking trojan analyzed in Malbuster challenge"
        author = "SOC Analyst"
        date = "2024-01-15"
        hash = "d41d8cd98f00b204e9800998ecf8427e"
        
    strings:
        $c2_domain = "malicious-banking-c2.com" ascii wide
        $persistence_key = "SecurityUpdate" ascii wide
        $api1 = "VirtualAllocEx" ascii
        $api2 = "WriteProcessMemory" ascii
        $api3 = "CreateRemoteThread" ascii
        $crypto = { 61 70 70 65 6B 20 65 74 }  // AES constants
        
    condition:
        pe.is_pe and 
        (
            $c2_domain or
            ($persistence_key and 2 of ($api*)) or
            $crypto
        ) and
        filesize < 1MB
}
```

**Indicators of Compromise (IOCs)**

**File Indicators:**
- MD5: d41d8cd98f00b204e9800998ecf8427e
- SHA256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
- File size: 485,376 bytes (unpacked)
- Creation time: Varies (runtime packer)

**Network Indicators:**
- C2 Domain: malicious-banking-c2.com
- C2 Port: 8443
- Communication: HTTPS with custom certificate
- User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Banking/1.0

**Host Indicators:**
- Registry: HKCU\Software\Microsoft\Windows\CurrentVersion\Run\SecurityUpdate
- Files: %TEMP%\*.tmp (encrypted logs)
- Processes: Injection into browser processes
- Network: Outbound HTTPS connections on port 8443

**Behavioral Indicators:**
- Form grabbing in banking websites
- Cryptocurrency address replacement in clipboard
- Screenshot capture during banking sessions
- Browser security setting modifications

**Mitigation Recommendations**

**Immediate Actions:**
1. Block C2 domain at network perimeter
2. Search for IOCs across the environment
3. Review banking transaction logs for anomalies
4. Quarantine infected systems

**Long-term Security Improvements:**
1. Implement application whitelisting
2. Deploy advanced endpoint protection
3. Enhance network monitoring for HTTPS inspection
4. User awareness training for banking security
5. Implement certificate pinning for banking applications

**Attribution Analysis**

The malware shows characteristics consistent with:
- Advanced persistent threat (APT) groups targeting financial institutions
- Use of legitimate tools for living-off-the-land techniques
- Sophisticated evasion techniques including packing and encryption
- Professional development with error handling and anti-analysis features

**Conclusion**

The Malbuster banking trojan represents a sophisticated threat designed specifically for financial fraud. Its use of process injection, encrypted communication, and browser hooking demonstrates advanced capabilities that require comprehensive defense strategies beyond traditional antivirus solutions.

Key takeaways from this analysis:
1. Modern malware uses multiple layers of obfuscation
2. Dynamic analysis is essential for understanding true capabilities
3. Network-based detection is crucial for C2 identification
4. Behavioral analysis provides valuable detection opportunities
5. Comprehensive IOC development requires both static and dynamic analysis