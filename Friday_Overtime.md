Disclaimer

Please note: The artefacts used in this scenario were retrieved from a real-world cyber-attack. Hence, it is advised that interaction with the artefacts be done only inside the attached VM, as it is an isolated environment. 

<img width="563" height="397" alt="image" src="https://github.com/user-attachments/assets/6194c516-8e2a-467c-bf8c-231a84457ac3" />

Hello Busy Weekend. . .

It's a Friday evening at PandaProbe Intelligence when a notification appears on your CTI platform. While most are already looking forward to the weekend, you realise you must pull overtime because SwiftSpend Finance has opened a new ticket, raising concerns about potential malware threats. The finance company, known for its meticulous security measures, stumbled upon something suspicious and wanted immediate expert analysis.

As the only remaining CTI Analyst on shift at PandaProbe Intelligence, you quickly took charge of the situation, realising the gravity of a potential breach at a financial institution. The ticket contained multiple file attachments, presumed to be malware samples.

With a deep breath, a focused mind, and the longing desire to go home, you began the process of:

    Downloading the malware samples provided in the ticket, ensuring they were contained in a secure environment.
    Running the samples through preliminary automated malware analysis tools to get a quick overview.
    Deep diving into a manual analysis, understanding the malware's behaviour, and identifying its communication patterns.
    Correlating findings with global threat intelligence databases to identify known signatures or behaviours.
    Compiling a comprehensive report with mitigation and recovery steps, ensuring SwiftSpend Finance could swiftly address potential threats.

Connecting to the machine

Start the virtual machine in split-screen view by clicking the green Start Machine button on the upper right section of this task. If the VM is not visible, use the blue Show Split View button at the top-right of the page. Additionally, you can open the DocIntel platform using the credentials below.

<img width="446" height="162" alt="image" src="https://github.com/user-attachments/assets/3d77f7f8-b2db-4482-8740-e23cf66c43c5" />

Note: While the web browser (i.e., Chromium) will immediately start after boot up, it may show a tab that has a "502 Bad Gateway" error message displayed. This is because the DocIntel platform takes about 5 more minutes to finish starting up after the VM has completely booted up. After 5 minutes, you can refresh the page in order to view the login page. We appreciate your patience. The ticket details can be found by logging in to the DocIntel platform. OSINT, a web browser, and a text editor outside the VM will also help. 


**Q1. Who shared the malware samples?**

I logged into the Panda Probe intelligence portal using the Chromium browser. Inside the mailbox, I located the email containing the shared malware samples, which also revealed the sender.

<img width="867" height="185" alt="image" src="https://github.com/user-attachments/assets/6218d8a1-72c4-48a0-8c4d-719bf72b8595" />


Answer: _Oliver Bennett_


**Q2. What is the SHA1 hash of the file "pRsm.dll" inside samples.zip?**

I downloaded the samples.zip attachment from the email, extracted its contents, and identified the file pRsm.dll. To obtain the SHA1 hash, I ran the sha1sum command.

<img width="570" height="78" alt="image" src="https://github.com/user-attachments/assets/91c9afcd-0f58-4580-9761-d2a400a08ebb" />


Answer: _9d1ecbbe8637fed0d89fca1af35ea821277ad2e8_


**Q3. Which malware framework utilizes these DLLs as add-on modules?**

To analyze the extracted DLL, I searched its SHA1 hash on VirusTotal. The results showed that the DLLs were associated with the MgBot malware framework, which commonly uses DLLs as add-on modules.

<img width="1312" height="578" alt="image" src="https://github.com/user-attachments/assets/782915ef-ea39-45de-b87d-e942a4e4a5ef" />


Answer: _MgBot_



**Q4. Which MITRE ATT&CK Technique is linked to using pRsm.dll in this malware framework?**

I conducted an open-source search on Google for the hash pRsm.dll. This led me to a report on WeLiveSecurity that detailed the malware’s behavior and listed the MITRE ATT&CK technique it employs. 
Filtering the report for pRsm.dll revealed the relevant technique ID.

<img width="712" height="107" alt="image" src="https://github.com/user-attachments/assets/21a1ee55-0416-4c9d-85c6-9e77802f8490" />


Answer: _T1123_



**Q5. What is the CyberChef defanged URL of the malicious download location first seen on 2020-11-02?**

By checking the timeline of reported events, I found the malicious download URL. To prevent accidental clicks, I processed the URL through CyberChef to defang it.

<img width="791" height="377" alt="image" src="https://github.com/user-attachments/assets/14272484-60f5-4736-b11e-05f08b35ac5f" />


Answer: _hxxp[://]update[.]browser[.]qq[.]com/qmbs/QQ/QQUrlMgr_QQ88_4296[.]exe_




**Q6. What is the CyberChef defanged IP address of the C&C server first detected on 2020-09-14 using these modules?**

I located the C2 server information by referencing the timeline for 2020-09-14. I then defanged the IP address using CyberChef to ensure safe handling.

<img width="790" height="188" alt="image" src="https://github.com/user-attachments/assets/ab49f926-88f4-4d0f-92a5-81ef87640a4b" />


Answer: _122[.]10[.]90[.]12_



**Q7. What is the md5 hash of the spyagent family spyware hosted on the same IP targeting Android devices in Jun 2025?**

I ran the IP address through VirusTotal, which revealed additional malware hosted on the same server. Among the detections was an Android spyware sample belonging to the SpyAgent family, along with its MD5 hash.

<img width="930" height="652" alt="image" src="https://github.com/user-attachments/assets/a9e1df59-29e4-4257-bacf-42aefbfef1af" />


Answer: _951F41930489A8BFE963FCED5D8DFD79_
