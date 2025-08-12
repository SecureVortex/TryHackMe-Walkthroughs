**SITREP**

Some employees from your company reported that they can’t log into Outlook. The Exchange system admin also reported that he can’t log in to the Exchange Admin Center. After initial triage, they discovered some weird readme files settled on the Exchange server.  
Below is a copy of the ransomware note.

<img width="1128" height="375" alt="15db974b80239a7eb2c52fb26c458933" src="https://github.com/user-attachments/assets/912b2047-5b80-43e0-bc99-2b0cde56028f" />

Warning: Do NOT attempt to visit and/or interact with any URLs displayed in the ransom note. 
Read the latest on the Conti ransomware here. 
Connect to OpenVPN or use the AttackBox to access the attached Splunk instance. 
Splunk Interface Credentials:
Username: bellybear
Password: password!!!
Splunk URL: http://MACHINE_IP:8000

**Exchange Server Compromised**

Below are the error messages that the Exchange admin and employees see when they try to access anything related to Exchange or Outlook.

Exchange Control Panel:

<img width="1896" height="297" alt="214468bd3cc7466762b2358993bb3069" src="https://github.com/user-attachments/assets/66bed27d-cf73-47bd-919e-b44879c02f62" />


Outlook Web Access:

<img width="1853" height="446" alt="c7d87fb962d961d81502f02dd1fdba77" src="https://github.com/user-attachments/assets/c0c619cb-128c-4936-ac88-675c9f47536b" />


Task: You are assigned to investigate this situation. Use Splunk to answer the questions below regarding the Conti ransomware. 

Q1. Can you identify the location of the ransomware?
Answer: _C:\Users\Administrator\Documents\cmd.exe_

Q2. What is the Sysmon event ID for the related file creation event?

Answer: _11_


Q3. Can you find the MD5 hash of the ransomware?

Answer: _290c7dfb01e50cea9e19da81a781af2c_


Q4. What file was saved to multiple folder locations?

Answer: _readme.txt_


Q5. What was the command the attacker used to add a new user to the compromised system?

Answer: _net user /add securityninja hardToHack123$_


Q6. The attacker migrated the process for better persistence. What is the migrated process image (executable), and what is the original process image (executable) when the attacker got on the system?

Answer: _C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,C:\Windows\System32\wbem\unsecapp.exe_


Q7. The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?

Answer: _C:\Windows\System32\lsass.exe_


Q8. What is the web shell the exploit deployed to the system?

Answer: _i3gfPctK1c2x.aspx_


Q0. What is the command line that executed this web shell?

Answer: _attrib.exe  -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx_


Q10. What three CVEs did this exploit leverage? Provide the answer in ascending order.

Answer: _CVE-2018-13374,CVE-2018-13379,CVE-2020-0796_






