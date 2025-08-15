**Navigate Through the Endpoint Logs**

Swiftspend Finance, the coolest fintech company in town, is on a mission to level up its cyber security game to keep those digital adversaries at bay and ensure their customers stay safe and sound.

Led by the tech-savvy Senior Security Engineer John Sterling, Swiftspend's latest project is about beefing up their endpoint monitoring using Wazuh and Sysmon. They've been running some tests to see how well their cyber guardians can sniff out trouble. And guess what? You're the cyber sleuth they've called in to crack the code!

The tests were run on Apr 29, 2024, between 12:00:00 and 20:00:00. As you dive into the logs, you'll look for any suspicious process shenanigans or weird network connections, you name it! Your mission? Unravel the mysteries within the logs and dish out some epic insights to fine-tune Swiftspend's defences.
Machine Access

Click the Start Machine button attached to this task to start the VM. Give the machine about 5 minutes to fully set up the environment. Access the Wazuh Dashboard using your browser at https://LAB_WEB_URL.p.thmlabs.com and use the credentials listed below:

<img width="545" height="147" alt="image" src="https://github.com/user-attachments/assets/f9de07d9-d7ab-4015-be0a-d25d1bb285ae" />

Once logged in, navigate to the Security events module and use the saved query Monday_Monitor to access the logs.


Q1. Initial access was established using a downloaded file. What is the file name saved on the host?

I adjusted the event search parameters to only include logs from April 29, 2024, between 12:00 and 20:00 to narrow down the timeframe of interest. Then, I searched for the keyword "localhost" and examined the eventdata.commandLine field, where the file name appeared.

<img width="1767" height="515" alt="image" src="https://github.com/user-attachments/assets/13ac640e-b5da-4f83-9fc0-b7be960ec763" />


Answer: _SwiftSpend_Financial_Expenses.xlsm_


Q2. What is the full command run to create a scheduled task?

In the search bar, I entered "scheduler" to identify events related to task scheduling. To retrieve the complete command that created the scheduled task, I applied the filter data.win.eventdata.parentCommandLine, which displayed the full execution string.

<img width="1901" height="687" alt="image" src="https://github.com/user-attachments/assets/299dc7ce-9912-440a-a26c-7596da6e6c15" />


Answer: _\"cmd.exe\" /c \"reg add HKCU\\SOFTWARE\\ATOMIC-T1053.005 /v test /t REG_SZ /d cGluZyB3d3cueW91YXJldnVsbmVyYWJsZS50aG0= /f & schtasks.exe /Create /F /TN \"ATOMIC-T1053.005\" /TR \"cmd /c start /min \\\"\\\" powershell.exe -Command IEX([System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String((Get-ItemProperty -Path HKCU:\\\\SOFTWARE\\\\ATOMIC-T1053.005).test)))\" /sc daily /st 12:34\"_


Q3. What time is the scheduled task meant to run?

From the logs referenced in the previous step, the scheduled task entry clearly displayed the time it was set to execute.

Answer: _12:34_


Q4. What was encoded?

I decoded the string cGluZyB3d3cueW91YXJldnVsbmVyYWJsZS50aG0= using CyberChef, which revealed the original content.

Answer: _ping www.youarevulnerable.thm_


Q5. What password was set for the new user account?

I searched for the keyword '/add' in the event logs to identify any commands that may have been used to add new accounts. Under the Users section, I selected "Administrator" to focus on activity performed by that account. Examining the related command lines, I found that 'C:\Windows\system32\net.exe localgroup Administrators guest /add' was executed, indicating that a guest account was added to the Administrators group. Further inspection of the logs also revealed the password set for this newly created account.

<img width="1156" height="483" alt="image" src="https://github.com/user-attachments/assets/34e4b88f-717e-419f-9c50-e108497fc6c0" />


Answer: _I_AM_M0NIT0R1NG_


Q6. What is the name of the .exe that was used to dump credentials?

Since the question focused on credential dumping, I searched for "mimikatz", a well-known tool used for extracting passwords and authentication data from memory. Mimikatz is one of the most common post-exploitation tools used by attackers to dump credentials from systems. This search quickly led me to the relevant event, revealing the answer.

<img width="1252" height="655" alt="image" src="https://github.com/user-attachments/assets/3e92f5b9-f67b-45a8-89c0-3099f8bb9b7c" />


Answer: _memotech.exe_


Q7. Data was exfiltrated from the host. What was the flag that was part of the data?

I filtered the event logs to show only those associated with the Administrator account. Next, I searched for events involving powershell.exe to identify any suspicious script executions. Initially, this didn’t reveal much, but by reviewing the message field in the logs, I was able to spot the relevant activity.

<img width="996" height="708" alt="image" src="https://github.com/user-attachments/assets/620599aa-e6bb-4ae3-a289-b1040d4dad74" />

When I opened the message field from the identified event, I finally found the flag.

<img width="1540" height="565" alt="image" src="https://github.com/user-attachments/assets/d85c6691-eb11-4dd6-8173-19ae5905abd1" />


Answer: _THM{M0N1T0R_1$_1N_3FF3CT}_
