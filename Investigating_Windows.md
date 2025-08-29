This is a challenge that is exactly what is says on the tin, there are a few challenges around investigating a windows machine that has been previously compromised.

Connect to the machine using RDP. The credentials the machine are as follows:

Username: Administrator
Password: letmein123!

Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.


**Q1. Whats the version and year of the windows machine?**

I started by retrieving system information to understand the environment. Using 'Get-ComputerInfo', I could see details like WindowsProductName, OsName, and OsVersion.

<img width="767" height="182" alt="image" src="https://github.com/user-attachments/assets/2a5f66a4-b3f1-46f2-931c-93dd009f4a6e" />

Answer: _Windows Server 2016_


**Q2. Which user logged in last?**

To determine the last logged-in user, I listed local user accounts. The 'Get-LocalUser' attribute helped me find the most recent login

<img width="563" height="132" alt="image" src="https://github.com/user-attachments/assets/0aeb2eba-435f-4858-8ef3-61c9b2d022d3" />

Answer: _Administrator_


**Q3. When did John log onto the system last?**
Answer format: MM/DD/YYYY H:MM:SS AM/PM

Running 'net user John' gave me John’s account details, including the Last logon field.

<img width="375" height="326" alt="image" src="https://github.com/user-attachments/assets/2fb21598-ae94-476e-9e6a-95a8e5491ffb" />

Answer: _03/02/2019 5:48:32 PM_


**Q4. What IP does the system connect to when it first starts?**

<img width="332" height="140" alt="image" src="https://github.com/user-attachments/assets/aed64c85-4b7d-4013-a819-f499ae191c32" />

When the target machine was started, an executable was executed automatically which contained the IP.

Answer: _10.34.2.3_


**Q5. What two accounts had administrative privileges (other than the Administrator user)?**
Answer format: List them in alphabetical order.

By checking the Administrators group, I found two additional accounts with elevated privileges. I ran the following:
GetLocalGroupMember -Group 'Administrators'

<img width="531" height="107" alt="image" src="https://github.com/user-attachments/assets/29a26fa6-32cf-470e-9a52-e397bcb007d4" />

Answer: _Guest, Jenny_


**Q6. Whats the name of the scheduled task that is malicous.**

I listed all scheduled tasks using 'Get-ScheduledTask' to spot anomalies. One task stood out as unusual: Clean file system — clearly malicious given its suspicious naming.

<img width="631" height="150" alt="image" src="https://github.com/user-attachments/assets/a8b33797-4639-402c-9e07-a3f2546b363d" />

Answer: _Clean file system_


**Q7. What file was the task trying to run daily?**

Looking at the task’s defined action showed it executed a PowerShell script called nc.ps1, which is often used as a netcat backdoor. I used the following command:

(Get-ScheduledTask -TaskName "Clean file system").Actions

<img width="595" height="103" alt="image" src="https://github.com/user-attachments/assets/fb2bdc32-a348-431e-9840-9a2d29e91fcf" />


Answer: _nc.ps1_


**Q8. What port did this file listen locally for?**

The previous screenshot contained the port using which the file was able to listen locally.

<img width="617" height="115" alt="image" src="https://github.com/user-attachments/assets/d333d079-7f79-4365-81a9-875fa1f354ac" />


Answer: _1348_


**Q9. When did Jenny last logon?**

Running 'net user Jenny' showed Jenny’s user details. The Last logon field indicated she had Never logged in.

<img width="430" height="323" alt="image" src="https://github.com/user-attachments/assets/a18b7e55-6328-46f6-9ff7-3d116f7427fe" />


Answer: _Never_


**Q10. At what date did the compromise take place?**
Answer format: MM/DD/YYYY

By checking the creation and modification dates of suspicious files under C:\TMP, I found the earliest compromise activity.

Get-ChildItem C:\TMP

<img width="520" height="287" alt="image" src="https://github.com/user-attachments/assets/d16c9fb4-0ee1-4085-91e7-381e03232949" />


Answer: _03/02/2019_


**Q11. During the compromise, at what time did Windows first assign special privileges to a new logon?**
Answer format: MM/DD/YYYY HH:MM:SS AM/PM

I reviewed the Security logs for Event ID 4672 in Event Viewer.

Answer: _03/02/2019 4:04:49 PM_


**Q12. What tool was used to get Windows passwords?**

The output of 'mim-out.txt' clearly referenced Mimikatz, a well-known credential dumping tool. I used 'Get-Content C:\TMP\mim-out.txt' to get the information.

<img width="577" height="145" alt="image" src="https://github.com/user-attachments/assets/3bb4b3ab-c725-4238-b602-bc8d042321f8" />

Answer: _Mimikatz_


**Q13. What was the attackers external control and command servers IP?**

The attacker had poisoned the hosts file to point traffic to their C2 server.

Get-Content C:\Windows\System32\drivers\etc\hosts

<img width="658" height="455" alt="image" src="https://github.com/user-attachments/assets/1847e182-65e4-4ec3-b98f-4d7cac51b8cb" />

Answer: _76.32.97.132_


**Q14. What was the extension name of the shell uploaded via the servers website?**

Get-ChildItem C:\inetpub\wwwroot

Inspecting the webroot revealed an unusual file extension: .jsp, indicating a Java-based web shell.

<img width="437" height="141" alt="image" src="https://github.com/user-attachments/assets/2406e2a1-2099-43ac-81d2-28f0c5a1d763" />

Answer: _.jsp_


**Q15. What was the last port the attacker opened?**

I checked the inbound rules of Windows Firewall via Control Panel > Advanced Settings. The most recent rule created by the attacker allowed traffic through port 1337.

<img width="875" height="240" alt="image" src="https://github.com/user-attachments/assets/4fb60b3f-b776-4da9-8cb9-623c66f32220" />


Answer: _1337_


**Q16. Check for DNS poisoning, what site was targeted?**

Reviewing the hosts file again showed evidence of DNS poisoning. The attacker had redirected requests for a legitimate website to their malicious IP. The targeted site was identified in the poisoned entry.

<img width="658" height="455" alt="image" src="https://github.com/user-attachments/assets/1847e182-65e4-4ec3-b98f-4d7cac51b8cb" />


Answer: _google.com_
