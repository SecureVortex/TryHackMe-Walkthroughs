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

Filter logs from 2022-03-01 till 2022-03-31.

<img width="1541" height="487" alt="image" src="https://github.com/user-attachments/assets/d62707e0-25a5-4900-a995-9916c5ee1ee4" />

Answer: _1482_


**Q2. What is the IP associated with the suspected user in the logs?**

I clicked on “source_ip” and found two IP addresses. One accounts for 99.6% of the traffic. But luckily, it’s the other one we’re interested in!

<img width="401" height="336" alt="image" src="https://github.com/user-attachments/assets/63e94f4f-cd20-432e-a3c3-2ef740a4aad2" />

Answer: _192.166.65.54_


**Q3. The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?**

By selecting the suspected IP and checking the User-Agent, you’ll see there is only one.

<img width="380" height="268" alt="image" src="https://github.com/user-attachments/assets/68784c5c-a0eb-4e50-9cd4-9f3ffcd5d882" />

Answer: _bitsadmin_


**Q4. The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?**

There are just two logs. I opened one, and the answer was in the “Host” field.

<img width="1035" height="502" alt="image" src="https://github.com/user-attachments/assets/67ed9757-b0fa-44c8-9225-36ac05ecde71" />

Answer: _pastebin.com_


**Q5. What is the full URL of the C2 to which the infected host is connected?**

I dug a bit deeper and located the URI in the logs. By combining the host with the URI, we obtain the full URL.

<img width="1005" height="246" alt="image" src="https://github.com/user-attachments/assets/9f90bf9e-95f1-42e4-9640-a2bb949e2149" />

Answer: _pastebin.com/yTg0Ah6a_


**Q6. A file was accessed on the filesharing site. What is the name of the file accessed?**

I visited the Pastebin link and found the answers to this question and the one below.

<img width="472" height="127" alt="image" src="https://github.com/user-attachments/assets/16d0b6ed-4020-447b-9d32-251276da7592" />

Answer: _secret.txt_


**Q7. The file contains a secret code with the format THM{_____}.**

<img width="356" height="128" alt="image" src="https://github.com/user-attachments/assets/acd5a4fb-b414-4e54-866d-a38f58dd946f" />

Answer: _THM{SECRET__CODE}_



