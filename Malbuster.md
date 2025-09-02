This room aims to be a practice room for Dissecting PE Headers and Static Analysis 1. In this scenario, you will act as one of the Reverse Engineers that will analyse malware samples based on the detections reported by your SOC team.

Prerequisites

This room requires basic knowledge of Malware Static Analysis. We recommend going through the following rooms before attempting this challenge.

Intro to Malware Analysis
Dissecting PE Headers
Basic Static Analysis
﻿Scenario

You are currently working as a Malware Reverse Engineer for your organisation. Your team acts as a support for the SOC team when detections of unknown binaries occur. One of the SOC analysts triaged an alert triggered by binaries with unusual behaviour. Your task is to analyse the binaries detected by your SOC team and provide enough information to assist them in remediating the threat.

Investigation Platforms  

<img width="921" height="560" alt="image" src="https://github.com/user-attachments/assets/0baadb6a-f032-4784-b934-eb8670219550" />

The team has provided two investigation platforms, a FLARE VM and a REMnux VM. You may utilise the machines based on your preference.﻿

If you prefer FLARE VM, you may start the machine attached to this task. Else, you may start the machine on the task below to start REMnux VM.

The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

You may also use the following credentials for alternative access via Remote Desktop (RDP):

<img width="661" height="431" alt="image" src="https://github.com/user-attachments/assets/741d0043-87d6-471c-aa20-c8d9275fe161" />

Lastly, you may find the malware samples on C:\Users\Administrator\Desktop\Samples. 

WE ADVISE YOU NOT TO DOWNLOAD THE MALWARE SAMPLES TO YOUR HOST.

**Challenge Questions**

﻿Investigation Platform

If you prefer REMnux, you may use the machine attached to this task by accessing it via the split-screen view.

Else, start the machine from the previous task to spin up the FLARE VM.

In addition, you can find the malware samples provided by the SOC team at /home/ubuntu/Desktop/Samples. 

The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

WE ADVISE YOU NOT TO DOWNLOAD THE MALWARE SAMPLES TO YOUR HOST.

Good luck!﻿



**Q1. Based on the ARCHITECTURE of the binary, is malbuster_1 a 32-bit or a 64-bit application? (32-bit/64-bit)**

Answer: _32-bit_


**Q2. What is the MD5 hash of malbuster_1?**

Answer: _4348da65e4aeae6472c7f97d6dd8ad8f_


**Q3. Using the hash, what is the popular threat label of malbuster_1 according to VirusTotal?**

Amswer: **trojan.zbot/razy**


**Q4. Based on VirusTotal detection, what is the malware signature of malbuster_2 according to Avira?**

Answer: _HEUR/AGEN.1306860_


**Q5. malbuster_2 imports the function _CorExeMain. From which DLL file does it import this function?**

Answer: _mscoree.dll_


**Q6. Based on the VS_VERSION_INFO header, what is the original name of malbuster_2?**

Answer: _7JYpE.exe_


**Q7. Using the hash of malbuster_3, what is its malware signature based on abuse.ch?**

Answer: _TrickBot_


**Q8. Using the hash of malbuster_4, what is its malware signature based on abuse.ch?**

Answer: _Zloader_


**Q9. What is the message found in the DOS_STUB of malbuster_4?**

Answer: _!This Salfram cannot be run in DOS mode._


**Q10. malbuster_4 imports the function ShellExecuteA. From which DLL file does it import this function?**

Answer: _shell32.dll_


**Q11. Using capa, how many anti-VM instructions were identified in malbuster_1?**

Answer: _3_


**Q12. Using capa, which binary can log keystrokes?**

Answer: _malbuster_3_


**Q13. Using capa, what is the MITRE ID of the DISCOVERY technique used by malbuster_4?**

Answer: _T1083_


**Q14. Which binary contains the string GodMode?**

Amswer: _malbuster_2_


**Q15. Which binary contains the string Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)?**

Answer: _malbuster_1_










































