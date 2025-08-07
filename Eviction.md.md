Eviction
<img width="128" height="128" alt="image" src="https://github.com/user-attachments/assets/e09719d1-b143-4fb1-8b4b-415458481ace" />


Sunny is a SOC analyst at E-corp, which manufactures rare earth metals for government and non-government clients. She receives a classified intelligence report that informs her that an APT group (APT28) might be trying to attack organizations similar to E-corp. To act on this intelligence, she must use the MITRE ATT&CK Navigator to identify the TTPs used by the APT group, to ensure it has not already intruded into the network, and to stop it if it has.
Please visit this link to check out the MITRE ATT&CK Navigator layer for the APT group and answer the questions below.

Q1. What is a technique used by the APT to both perform recon and gain initial access?
APT28 commonly uses Spearphishing Attachments for both reconnaissance and initial access. They craft deceptive emails containing malicious attachments and send them to targeted employees, gaining access once the attachment is opened.
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/5fbf5b7a-859d-445f-90d0-63be1a15cf76" />
Spearphishing link

Q2. Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources?
<img width="940" height="448" alt="image" src="https://github.com/user-attachments/assets/da00c9ba-233a-4905-9ea8-c89aa7f377dc" />
Email accounts

Q3. E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for? (Answer format: <technique 1> and <technique 2>)
Under the Execution, we have the two techniques that Sunny is looking for.
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/116734e9-0eda-4553-9bd0-40a21749c85b" />
Malicious file and malicious link

Q4. If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution? (Answer format: <technique 1> and <technique 2>)
We have the two scripting interpreters to identify successful execution.
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/e6ecbc2d-69c7-44b6-ab97-1980f8df696c" />
Powershell and Windows Command shell

Q5. While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes?
Under Persistence, the relevant registry keys that can be used to track changes are displayed.
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/1ed2cc07-5756-490c-b734-e17f9d780c3f" />
Registry run keys

Q6. Sunny identified that the APT executes system binaries to evade defences. Which system binary's execution should Sunny scrutinize for proxy execution?
We're now in Defense Evasion. Sunny should closely examine rundll32.exe for proxy execution. Attackers use this binary to run malicious code and bypass security measures.
<img width="940" height="450" alt="image" src="https://github.com/user-attachments/assets/6d25e5fa-0275-4d5a-8f14-342841904c70" />
Rundll32

Q7. Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery?
tcpdump suggests that network sniffing is being employed. This is a technique attackers use to capture network traffic and extract valuable information about the internal network.
<img width="940" height="448" alt="image" src="https://github.com/user-attachments/assets/27f14a6f-a877-4a0b-8a91-07b699c5dfe5" />
Network sniffing

Q8. It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces?
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/5bed5f1d-32a1-4602-b8ce-cfd4dc6d5335" />
SMB/Windows Admin shares

Q9. It looked like the primary goal of the APT was to steal intellectual property from E-corp's information repositories. Which information repository can be the likely target of the APT?
<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/be683385-c29c-49a9-8161-ba373b893597" /> 
Sharepoint

Q10. Although the APT had collected the data, it could not connect to the C2 for data exfiltration. To thwart any attempts to do that, what types of proxy might the APT use? (Answer format: <technique 1> and <technique 2>)
<img width="940" height="450" alt="image" src="https://github.com/user-attachments/assets/c760ddea-cede-4490-9b5d-87798fd50ba2" /> 
external proxy and multi-hop proxy

Q11. Congratulations! You have helped Sunny successfully thwart the APT's nefarious designs by stopping it from achieving its goal of stealing the IP of E-corp.
No answer needed

