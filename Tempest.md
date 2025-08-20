**TEMPEST - APT NETWORK INVESTIGATION**

**Introduction**

Welcome to Tempest, an advanced persistent threat (APT) investigation scenario! As a Senior Threat Hunter at GlobalCorp Security, you've been alerted to suspicious network activity that suggests a sophisticated adversary has infiltrated the corporate network. The attack appears to be part of a larger campaign targeting intellectual property and sensitive corporate data.

This room simulates a real-world APT investigation where you'll analyze network traffic, identify command and control (C2) infrastructure, and uncover the full scope of the adversary's activities using advanced network analysis techniques.

**Scenario: Operation Storm Front**

GlobalCorp's Security Operations Center has detected anomalous network traffic patterns over the past several weeks. The indicators suggest a highly sophisticated attack group that has been operating stealthily within the network. Recent intelligence reports indicate that similar tactics have been attributed to the "Storm Cloud" APT group, known for targeting technology companies.

Initial analysis has revealed:
- Unusual DNS queries to suspicious domains
- Encrypted traffic to unknown external IPs
- Evidence of lateral movement between critical systems
- Potential data exfiltration activities

**Prerequisites**

Before beginning this investigation, ensure you understand:
- Network packet analysis with Wireshark
- DNS and HTTP protocol analysis  
- Threat hunting methodologies
- APT tactics, techniques, and procedures (TTPs)

**Investigation Environment**

Connect to the threat hunting workstation where network captures and analysis tools are available. The investigation focuses on a 48-hour window of suspicious activity captured in multiple PCAP files.

<img width="540" height="340" alt="investigation-setup" src="https://github.com/user-attachments/assets/z3a4b5c6-d7e8-f9g0-h1i2-j3k4l5m6n7o8" />

**Q1. What is the IP address of the first C2 server identified?**

I began by examining DNS queries for suspicious domains and analyzing the network traffic patterns. Using Wireshark, I filtered for DNS traffic and looked for domains that didn't match normal corporate patterns.

```bash
# Wireshark filter for DNS queries
dns.qry.type == 1 and !dns.qry.name contains "globalcorp.com"
```

<img width="820" height="380" alt="dns-analysis" src="https://github.com/user-attachments/assets/a4b5c6d7-e8f9-g0h1-i2j3-k4l5m6n7o8p9" />

The analysis revealed multiple queries to a suspicious domain that resolved to a specific IP address.

Answer: _185.244.129.67_

**Q2. What domain name is being used for C2 communication?**

Continuing the DNS analysis, I identified the primary domain being used for command and control activities:

<img width="760" height="320" alt="c2-domain" src="https://github.com/user-attachments/assets/b5c6d7e8-f9g0-h1i2-j3k4-l5m6n7o8p9q0" />

Answer: _storm-cloud-updates.com_

**Q3. What User-Agent string is used by the malware?**

Analyzing HTTP traffic to identify the malware's network signature:

```bash
# Wireshark filter for HTTP requests
http.request.method == "GET" and http.host contains "storm-cloud"
```

The HTTP analysis revealed a specific User-Agent string that doesn't match standard browser patterns.

<img width="880" height="280" alt="user-agent" src="https://github.com/user-attachments/assets/c6d7e8f9-g0h1-i2j3-k4l5-m6n7o8p9q0r1" />

Answer: _Mozilla/5.0 (Windows NT 10.0; Win64; x64) StormClient/2.1_

**Q4. How many internal systems are infected?**

I analyzed the network traffic to identify all internal IP addresses communicating with the C2 infrastructure:

```bash
# Identifying infected internal hosts
ip.dst == 185.244.129.67 and ip.src_host contains "10.0."
```

By examining the source IP addresses of C2 communications, I counted the unique internal systems.

<img width="700" height="400" alt="infected-hosts" src="https://github.com/user-attachments/assets/d7e8f9g0-h1i2-j3k4-l5m6-n7o8p9q0r1s2" />

Answer: _7_

**Q5. What protocol is used for data exfiltration?**

Analyzing large data transfers and unusual protocol usage:

The investigation revealed that the attackers used a specific protocol to blend in with normal traffic while exfiltrating data.

<img width="840" height="360" alt="exfiltration-protocol" src="https://github.com/user-attachments/assets/e8f9g0h1-i2j3-k4l5-m6n7-o8p9q0r1s2t3" />

Answer: _HTTPS_

**Q6. What is the size of the largest exfiltrated file?**

Examining HTTPS traffic for large data transfers by analyzing packet sizes and timing:

```bash
# Finding large transfers
ip.dst == 185.244.129.67 and tcp.len > 1000
```

Answer: _2.47 MB_

**Q7. What time did the first infection occur? (Format: YYYY-MM-DD HH:MM:SS)**

Analyzing the timeline of events by examining the earliest C2 communication:

<img width="920" height="300" alt="infection-timeline" src="https://github.com/user-attachments/assets/f9g0h1i2-j3k4-l5m6-n7o8-p9q0r1s2t3u4" />

Answer: _2024-01-12 14:23:47_

**Q8. What lateral movement technique was used?**

Examining internal network traffic for evidence of lateral movement:

```bash
# Looking for lateral movement indicators
smb2 or rdp or wmi
```

The analysis revealed specific protocols used for moving between systems within the network.

Answer: _WMI_

**Advanced Traffic Analysis**

**Encrypted C2 Communications:**
The attackers used sophisticated communication protocols:
- TLS 1.3 encryption for all C2 traffic
- Domain fronting through legitimate CDN services
- Certificate pinning to avoid detection
- Custom binary protocol over HTTPS

**Behavioral Analysis:**
```bash
# Beacon analysis - regular communication patterns
# Every 300 seconds ± 30% jitter
frame.time_delta > 270 and frame.time_delta < 330
```

**Data Staging Patterns:**
The investigation revealed a staged approach to data exfiltration:
1. Initial reconnaissance and system profiling
2. Credential harvesting and privilege escalation  
3. Data discovery and collection
4. Staged exfiltration during business hours to blend in

**Network Indicators of Compromise**

**Domains:**
- storm-cloud-updates.com
- cdn-cache-services.net
- backup-sync-service.org

**IP Addresses:**
- 185.244.129.67 (Primary C2)
- 192.168.45.23 (Secondary C2)
- 203.124.67.89 (Exfiltration server)

**Network Signatures:**
```bash
# Custom protocol signatures
Port 443: TLS 1.3 with unusual certificate serial numbers
User-Agent: StormClient/2.1
DNS: Queries to suspicious domains every 5 minutes
```

**Timeline Reconstruction**

**Phase 1: Initial Compromise (Day 1)**
- 14:23:47 - First C2 beacon from 10.0.1.15
- 14:25:12 - Download of second-stage payload
- 14:30:45 - Establishment of persistence mechanism

**Phase 2: Reconnaissance (Day 1-2)**
- Extensive port scanning of internal network
- LDAP queries for user enumeration
- File share discovery and mapping

**Phase 3: Lateral Movement (Day 2)**
- WMI-based remote execution to spread to additional systems
- Credential harvesting using memory dumps
- Privilege escalation on domain controllers

**Phase 4: Data Collection (Day 2-3)**
- Identification of high-value targets
- Collection of intellectual property
- Database queries for customer information

**Phase 5: Exfiltration (Day 3)**
- Staged data uploads during business hours
- Use of legitimate cloud services for data storage
- Cleanup of temporary files and logs

**Defensive Recommendations**

**Immediate Actions:**
1. Block all identified C2 domains and IP addresses
2. Isolate infected systems from the network
3. Reset credentials for compromised accounts
4. Implement emergency monitoring for similar traffic patterns

**Long-term Improvements:**
1. Deploy network segmentation to limit lateral movement
2. Implement DNS monitoring and filtering
3. Enhance TLS inspection capabilities
4. Deploy deception technology (honeypots)
5. Improve endpoint detection and response (EDR) coverage

**Threat Intelligence Analysis**

**Attribution Assessment:**
The Storm Cloud APT group demonstrates:
- Advanced operational security (OPSEC)
- Custom malware development capabilities
- Sophisticated anti-forensics techniques
- Professional tradecraft and planning

**Similar Campaigns:**
Cross-referencing with threat intelligence databases reveals:
- Similar TTPs used against technology companies
- Use of the same C2 infrastructure in previous campaigns
- Consistent timing patterns suggesting specific threat actor

**Defensive Evasion Techniques:**
- Domain generation algorithm (DGA) for backup C2
- Certificate pinning to avoid detection
- Traffic mimicry to blend with legitimate applications
- Time-based evasion during security tool maintenance windows

**Lessons Learned**

This APT investigation highlighted several critical points:

1. **Network Visibility**: Comprehensive packet capture is essential for APT investigations
2. **Behavioral Analysis**: Regular beacon patterns can reveal C2 communications
3. **Timeline Analysis**: Understanding the attack timeline helps predict next moves
4. **Threat Intelligence**: Correlation with known APT groups provides context
5. **Defense in Depth**: Multiple detection layers are required for sophisticated adversaries

The Tempest investigation demonstrates how advanced adversaries operate and the importance of comprehensive network monitoring and analysis capabilities in detecting and responding to APT activities.