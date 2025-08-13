**Ransomware Investigation**

![tuxpi-com-1629599275](https://github.com/user-attachments/assets/f7784dad-cfa0-4d56-8d05-ae6f491a5eec)


The firewall alerted the Security Operations Center that one of the machines at the Sales department, which stores all the customers' data, contacted the malicious domains over the network. When the Security Analysts looked closely, the data sent to the domains contained suspicious base64-encoded strings. The Analysts involved the Incident Response team in pulling the Process Monitor and network traffic data to determine if the host is infected. But once they got on the machine, they knew it was a ransomware attack by looking at the wallpaper and reading the ransomware note.
Can you find more evidence of compromise on the host and what ransomware was involved in the attack?  


Q1. Provide the two PIDs spawned from the malicious executable. (In the order as they appear in the analysis tool)

Launch ProcDOT and, under Monitoring Logs, navigate to Procmon and open Logfile.csv. Use the Launcher option to render the configuration. During analysis, I noticed two suspicious executables sharing the same name - exploreer.exe

<img width="1258" height="712" alt="image" src="https://github.com/user-attachments/assets/6dd12efb-bf4d-4737-9d42-8d3da0568d4f" />


Answer: _8644,7128_


Q2. Provide the full path where the ransomware initially got executed? (Include the full path in your answer)

Double-click on exploree.exe to open it in the Launcher. The executable’s path will already be pre-filled. In Windump, load the traffic.pcap file and then refresh the view. This will generate a visual chart of the activity.

<img width="438" height="95" alt="image" src="https://github.com/user-attachments/assets/d9743db6-42f6-41e2-a248-f3828420119f" />


By zooming in, the execution path of the executable file becomes clearly visible.

<img width="438" height="95" alt="image" src="https://github.com/user-attachments/assets/cf2c8800-e973-4d59-9e41-0bf17816c72b" />


Answer: _c:\users\sales\appdata\local\temp\exploreer.exe_


Q3. This ransomware transfers the information about the compromised system and the encryption results to two domains over HTTP POST. What are the two C2 domains? (no space in the answer)

The analysis reveals two domains associated with the exploree.exe executable.

<img width="777" height="305" alt="image" src="https://github.com/user-attachments/assets/715bb2e2-025d-437c-a84a-b02937ef0662" />


Answer: _mojobiden.com,paymenthacks.com_


Q3. What are the IPs of the malicious domains? (no space in the answer)

The investigation also uncovered the IP addresses linked to the exploree.exe executable.

<img width="777" height="305" alt="image" src="https://github.com/user-attachments/assets/47c1f2d7-6d94-42ad-bbb6-8a5d537612ed" />


Answer: _146.112.61.108,206.188.197.206_


Q4. Provide the user-agent used to transfer the encrypted data to the C2 channel. 

I entered the IP address of the malicious domain into Wireshark and followed the corresponding TCP stream for further analysis.

<img width="372" height="167" alt="image" src="https://github.com/user-attachments/assets/60c866f8-ae8a-41fb-ac33-3d37762365a7" />


Answer: _Firefox/89.0_


Q5. Provide the cloud security service that blocked the malicious domain. 

The same TCP stream also contains a reference to the server.

<img width="697" height="352" alt="image" src="https://github.com/user-attachments/assets/43186e97-b102-4740-a530-102305dbce8f" />


Answer: _Cisco Umbrella_


Q6. Provide the name of the bitmap that the ransomware set up as a desktop wallpaper. 

Returning to ProcDOT to review the chart for further details, I identified the name of the bitmap file.

<img width="823" height="225" alt="image" src="https://github.com/user-attachments/assets/2a6ee5a4-db5f-45f0-845c-51b0feab3d1c" />


Answer: _ley9kpi9r.bmp_


Q7. Find the PID (Process ID) of the process which attempted to change the background wallpaper on the victim's machine.

The background wallpaper was altered, as indicated by the following PID visible in the screenshot.

<img width="1617" height="313" alt="image" src="https://github.com/user-attachments/assets/322081c0-4965-4440-b545-df1f5103a0b7" />


Answer: _4892_


Q8. The ransomware mounted a drive and assigned it the letter. Provide the registry key path to the mounted drive, including the drive letter.

The registry key path to the mounted drive is visible here.

<img width="280" height="47" alt="image" src="https://github.com/user-attachments/assets/546ad8a7-8687-4632-a79b-e2f2c28b670e" />


Answer: _HKLM\SYSTEM\MountedDevices\DosDevices\Z:_


Q9. Now you have collected some IOCs from this investigation. Provide the name of the ransomware used in the attack. (external research required)

Scanning 206.188.197.206 on VirusTotal revealed the name of the ransomware involved in the attack.

<img width="1072" height="533" alt="image" src="https://github.com/user-attachments/assets/76ec7217-eab4-43f6-ba3b-0eb9670c2ca8" />


Answer: _Blackmatter Ransomware_



