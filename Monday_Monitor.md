**Navigate Through the Endpoint Logs**

Swiftspend Finance, the coolest fintech company in town, is on a mission to level up its cyber security game to keep those digital adversaries at bay and ensure their customers stay safe and sound.

Led by the tech-savvy Senior Security Engineer John Sterling, Swiftspend's latest project is about beefing up their endpoint monitoring using Wazuh and Sysmon. They've been running some tests to see how well their cyber guardians can sniff out trouble. And guess what? You're the cyber sleuth they've called in to crack the code!

The tests were run on Apr 29, 2024, between 12:00:00 and 20:00:00. As you dive into the logs, you'll look for any suspicious process shenanigans or weird network connections, you name it! Your mission? Unravel the mysteries within the logs and dish out some epic insights to fine-tune Swiftspend's defences.
Machine Access

Click the Start Machine button attached to this task to start the VM. Give the machine about 5 minutes to fully set up the environment. Access the Wazuh Dashboard using your browser at https://LAB_WEB_URL.p.thmlabs.com and use the credentials listed below:

<img width="545" height="147" alt="image" src="https://github.com/user-attachments/assets/f9de07d9-d7ab-4015-be0a-d25d1bb285ae" />

Once logged in, navigate to the Security events module and use the saved query Monday_Monitor to access the logs.


Q1. Initial access was established using a downloaded file. What is the file name saved on the host?

Answer: _SwiftSpend_Financial_Expenses.xlsm_


Q2. What is the full command run to create a scheduled task?

Answer: _\"cmd.exe\" /c \"reg add HKCU\\SOFTWARE\\ATOMIC-T1053.005 /v test /t REG_SZ /d cGluZyB3d3cueW91YXJldnVsbmVyYWJsZS50aG0= /f & schtasks.exe /Create /F /TN \"ATOMIC-T1053.005\" /TR \"cmd /c start /min \\\"\\\" powershell.exe -Command IEX([System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String((Get-ItemProperty -Path HKCU:\\\\SOFTWARE\\\\ATOMIC-T1053.005).test)))\" /sc daily /st 12:34\"_


Q3. What time is the scheduled task meant to run?

Answer: _12:34_


Q4. What was encoded?

Answer: _ping www.youarevulnerable.thm_


Q5. What password was set for the new user account?

Answer: _I_AM_M0NIT0R1NG_


Q6. What is the name of the .exe that was used to dump credentials?

Answer: _memotech.exe_


Q7. Data was exfiltrated from the host. What was the flag that was part of the data?

Answer: _THM{M0N1T0R_1$_1N_3FF3CT}_
