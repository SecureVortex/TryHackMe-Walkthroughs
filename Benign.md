**Introduction**

We will investigate host-centric logs in this challenge room to find suspicious process execution. To learn more about Splunk and how to investigate the logs, look at the rooms splunk101 and splunk201.
Room Machine
Before moving forward, deploy the machine. When you deploy the machine, it will be assigned an IP. Access this room via the AttackBox, or via the VPN at MACHINE_IP. The machine will take up to 3-5 minutes to start. ll the required logs are ingested in the index win_eventlogs.

**Scenario: Identify and Investigate an Infected Host**

One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index win_eventlogs for further investigation.

About the Network Information

The network is divided into three logical segments. It will help in the investigation.

IT Department

    James
    Moin
    Katrina

HR department

    Haroon
    Chris
    Diana

Marketing department

    Bell
    Amelia
    Deepak

Q1. How many logs are ingested from the month of March, 2022?

I set the time filter from March 1st to March 31st, 2022, and used index=* to display all events for that period.

<img width="532" height="211" alt="image" src="https://github.com/user-attachments/assets/26b98847-3036-4f05-9578-6e3d91789b74" />


Answer: _13959_


Q2. Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?

While reviewing the usernames, I noticed a mismatch. 10 usernames were shown, but the total count was 11. To verify, I ran stats values(UserName) to list all accounts. That’s when the imposter stood out.

<img width="137" height="292" alt="image" src="https://github.com/user-attachments/assets/30a1ebb4-2b32-4fa6-bc15-06c8c86080f4" />

Answer: _Amel1a_


Q3. Which user from the HR department was observed to be running scheduled tasks?

I searched for events containing the keyword “schtasks”. Out of 87 results, I reviewed usernames linked to these tasks. Three were from IT, while one belonged to HR, revealing the answer.

<img width="777" height="466" alt="image" src="https://github.com/user-attachments/assets/e9e49ff2-0691-4cd7-80b2-8c70e8c5d446" />

Answer: _Chris.fort_


Q4. Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.

I filtered logs for HR accounts and reviewed their command-line executions. 

<img width="750" height="362" alt="image" src="https://github.com/user-attachments/assets/e9593314-a26d-4e6d-8c1d-5f228a19bfe8" />

Only one log entry showed a LOLBIN being used to fetch a payload, which revealed the user.

<img width="817" height="520" alt="image" src="https://github.com/user-attachments/assets/8562a527-ce24-4665-8420-c1380c6b2849" />

Answer: _haroon_


Q5. To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?

The process name used to download the payload was clearly listed in the same log.

<img width="911" height="502" alt="image" src="https://github.com/user-attachments/assets/536adb09-b62a-4a31-9e00-2bb3842f68a5" />

Answer: _certutil.exe_


Q6. What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)

The execution date was recorded within the same log entry.

<img width="932" height="300" alt="image" src="https://github.com/user-attachments/assets/bb3a76a6-3966-4350-bcc5-1ca6c17a912b" />

Answer: _2022-03-04_


Q7. Which third-party site was accessed to download the malicious payload?

The same log showed the external domain that was contacted to fetch the payload.

<img width="777" height="256" alt="image" src="https://github.com/user-attachments/assets/2281894d-113e-43ef-a0b3-34d19f01942a" />


Answer: _controlc.com_


Q8. What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?

The post-exploitation log clearly indicated the malicious file saved to the host system.

<img width="785" height="252" alt="image" src="https://github.com/user-attachments/assets/1dbde458-31eb-4f55-a51c-41e90d2cdf00" />

Answer: _benign.exe_


Q9. The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?

I followed the URL mentioned in the command line, which revealed the hidden flag.

<img width="148" height="75" alt="image" src="https://github.com/user-attachments/assets/26c02fa8-4af5-42ad-a82a-64005e0c84c3" />


Answer: _THM{KJ&*H^B0}_


Q10. What is the URL that the infected host connected to?

The full URL accessed by the compromised host was visible in the same command line entry.

c<img width="991" height="300" alt="image" src="https://github.com/user-attachments/assets/0a62af7f-3886-4c59-a0cb-535f0168de32" />

Answer: _https://controlc.com/e4d11035_




