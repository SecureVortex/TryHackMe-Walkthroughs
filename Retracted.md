**Retracted**

**TASK1: Introduction**

A Mother's Plea
"Thanks for coming. I know you are busy with your new job, but I did not know who else to turn to."
"So I downloaded and ran an installer for an antivirus program I needed. After a while, I noticed I could no longer open any of my files. And then I saw that my wallpaper was different and contained a terrifying message telling me to pay if I wanted to get my files back. I panicked and got out of the room to call you. But when I came back, everything was back to normal."
"Except for one message telling me to check my Bitcoin wallet. But I don't even know what a Bitcoin is!"
"Can you help me check if my computer is now fine?"
Connecting to the Machine
Start the virtual machine in split-screen view by clicking on the green "Start Machine" button on the upper right section of this task. If the VM is not visible, use the blue "Show Split View" button at the top-right of the page. Alternatively, you can connect to the VM using the credentials below via "Remote Desktop".

<img width="551" height="203" alt="image" src="https://github.com/user-attachments/assets/4cb2b9cf-07c3-43ab-b8dc-64242ec13445" />

"Oh, the password doesn't work? Wait, I have it written somewhere. Uhmm... Try this:"

<img width="553" height="204" alt="image" src="https://github.com/user-attachments/assets/2c3c9b53-32b1-4b87-8439-045515a4fd39" />

**TASK 2: The Message**

"So, as soon as you finish logging in to the computer, you'll see a file on the desktop addressed to me."
"I have no idea why that message is there and what it means. Maybe you do?"

**Q1. What is the full path of the text file containing the "message"?**
Locate SOPHIE.txt on the desktop and review its properties.
<img width="464" height="651" alt="image" src="https://github.com/user-attachments/assets/3f1399b7-bfe4-472d-a5ae-1ab37ce1eac9" />

Answer: _C:\Users\Sophie\Desktop\SOPHIE.txt_

**Q2. What program was used to create the text file?**
Open Event Viewer.
Go to Application and Service Logs, then select Microsoft > Windows > Sysmon > Operational.
To access the ‘Process Create’ category, apply a filter using Event ID 1.
Many events may appear. Search for SOPHIE.txt within the results.
<img width="940" height="576" alt="image" src="https://github.com/user-attachments/assets/9875aeeb-371a-469f-88f7-90a87707a23f" />

<img width="940" height="641" alt="image" src="https://github.com/user-attachments/assets/1992fdf9-f812-4ae7-8b14-b54164cd77f2" />

Below the image, the program used to create the file is displayed.
Answer: _notepad.exe_

**Q3. What is the time of execution of the process that created the text file? Timezone UTC (Format YYYY-MM-DD hh:mm:ss)**
The execution time is listed under UtcTime in the Event Properties.
<img width="940" height="643" alt="image" src="https://github.com/user-attachments/assets/5dcbf7d7-4b5f-482f-97da-37a6caf86ff5" />

Answer: _2024-01-08 14:25:30_

**TASK 3: Something Wrong**
"I swear something went wrong with my computer when I ran the installer. Suddenly, my files could not be opened, and the wallpaper changed, telling me to pay."
"Wait, are you telling me that the file I downloaded is a virus? But I downloaded it from Google!"

**Q1. What is the filename of this "installer"? (Including the file extension)**
Let’s review the downloads section within the INTERNET browser.
<img width="800" height="308" alt="image" src="https://github.com/user-attachments/assets/1326f304-59fc-44d1-b8a7-292134940357" />

Answer: _antivirus.exe_

**Q2. What is the download location of this installer?**
<img width="776" height="176" alt="image" src="https://github.com/user-attachments/assets/74e37065-e3b3-4f3f-ad3b-f323ebc93de3" />

Answer: _C:\Users\Sophie\download_

**Q3. The installer encrypts files and then adds a file extension to the end of the file name. What is this file extension?**
Return to Event Viewer and search for Event ID 11 related to file creation. Locate the antivirus.exe file.
<img width="940" height="638" alt="image" src="https://github.com/user-attachments/assets/43ab38c9-8f49-455b-a1a8-ec5ef1c25ef0" />

Answer: _.dmp_

**Q4. The installer reached out to an IP. What is this IP?**
Filter Event ID 3 logs for network connections and locate antivirus.exe.
<img width="940" height="680" alt="image" src="https://github.com/user-attachments/assets/b3a864c9-d401-493d-a974-9afb9ef36efe" />

Answer: _10.10.8.111_

**TASK 4: Back to Normal**
"So what happened to the virus? It does seem to be gone since all my files are back."

**Q1: The threat actor logged in via RDP right after the “installer” was downloaded. What is the source IP?**
We can use the event ID 3 filter and check for any RDP connections.
<img width="794" height="563" alt="image" src="https://github.com/user-attachments/assets/172bfe94-a359-44a6-bd8d-aeb9f3dc168d" />

Answer: _10.11.27.46_

**Q2: This other person downloaded a file and ran it. When was this file run? Timezone UTC (Format YYYY-MM-DD hh:mm:ss)**
To check for process creation, use Event ID 1 in the search. Let’s search for the decryptor.exe file that was previously identified in the Downloads folder.
<img width="940" height="676" alt="image" src="https://github.com/user-attachments/assets/0212d178-b3ca-47c4-8e39-83117c815178" />

Answer: _2024-01-08 14:24:18_

**TASK 5. Doesn’t Make Sense**

"So you're telling me that someone accessed my computer and changed my files but later undid the changes?"
"That doesn't make any sense. Why infect my machine and clean it afterwards?"
"Can you help me make sense of this?"
Arrange the following events in sequential order from 1 to 7, based on the timeline in which they occurred.

**Q1. After seeing the ransomware note, Sophie ran out and reached out to you for help.**
3

**Q2. Sophie downloaded the malware and ran it.**
1

**Q3. After all the files are restored, the intruder left the desktop telling Sophie to check her Bitcoin.**
6

**Q4. The intruder realized he infected a charity organization. He then downloaded a decryptor and decrypted all the files.**
5

**Q6. The downloaded malware encrypted the files on the computer and showed a ransomware note.**
2

**Q7. While Sophie was away, an intruder logged into Sophie's machine via RDP and started looking around.**
4

**Q8. Sophie and I arrive on the scene to investigate. At this point, the intruder was gone.**
7

**TASK6: Conclusion**
"Adelle from Finance just called me. She says that someone just donated a huge amount of bitcoin to our charity's account!"
"Could this be our intruder? His malware accidentally infected our systems, found the mistake, and retracted all the changes?"
"Maybe he had a change of heart?"


