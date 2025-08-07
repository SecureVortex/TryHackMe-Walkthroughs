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
Answer: _hxxp[://]886e-181-215-214-32[.]ngrok[.]io_

**Q3. What Windows executable was used to download the suspicious binary? Enter full path.**
Answer: _C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe_

**Q4. What command was executed to configure the suspicious binary to run with elevated privileges?**
Answer: _"C:\Windows\system32\schtasks.exe" /Create /TN OUTSTANDING_GUTTER.exe /TR C:\Windows\Temp\COUTSTANDING_GUTTER.exe /SC ONEVENT /EC Application /MO *[System/EventID=777] /RU SYSTEM /f_

**Q5. What permissions will the suspicious binary run as? What was the command to run the binary with elevated privileges? (Format: User + ; + CommandLine)**
Answer: _NT AUTHORITY\SYSTEM;"C:\Windows\system32\schtasks.exe" /Run /TN OUTSTANDING_GUTTER.exe_

**Q6. The suspicious binary connected to a remote server. What address did it connect to? Add http:// to your answer & defang the URL.**
Answer: _hxxp[://]9030-181-215-214-32[.]ngrok[.]io_

**Q7. A PowerShell script was downloaded to the same location as the suspicious binary. What was the name of the file?**
Answer: _script.ps1_

**Q8. The malicious script was flagged as malicious. What do you think was the actual name of the malicious script?**
Answer: _BlackSun.ps1_

**Q9. A ransomware note was saved to disk, which can serve as an IOC. What is the full path to which the ransom note was saved?**
Answer: _C:\Users\keegan\Downloads\vasg6b0wmw029hd\BlackSun_README.txt_

**Q10. The script saved an image file to disk to replace the user's desktop wallpaper, which can also serve as an IOC. What is the full path of the image**?
Answer: _C:\Users\Public\Pictures\blacksun.jpg_
