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

I began by running the query index=* and setting the timeframe to 'All Time' in Splunk to retrieve all available logs. Since Event ID 11 corresponds to FileCreate events triggered when a file is created or overwritten, it’s particularly useful for spotting suspicious file activity. I then filtered the results to only show logs with EventCode=11 to focus on file creation events.

<img width="907" height="518" alt="image" src="https://github.com/user-attachments/assets/f17f46d5-4984-43e4-a2a6-01c41ed7460a" />


Answer: _C:\Users\Administrator\Documents\cmd.exe_


Q2. What is the Sysmon event ID for the related file creation event?

Based on the question 1, we have identified the Event Code for this question.

Answer: _11_


Q3. Can you find the MD5 hash of the ransomware?

I searched the image file for the keyword "MD5", which returned a single result. The MD5 hash was clearly listed in that log.

Image="c:\\Users\\Administrator\\Documents\\cmd.exe" "MD5"

<img width="877" height="507" alt="image" src="https://github.com/user-attachments/assets/a5e7a7b5-0e9c-4413-9227-2217d4b07867" />


Answer: _290c7dfb01e50cea9e19da81a781af2c_


Q4. What file was saved to multiple folder locations?

I removed the keyword "MD5" from the search and focused on the TargetFileName field, which revealed the same file being created in multiple locations.

<img width="770" height="427" alt="image" src="https://github.com/user-attachments/assets/684fa698-6414-4d09-9e8a-7d7a0bc3c5c7" />


Answer: _readme.txt_


Q5. What was the command the attacker used to add a new user to the compromised system?

In Windows, the net user command is used to add new users to a system, following the format:

net user username password /add

I searched for the keyword "/add" in Splunk using the query index=* "/add". This returned four results in the ParentCommandLine field, and the one matching the expected pattern was identified as the relevant command.

<img width="640" height="398" alt="image" src="https://github.com/user-attachments/assets/ac094d35-bb4e-4099-bf67-7f305a14a048" />


Answer: _net user /add securityninja hardToHack123$_


Q6. The attacker migrated the process for better persistence. What is the migrated process image (executable), and what is the original process image (executable) when the attacker got on the system?

Following the hint, I filtered for Sysmon Event ID 8, which resulted in only two log entries. The second log contained the answer, with the SourceImage and TargetImage fields revealing the relevant processes.

<img width="925" height="480" alt="image" src="https://github.com/user-attachments/assets/c263edc0-b875-43a2-b60c-8df49e1f72b2" />


Answer: _C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,C:\Windows\System32\wbem\unsecapp.exe_


Q7. The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?

The answer was found in the first log entry, in the TargetImage field.

<img width="653" height="295" alt="image" src="https://github.com/user-attachments/assets/091cc29d-8324-46de-b035-1c66168432c3" />


Answer: _C:\Windows\System32\lsass.exe_


Q8. What is the web shell the exploit deployed to the system?

The hint indicated checking the IIS logs for POST requests. I changed the sourcetype to iis and filtered the cs_method field for POST.

<img width="836" height="662" alt="image" src="https://github.com/user-attachments/assets/5280a182-d37a-4f58-a9b8-a67ee511ad04" />

Knowing from research that .aspx is a common web shell extension, I searched for "aspx" and examined the cs_uri_stem field to identify the suspicious file.

<img width="642" height="406" alt="image" src="https://github.com/user-attachments/assets/d3637e4a-ce19-457b-87fb-b7666e430251" />


Answer: _i3gfPctK1c2x.aspx_


Q9. What is the command line that executed this web shell?

I searched for the web shell name in the query, and the first log entry contained the CommandLine field with the required answer. 

<img width="1533" height="432" alt="image" src="https://github.com/user-attachments/assets/87b2a419-0acc-42f8-8a7c-1fa6d1559f2d" />


Answer: _attrib.exe  -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx_


Q10. What three CVEs did this exploit leverage? Provide the answer in ascending order.

I Googled about the Conti Ransomware CVEs and came across this link - https://www.cyfirma.com/news/weekly-cyber-security-trends/, which contained the CVEs for Conti Ransomware.

<img width="782" height="117" alt="image" src="https://github.com/user-attachments/assets/ba97d1e6-9ff4-49c1-b303-a2a38eb1b875" />


Answer: _CVE-2018-13374,CVE-2018-13379,CVE-2020-0796_
