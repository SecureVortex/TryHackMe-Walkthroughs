**Introduction**

<img width="256" height="256" alt="be84f26c22e5a051fc003dba5ed3dcd4" src="https://github.com/user-attachments/assets/99235ce3-f872-4a67-bc9c-b82c2fc3d10b" />

In this challenge room, we will take a simple challenge to investigate an alert by IDS regarding a potential C2 communication.
Room Machine

Before moving forward, deploy the machine. When you deploy the machine, it will be assigned an IP Machine IP: MACHINE_IP. The machine will take up to 3-5 minutes to start. Use the following credentials to log in and access the logs in the Discover tab.

Username: Admin

Password: elastic123

**Scenario - Investigate a potential C2 communication alert**

Scenario

During normal SOC monitoring, Analyst John observed an alert on an IDS solution indicating a potential C2 communication from a user Browne from the HR department. A suspicious file was accessed containing a malicious pattern THM:{ ________ }. A week-long HTTP connection logs have been pulled to investigate. Due to limited resources, only the connection logs could be pulled out and are ingested into the connection_logs index in Kibana.
Our task in this room will be to examine the network connection logs of this user, find the link and the content of the file, and answer the questions.

**Q1. How many events were returned for the month of March 2022?**
Answer: _1482_

**Q2. What is the IP associated with the suspected user in the logs?**
Answer: _192.166.65.54_

**Q3. The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?**
Answer: _bitsadmin_

**Q4. The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?**
Answer: _pastebin.com_

**Q5. What is the full URL of the C2 to which the infected host is connected?**
Answer: _pastebin.com/yTg0Ah6a_

**Q6. A file was accessed on the filesharing site. What is the name of the file accessed?**
Answer: _secret.txt_

**Q7. The file contains a secret code with the format THM{_____}.**
Answer: _THM{SECRET__CODE}_



