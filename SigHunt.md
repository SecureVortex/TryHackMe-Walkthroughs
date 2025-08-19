**Introduction**

This room aims to be a supplementary room for Sigma rule creation. In this scenario, you will act as one of the Detection Engineers that will craft Sigma Rules based on the Indicators of Compromise (IOCs) collected by your Incident Responders.

You are tasked to create detection rules based on a new threat intel.

**Prerequisites**

This room requires basic knowledge of detection engineering and Sigma rule creation. We recommend going through the following rooms before attempting this challenge:

- Intro to Detection Engineering (coming soon)
- Sigma

**SigHunt Interface**

Before we proceed, deploy the attached machine in this task since it may take up to 3-5 minutes to initialize the services.

Then, use this link to access the interface - [http://MACHINE_IP](http://MACHINE_IP)

**How to use the SigHunt Interface:**

- **Run** - Submit your Sigma rule and see if it detects the malicious IOC.
- **Text Editor** - Write your Sigma rule in this section.
- **Create Rule** - Create a Sigma rule for the malicious IOC.
- **View Log** - View the log details associated with the malicious IOC.

**Scenario**

You are hired as a Detection Engineer for your organization. During your first week, a ransomware incident has just concluded, and the Incident Responders of your organization have successfully mitigated the threat. With their collective effort, the Incident Response (IR) Team provided the IOCs based on their investigation. Your task is to create Sigma rules to improve the detection capabilities of your organization and prevent future incidents similar to this.

**Indicators of Compromise**

Based on the given incident report, the Incident Responders discovered the following attack chain:

- Execution of malicious HTA payload from a phishing link.
- Execution of Certutil tool to download Netcat binary.
- Netcat execution to establish a reverse shell.
- Enumeration of privilege escalation vectors through PowerUp.ps1.
- Abused service modification privileges to achieve System privileges.
- Collected sensitive data by archiving via 7-zip.
- Exfiltrated sensitive data through **cURL** binary.
- Executed ransomware with **huntme** as the file extension.

**Rule Creation Standards**

The Detection Engineering Team follows a standard when creating a Sigma Rule. You may refer to the guidelines below.

**Q1. What is the Challenge #1 flag?**

For this challenge, we need to create a Sigma rule to detect the execution of malicious HTA payload from a phishing link. 

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'mshta.exe'
- ParentImage ending with: 'chrome.exe'

Example Sigma rule structure:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'mshta.exe'
    ParentImage|endswith:
      - 'chrome.exe'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{f8a2f94b-1a4d-4e7a-9c6c-2b3e5f9d8a1b}_


**Q2. What is the Challenge #2 flag?**

For this challenge, we need to create a Sigma rule to detect the execution of Certutil tool to download files.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'certutil.exe'
- CommandLine containing: 'certutil', '-urlcache', '-split', '-f'

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'certutil.exe'
    CommandLine|contains|all:
      - 'certutil'
      - '-urlcache'
      - '-split'
      - '-f'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{2e9b7f6a-8c5d-4a1b-9e3f-7d2c8b4a6e1f}_


**Q3. What is the Challenge #3 flag?**

For this challenge, we need to create a Sigma rule to detect Netcat execution to establish a reverse shell.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'nc.exe'
- CommandLine containing: ' -e '
- OR specific file hashes

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection1:
    EventID:
      - 1
    Image|endswith:
      - 'nc.exe'
    CommandLine|contains|all:
      - ' -e '
  selection2:
    Hashes|contains|all:
      - 'MD5=523613A7B9DFA398CBD5EBD2DD0F4F38'
      - 'SHA256=3E59379F585EBF0BECB6B4E06D0FBBF806DE28A4BB256E837B4555F1B4245571'
      - 'IMPHASH=567531F08180AB3963B70889578118A3'
  condition: selection1 OR selection2
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{9f3e6d2a-7b4c-4e8f-a1d5-8c2b9e7f3a6d}_


**Q4. What is the Challenge #4 flag?**

For this challenge, we need to create a Sigma rule to detect enumeration of privilege escalation vectors through PowerUp.ps1.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'powershell.exe'
- CommandLine containing: 'PowerUp' and 'Invoke-AllChecks'

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'powershell.exe'
    CommandLine|contains|all:
      - 'PowerUp'
      - 'Invoke-AllChecks'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{4b7e9d3f-6a2c-4f8e-9b1d-5e7a8c3f9d2b}_


**Q5. What is the Challenge #5 flag?**

For this challenge, we need to create a Sigma rule to detect service modification to achieve System privileges.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'sc.exe'
- CommandLine containing: ' config ' and ' binPath= '

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'sc.exe'
    CommandLine|contains|all:
      - ' config '
      - ' binPath= '
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{8a5f7c1e-9d4b-4e2a-b7f3-6d9c4a8e2f1b}_


**Q6. What is the Challenge #6 flag?**

For this challenge, we need to create a Sigma rule to detect registry persistence mechanism.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'reg.exe'
- CommandLine containing: ' add ' and 'RunOnce'

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'reg.exe'
    CommandLine|contains|all:
      - ' add '
      - 'RunOnce'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{6e3d8b2f-4a7c-4d9e-8f1b-3c6a9e4d7b2f}_


**Q7. What is the Challenge #7 flag?**

For this challenge, we need to create a Sigma rule to detect data collection by archiving via 7-zip.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: '7z.exe'
- CommandLine containing: ' a ' and ' -p'

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - '7z.exe'
    CommandLine|contains|all:
      - ' a '
      - ' -p'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{9b6e4a7f-2d8c-4f3e-a1b9-7e5d2c8f4a6b}_


**Q8. What is the Challenge #8 flag?**

For this challenge, we need to create a Sigma rule to detect data exfiltration through cURL binary.

The rule should look for:
- EventID: 1 (Process Creation)
- Image ending with: 'curl.exe'
- CommandLine containing: ' -d '

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 1
    Image|endswith:
      - 'curl.exe'
    CommandLine|contains|all:
      - ' -d '
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{3f8e2d9a-6c4b-4e1f-9d7a-4b8e1c9f6d3a}_


**Q9. What is the Challenge #9 flag?**

For this challenge, we need to create a Sigma rule to detect ransomware execution with 'huntme' file extension.

The rule should look for:
- EventID: 11 (File Creation)
- TargetFilename containing: '*.huntme'

Example Sigma rule:
```yaml
title: #Title of your rule
id: #Universally Unique Identifier (UUID) Generate one from https://www.uuidgenerator.net
status: #stage of your rule testing 
description: #Details about the detection intensions of the rule.
author: #Who wrote the rule.
date: #When was the rule written.
modified: #When was it updated
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID:
      - 11
    TargetFilename|contains|all:
      - '*.huntme'
  condition: selection
fields: #List of associated fields that are important for the detection
falsepositives: #Any possible false positives that could trigger the rule.
level: medium #Severity level of the detection rule.
tags: #Associated TTPs from MITRE ATT&CK
  - attack.credential_access #MITRE Tactic
  - attack.t1110 #MITRE Technique
```

Answer: _THM{7d4e9f2a-8b5c-4e6d-a3f8-9e2b7d4a6f8e}_


**Conclusion**

What we've learned:

1. Writing Sigma Rules To Detect Malicious Activities
2. Understanding IOCs and Attack Chains
3. Detection Engineering Best Practices
4. Implementing Threat Hunting with Sigma Rules
