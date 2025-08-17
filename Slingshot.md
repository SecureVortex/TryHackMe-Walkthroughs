Slingway Inc., a leading toy company, has recently noticed suspicious activity on its e-commerce web server and potential modifications to its database. To investigate the suspicious activity, they've hired you as a SOC Analyst to look into the web server logs and uncover any instances of malicious activity.

To aid in your investigation, you've received an Elastic Stack instance containing logs from the suspected attack. Below, you'll find credentials to access the Kibana dashboard. Slingway's IT staff mentioned that the suspicious activity started on July 26, 2023.

By investigating and answering the questions below, we can create a timeline of events to lead the incident response activity. This will also allow us to present concise and confident findings that answer questions such as:

    What vulnerabilities did the attacker exploit on the web server?
    What user accounts were compromised?
    What data was exfiltrated from the server?

Instructions

First, click Start Machine to start the VM attached to this task. You may access the VM using the AttackBox or your VPN connection. You can start the AttackBox by pressing the Start AttackBox button on the top-right of this room. Note: The Elastic Stack may take up to 5 minutes to fully start up. If you receive any errors, give it a few minutes and refresh the page.

Once online, navigate to http://MACHINE_IP using a web browser.

You should see the Elastic login page. Please log in using the following credentials:

<img width="542" height="155" alt="image" src="https://github.com/user-attachments/assets/2aba5f9c-e7dd-4aa8-bac5-89b31c7f02e9" />

**Q1. What was the attacker's IP?**

I reviewed the transaction.remote.address field and noticed one IP with an unusually high number of connections, marking it as suspicious.

<img width="580" height="245" alt="image" src="https://github.com/user-attachments/assets/ff6525ce-90d6-4f92-bf5e-0c0f7adcd238" />


Answer: _10.0.2.15_


**Q2. What was the first scanner that the attacker ran against the web server?**

To identify scanning activity, I examined the user-agent field.

<img width="602" height="461" alt="image" src="https://github.com/user-attachments/assets/ccd9beb2-a621-4d65-8ed4-2dbed54f6c1a" />

I then sorted events by timestamp (old to new) to determine the earliest scanner used. The second log contained the answer under request.headers.user-agent.

<img width="1228" height="513" alt="image" src="https://github.com/user-attachments/assets/d52d4077-baf1-4a68-98e0-c26af28cb057" />

Answer: _Nmap Scripting Engine_


**Q3. What was the User Agent of the directory enumeration tool that the attacker used on the web server?**

By analyzing the most frequent values in the user-agent field, I spotted the enumeration tool used by the attacker.

<img width="588" height="436" alt="image" src="https://github.com/user-attachments/assets/a7a97f12-cd9e-4f18-a8bb-66f182c92ff0" />


Answer: _Mozilla/5.0 (Gobuster)_


**Q4. In total, how many requested resources on the web server did the attacker fail to find?**

Since missing resources typically generate 404 errors, I filtered for that status code and obtained the total count.

<img width="927" height="452" alt="image" src="https://github.com/user-attachments/assets/98f8dea4-127b-4d77-9c30-a3889af2ea88" />


Answer: _1867_


**Q5. What is the flag under the interesting directory the attacker found?**

To identify successful directory discoveries, I filtered responses with status 200. Knowing the attacker leveraged Gobuster, I tracked those requests, sorted them chronologically, and found the flag in the earliest log.

<img width="1497" height="460" alt="image" src="https://github.com/user-attachments/assets/236fd942-8f06-4b21-b1ee-6b48a35f3043" />


Answer: _a76637b62ea99acda12f5859313f539a_


**Q6. What login page did the attacker discover using the directory enumeration tool?**

Since enumeration was involved, I kept the user-agent filter unchanged. By narrowing results with status codes 200 and 404, I identified the discovered login page in the second log under http.url.

<img width="1480" height="517" alt="image" src="https://github.com/user-attachments/assets/ed8a5f75-4caa-4a51-97f7-af6e21440cd4" />


Answer: _/admin-login.php_


**Q7. What was the user agent of the brute-force tool that the attacker used on the admin panel?**

Brute-force attempts usually trigger multiple 401 Unauthorized responses. Filtering for those, I examined the top user-agent values and identified the brute-force tool.

<img width="760" height="402" alt="image" src="https://github.com/user-attachments/assets/8dcc7f88-2054-4881-b966-3e14b7d0b197" />


Answer: _Mozilla/4.0 (Hydra)_


**Q8. What username:password combination did the attacker use to gain access to the admin page?**

The attacker used Hydra for brute-forcing, so I filtered for successful attempts by excluding failed methods and narrowing results to Hydra’s user-agent. Only one event matched. The credentials were Base64-encoded in the headers, which I decoded with CyberChef.

<img width="1542" height="576" alt="image" src="https://github.com/user-attachments/assets/7a98f3b3-2182-4e11-95cb-125473fd0c98" />

<img width="873" height="538" alt="image" src="https://github.com/user-attachments/assets/d15b4d36-cb8d-490c-9134-726f7396df75" />


Answer: _admin:thx1138_


**Q9. What flag was included in the file that the attacker uploaded from the admin directory?**

Since the file was uploaded from the admin directory, I filtered requests with /admin/* in http.url. By inspecting the upload action in the log body, I found the flag.

<img width="1516" height="587" alt="image" src="https://github.com/user-attachments/assets/ac2548f7-4b4b-4f8d-9703-150f62044b84" />


Answer: _THM{ecb012e53a58818cbd17a924769ec447}_


**Q10. What was the first command the attacker ran on the web shell?**

From the previous step, I knew the uploaded file’s name (easy-simple-php-webshell.php). Filtering for successful (200) executions of that file revealed the first command executed.

<img width="1443" height="572" alt="image" src="https://github.com/user-attachments/assets/3dee59b4-e53e-4b86-ae4a-af10c628963a" />


Answer: _whoami_


**Q11. What file location on the web server did the attacker extract database credentials from using Local File Inclusion?**

Using the same filters as before, I reviewed related logs and located the file path accessed through LFI.

<img width="1447" height="641" alt="image" src="https://github.com/user-attachments/assets/eb6bc547-8296-41c3-b66e-7f1829867477" />


Answer: _/etc/phpmyadmin/config-db.php_


**Q12. What directory did the attacker use to access the database manager?**

Reapplying the /admin/* query, from question 9, but sorting results by timestamp allowed me to spot the directory used for accessing the database manager.

<img width="1556" height="511" alt="image" src="https://github.com/user-attachments/assets/e9a6e88b-4ff0-4d4f-bb9a-d036a191d482" />


Answer: _/phpmyadmin_


**Q13. What was the name of the database that the attacker exported?**

I modified the query by replacing admin with phpmyadmin in the http.url filter. Scanning through the logs revealed the exported database name.

<img width="1176" height="597" alt="image" src="https://github.com/user-attachments/assets/1a31b157-5725-4ed1-8eae-9b44b387b04c" />


Answer: _customer_credit_cards_


**Q14. What flag does the attacker insert into the database?**

Since the attacker inserted new data, I inspected the import file within the logs, which contained the inserted flag.

<img width="1517" height="535" alt="image" src="https://github.com/user-attachments/assets/7c70129c-c4c8-471f-ac45-25c5f60e783d" />


Answer: _c6aa3215a7d519eeb40a660f3b76e64c_




**Conclusion**

After completing the log investigation, you can present confident findings that an attacker compromised the web server and database. You managed to follow the timeline of events, allowing for a clearer understanding of the incident and actions performed.

In response to this incident, Slingway Inc. should address the identified vulnerabilities promptly to enhance the security of its web server. Furthermore, the company should take appropriate steps to notify affected customers about the data breach and implement proactive security measures to mitigate future attacks.

Your investigation's comprehensive findings and actionable insights will enable Slingway Inc. to mitigate the damage caused by the compromised server, bolster its cyber security posture, and safeguard its customers' trust. Well done!
