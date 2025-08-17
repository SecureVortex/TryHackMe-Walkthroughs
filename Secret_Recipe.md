**Introduction**

**Storyline**

Jasmine owns a famous New York coffee shop Coffely which is famous city-wide for its unique taste. Only Jasmine keeps the original copy of the recipe, and she only keeps it on her work laptop. Last week, James from the IT department was consulted to fix Jasmine's laptop. But it is suspected he may have copied the secret recipes from Jasmine's machine and is keeping them on his machine. Image showing a Laptop with a magnifying glass

His machine has been confiscated and examined, but no traces could be found. The security department has pulled some important registry artifacts from his device and has tasked you to examine these artifacts and determine the presence of secret files on his machine.

Room Machine

Before moving forward, let's deploy the machine by clicking on the Start Machine button on the top of the task. The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top of the page. You may also access it via the AttackBox or RDP using the credentials below. It will take up to 3-5 minutes to start.

On the Desktop, there is a folder named Artifacts, which contains the registry Hives to examine and another folder named EZ tools, which includes all the required tools to analyze the artifacts.

Credentials

Username : Administrator

Password: thm_4n6

Note: If you are using Registry Explorer to parse the hives, expect some delay in loading as it takes time to parse the hives.

Q1. Connect with the lab.

Answer: _No answer needed_ 


Q2. How many files are available in the Artifacts folder on the Desktop?

<img width="811" height="327" alt="image" src="https://github.com/user-attachments/assets/f776e4c2-a337-46ce-8739-c7a8ac438942" />


Answer: _6_


**Windows Registry Forensics**


Registry Recap

Windows Registry is like a database that contains a lot of juicy information about the system, user, user activities, processes executed, the files accessed or deleted, etc. Image showing Registry icon

Following Registry Hives have been pulled from the suspect Host and placed in the C:\Users\Administrator\Desktop\Artifacts folder. All required tools are also placed on the path. C:\Users\Administrator\Desktop\EZ Tools.

Your challenge is to examine the registry hives using the tools provided, observe the user's activities and answer the questions.

Registry Hives

    SYSTEM
    SECURITY
    SOFTWARE
    SAM
    NTUSER.DAT
    UsrClass.dat

Note: The Download Task Files button has a cheat sheet, which can be used as a reference to answer the questions.

Q1. What is the computer name of the machine found in the registry?

I launched Registry Explorer, loaded the hive, and opened the SYSTEM file from the Artifacts folder on the Desktop. Once it was loaded, I used Tools → Find to search for the term "computername".

<img width="1060" height="508" alt="image" src="https://github.com/user-attachments/assets/37436bb0-aa0d-44a2-afdf-0c6d833fc728" />


Answer: _JAMES_


Q2. When was the Administrator account created on this machine? (Format: yyyy-mm-dd hh:mm:ss)

For this, I loaded the SAM hive and found the answer in the "Created On" column.

<img width="1157" height="332" alt="image" src="https://github.com/user-attachments/assets/e6645ac2-89c8-4f26-95f6-b2ce8d27b947" />


Answer: _2021-03-17 14:58:48_


Q3. What is the RID associated with the Administrator account?

The RID is visible in the screenshot referenced in the previous answer.

Answer: _500_


Q4. How many user accounts were observed on this machine?

There are seven user accounts present on this machine.

<img width="391" height="435" alt="image" src="https://github.com/user-attachments/assets/c7b51eb9-0153-4890-b2fd-3ec92c888712" />

Answer: _7_


Q5. There seems to be a suspicious account created as a backdoor with RID 1013. What is the account name?

We can see the User ID 1013 and identify the account associated with it.

<img width="608" height="190" alt="image" src="https://github.com/user-attachments/assets/e6d7353e-7dc8-4606-b871-47b807083a9c" />


Answer: _bdoor_


Q6. What is the VPN connection this host connected to?

The hint suggests looking for NetworkList in the SOFTWARE hive.
I loaded the SOFTWARE hive in Registry Explorer, then used the Find option to search for "NetworkList". Two results appeared, and I proceeded with the first one.

<img width="1001" height="315" alt="image" src="https://github.com/user-attachments/assets/05f4cc64-ac7e-4786-bd0f-1e02ebe0dfce" />


Answer: _ProtonVPN_


Q7. When was the first VPN connection observed? (Format: YYYY-MM-DD HH:MM:SS)

This value is listed under First Connect LOCAL.

<img width="1165" height="296" alt="image" src="https://github.com/user-attachments/assets/6ef30307-9b65-4820-ba96-41d6f1ed3847" />


Answer: _2022-10-12 19:52:36_


Q8. There were three shared folders observed on his machine. What is the path of the third share?

I searched on Google to find where shared folders are stored and came across the path SYSTEM\ControlSet001\Services\LanmanServer\Shares.
There, I found three shares, one of which was the answer.

<img width="1168" height="413" alt="image" src="https://github.com/user-attachments/assets/5127161f-a00d-4e02-a524-1bc8dd272b5d" />


Answer: _C:\RESTRICTED FILES_


Q9. What is the last DHCP IP assigned to this host?

The answer can be found in SYSTEM\ControlSet002\Services\Tcpip\Interfaces.

<img width="1562" height="450" alt="image" src="https://github.com/user-attachments/assets/47f01dff-bc18-4b95-b039-8505348b8c2f" />


Answer: _172.31.2.197_


Q10. The suspect seems to have accessed a file containing the secret coffee recipe. What is the name of the file?

For this, I uploaded the NTUSER.DAT file and created the hive. I then searched for "RecentDocs", and there was only one matching entry for NTUSER.DAT—and that’s where the answer was.

<img width="1121" height="272" alt="image" src="https://github.com/user-attachments/assets/ecb0498a-b54b-49c6-93ea-c9ec6d2f1707" />


Answer: _secret-recipe.pdf_


Q11. The suspect executed multiple commands using the Run window. What command was used to enumerate the network interfaces?

We can view the commands run by the user in the NTUSER hive under NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU.
The RunMRU key stores a list of recently executed commands entered via the Run dialog box.

<img width="1420" height="567" alt="image" src="https://github.com/user-attachments/assets/a4706e33-ab0e-4fb7-9043-db29984eafe6" />


Answer: _pnputil /enum-interfaces_


Q12. The user searched for a network utility tool to transfer files using the file explorer. What is the name of that tool?

Using WordWheelQuery, we can see a list of search terms entered by the user in the Windows File Explorer search box, where we found the answer.

<img width="918" height="416" alt="image" src="https://github.com/user-attachments/assets/1903959a-ff15-4a6d-af7c-ed0e849edd7b" />

Answer: _netcat_


Q13. What is the recent text file opened by the suspect?

I navigated to RecentDocs to review the recent files and sorted the Target Name by the "Opened On" parameter. The answer was the file accessed right after the PDF file was opened.

<img width="1603" height="272" alt="image" src="https://github.com/user-attachments/assets/f334220f-f582-4c1a-beb8-b8b36985c968" />


Answer: _secret-code.txt_


Q14. How many times was PowerShell executed on this host?

The answer is located in NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count.

<img width="1082" height="272" alt="image" src="https://github.com/user-attachments/assets/40ef7be5-910e-416b-a360-8d4365d6ea91" />


Answer: _3_


Q15. The suspect also executed a network monitoring tool. What is the name of the tool?

I scrolled down the list and located the network monitoring tool.

<img width="906" height="302" alt="image" src="https://github.com/user-attachments/assets/77017781-6c0a-4599-9e5c-0d5f3e041944" />


Answer: _wireshark_


Q16. Registry Hives also note the amount of time a process is in focus. Examine the Hives and confirm for how many seconds was ProtonVPN executed?

For this, I scrolled up until I found the ProtonVPN program name.

<img width="1537" height="405" alt="image" src="https://github.com/user-attachments/assets/6a189619-9fe1-471b-a9bc-fcb8257e7f8f" />


The focus time is displayed in hours, minutes, and seconds. After converting it into seconds, we get the answer.

Answer: _343_


Q17. Everything.exe is a utility used to search for files in a Windows machine. What is the full path from which everything.exe was executed?

This one was straightforward. I ran a search and quickly found the result.

<img width="820" height="241" alt="image" src="https://github.com/user-attachments/assets/731a3bdc-0ded-4d9f-a292-bf2aec8cd303" />


Answer: _C:\Users\Administrator\Downloads\tools\Everything\Everything.exe_


**Summary**

In this walkthrough, I performed Windows registry forensics on a compromised system to uncover traces of attacker activity. By loading different hives (SYSTEM, SOFTWARE, SAM, and NTUSER.DAT) into Registry Explorer, I identified system configuration details, user accounts, suspicious backdoor creation, network connections, shared folders, and evidence of executed programs. The analysis revealed both legitimate system usage and malicious behavior, including the creation of a backdoor account (bdoor) and the use of ProtonVPN and other tools.
