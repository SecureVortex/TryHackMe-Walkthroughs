**Ransomware or not**

<img width="741" height="421" alt="task2" src="https://github.com/user-attachments/assets/38b6bed2-101f-4bc1-88da-6c2be78aef35" />

Scenario : You are a SOC Analyst for an MSSP (Managed Security Service Provider) company called TryNotHackMe .
A customer sent an email asking for an analyst to investigate the events that occurred on Keegan's machine on Monday, May 16th, 2022 . The client noted that the machine is operational, but some files have a weird file extension. The client is worried that there was a ransomware attempt on Keegan's device. 
Your manager has tasked you to check the events in Splunk to determine what occurred in Keegan's device. 
Happy Hunting!

Virtual Machine

You can use the Attack Box or OpenVPN to access the Splunk instance.  The IP for the Splunk instance is MACHINE_IP . 
Note : Wait for the virtual machine to fully load. If you see errors after 2 minutes, refresh the URL until it loads. 

**Q1. A suspicious binary was downloaded to the endpoint. What was the name of the binary?**
I started by entering * in the search bar and setting the time range to "All Time" to view all available logs. To refine the results, I added additional fields such as Source IP, Source Port, Destination IP, and Destination Port.
Next, I filtered by Destination Port 443 to focus specifically on HTTPS connections. As shown in the image, three distinct values are returned, one of which corresponds to the downloaded binary.

<img width="1073" height="637" alt="image" src="https://github.com/user-attachments/assets/90c3507b-7c09-4226-b8bf-c38175aefe9d" />

Answer: _OUTSTANDING_GUTTER.exe_


**Q2. What is the address the binary was downloaded from? Add http:// to your answer & defang the URL.**

I searched for OUTSTANDING_GUTTER.exe in the query and included additional fields such as ParentCommandLine. 

<img width="972" height="677" alt="image" src="https://github.com/user-attachments/assets/af3592a8-13dc-41b6-862b-2414317435cd" />

Within this field, I noticed a Base64-encoded string. Using CyberChef, I decoded the value and extracted the following URL.

<img width="961" height="662" alt="image" src="https://github.com/user-attachments/assets/c0a6ce84-196c-414c-9645-02cf79a70a88" />

I defanged the URL using Cyberchef

Answer: _hxxp[://]886e-181-215-214-32[.]ngrok[.]io_


**Q3. What Windows executable was used to download the suspicious binary? Enter full path.**

<img width="861" height="453" alt="image" src="https://github.com/user-attachments/assets/edfb31bd-7529-42c8-ab11-434ee803cbf3" />

I could see the Windows executable used to download the suspicous binary in the same ParentCommandLine field.

Answer: _C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe_


**Q4. What command was executed to configure the suspicious binary to run with elevated privileges?**

Under the CommandLine field, I can see the value.

<img width="942" height="545" alt="image" src="https://github.com/user-attachments/assets/adda484a-fc9e-4b31-b7ea-841f546bd2a1" />

Answer: _"C:\Windows\system32\schtasks.exe" /Create /TN OUTSTANDING_GUTTER.exe /TR C:\Windows\Temp\COUTSTANDING_GUTTER.exe /SC ONEVENT /EC Application /MO *[System/EventID=777] /RU SYSTEM /f_


**Q5. What permissions will the suspicious binary run as? What was the command to run the binary with elevated privileges? (Format: User + ; + CommandLine)**

From the decoded PowerShell command, it’s clear that the scheduled task was configured to run under the SYSTEM account. The attacker then used schtasks.exe /Run to manually trigger this task, causing the binary to execute with SYSTEM-level privileges. I merged the details as specified in the format provided in the question.

<img width="855" height="597" alt="image" src="https://github.com/user-attachments/assets/a1bdfd38-7643-4d4d-9a41-98493a892520" />

Answer: _NT AUTHORITY\SYSTEM;"C:\Windows\system32\schtasks.exe" /Run /TN OUTSTANDING_GUTTER.exe_


**Q6. The suspicious binary connected to a remote server. What address did it connect to? Add http:// to your answer & defang the URL.**

I entered the query name into the search bar. The results contained the connection details, from which I identified the remote server address. Let's defang this URL using Cyberchef.

<img width="865" height="497" alt="image" src="https://github.com/user-attachments/assets/cb262cf1-1069-4cd0-922c-127cddf7fe0d" />

Answer: _hxxp[://]9030-181-215-214-32[.]ngrok[.]io_


**Q7. A PowerShell script was downloaded to the same location as the suspicious binary. What was the name of the file?**

To investigate this, I searched for '.ps1' to filter PowerShell-related logs. In the first result under TargetFilename, I found the script that had been downloaded to the same location as the previously identified suspicious binary.

<img width="890" height="437" alt="image" src="https://github.com/user-attachments/assets/754011d3-eb8b-4250-8f00-827e2479be07" />

Answer: _script.ps1_


**Q8. The malicious script was flagged as malicious. What do you think was the actual name of the malicious script?**

I looked up the SHA hash E0AFCF804394ABD43AD4723A0FEB147F10E589CD on VirusTotal. Reviewing the analysis details revealed the actual name of the script.

<img width="1565" height="833" alt="image" src="https://github.com/user-attachments/assets/bd31c46e-d210-4455-b998-f2ea03e81836" />

Answer: _BlackSun.ps1_


**Q9. A ransomware note was saved to disk, which can serve as an IOC. What is the full path to which the ransom note was saved?**

Ransomware notes are typically stored in .txt files, so I searched for "blacksun". Scrolling through the results, I located the README text file.

<img width="1161" height="632" alt="image" src="https://github.com/user-attachments/assets/7beb5bb2-ebd5-481a-b6b6-b2db4c7c2a53" />

Answer: _C:\Users\keegan\Downloads\vasg6b0wmw029hd\BlackSun_README.txt_


**Q10. The script saved an image file to disk to replace the user's desktop wallpaper, which can also serve as an IOC. What is the full path of the image**?

By searching for .jpg or .png image files, the first log entry reveals the image file that was created by the ransomware.

<img width="1060" height="591" alt="image" src="https://github.com/user-attachments/assets/e1694f39-37fd-49c1-ab72-e3d69cb8afd8" />

Answer: _C:\Users\Public\Pictures\blacksun.jpg_
