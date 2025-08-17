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

Under the transaction.remote.address, I could see the highest number of connections belongs is suspicious, so I'll this try this IP

<img width="580" height="245" alt="image" src="https://github.com/user-attachments/assets/ff6525ce-90d6-4f92-bf5e-0c0f7adcd238" />


Answer: _10.0.2.15_


**Q2. What was the first scanner that the attacker ran against the web server?**

Nmap Scripting Engine


**Q3. What was the User Agent of the directory enumeration tool that the attacker used on the web server?**

Answer: _Mozilla/5.0 (Gobuster)_


**Q4. In total, how many requested resources on the web server did the attacker fail to find?**

Answer: _1867_


**Q5. What is the flag under the interesting directory the attacker found?**

Answer: _a76637b62ea99acda12f5859313f539a_


**Q6. What login page did the attacker discover using the directory enumeration tool?**

Answer: _/admin-login.php_


**Q7. What was the user agent of the brute-force tool that the attacker used on the admin panel?**

Answer: _Mozilla/4.0 (Hydra)_


**Q8. What username:password combination did the attacker use to gain access to the admin page?**

Answer: _admin:thx1138_


**Q9. What flag was included in the file that the attacker uploaded from the admin directory?**

Answer: _THM{ecb012e53a58818cbd17a924769ec447}_


**Q10. What was the first command the attacker ran on the web shell?**

Answer: _whoami_


**Q11. What file location on the web server did the attacker extract database credentials from using Local File Inclusion?**

Answer: _/etc/phpmyadmin/config-db.php_


**Q12. What directory did the attacker use to access the database manager?**

Answer: _/phpmyadmin_


**Q13. What was the name of the database that the attacker exported?**

Answer: _customer_credit_cards_


**Q14. What flag does the attacker insert into the database?**

Answer: _c6aa3215a7d519eeb40a660f3b76e64c_




**Conclusion**

After completing the log investigation, you can present confident findings that an attacker compromised the web server and database. You managed to follow the timeline of events, allowing for a clearer understanding of the incident and actions performed.

In response to this incident, Slingway Inc. should address the identified vulnerabilities promptly to enhance the security of its web server. Furthermore, the company should take appropriate steps to notify affected customers about the data breach and implement proactive security measures to mitigate future attacks.

Your investigation's comprehensive findings and actionable insights will enable Slingway Inc. to mitigate the damage caused by the compromised server, bolster its cyber security posture, and safeguard its customers' trust. Well done!
