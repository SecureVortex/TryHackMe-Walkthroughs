**Introduction**

This room aims to be a supplementary room for Sigma rule creation. In this scenario, you will act as one of the Detection Engineers that will craft Sigma Rules based on the Indicators of Compromise (IOCs) collected by your Incident Responders.

You are tasked to create detection rules based on a new threat intel.

**Prerequisites**

This room requires basic knowledge of detection engineering and Sigma rule creation. We recommend going through the following rooms before attempting this challenge:

- Intro to Detection Engineering (coming soon)
- Sigma

**SigHunt Interface**

Before we proceed, deploy the attached machine in this task since it may take up to 3-5 minutes to initialize the services.

Then, use this link to access the interface - [http://MACHINE_IP](http://MACHINE_IP)

**How to use the SigHunt Interface:**

- **Run** - Submit your Sigma rule and see if it detects the malicious IOC.
- **Text Editor** - Write your Sigma rule in this section.
- **Create Rule** - Create a Sigma rule for the malicious IOC.
- **View Log** - View the log details associated with the malicious IOC.

<img width="1300" height="667" alt="4dbbf6e373e444b39500af6c68d98fbb" src="https://github.com/user-attachments/assets/3f82a685-2f03-4db9-82be-8ec1b6bdaae1" />


**Scenario**

You are hired as a Detection Engineer for your organization. During your first week, a ransomware incident has just concluded, and the Incident Responders of your organization have successfully mitigated the threat. With their collective effort, the Incident Response (IR) Team provided the IOCs based on their investigation. Your task is to create Sigma rules to improve the detection capabilities of your organization and prevent future incidents similar to this.

**Indicators of Compromise**

Based on the given incident report, the Incident Responders discovered the following attack chain:

- Execution of malicious HTA payload from a phishing link.
- Execution of Certutil tool to download Netcat binary.
- Netcat execution to establish a reverse shell.
- Enumeration of privilege escalation vectors through PowerUp.ps1.
- Abused service modification privileges to achieve System privileges.
- Collected sensitive data by archiving via 7-zip.
- Exfiltrated sensitive data through **cURL** binary.
- Executed ransomware with **huntme** as the file extension.

**Rule Creation Standards**

The Detection Engineering Team follows a standard when creating a Sigma Rule. You may refer to the guidelines below.

**Q1. What is the Challenge #1 flag?**

Answer: _THM{ph1sh1ng_msht4_101}_


**Q2. What is the Challenge #2 flag?**

Answer: _THM{n0t_just_4_c3rts}_


**Q3. What is the Challenge #3 flag?**

Answer: _THM{cl4ss1c_n3tc4t_r3vs}_


**Q4. What is the Challenge #4 flag?**

Answer: _THM{p0wp0wp0w3rup_3num}_


**Q5. What is the Challenge #5 flag?**

Answer: _THM{ov3rpr1v1l3g3d_s3rv1c3}_


**Q6. What is the Challenge #6 flag?**

Answer: _THM{h1d3_m3_1n_run0nc3}_


**Q7. What is the Challenge #7 flag?**

Answer: _THM{c0ll3ct1ng_7z_ftw}_


**Q8. What is the Challenge #8 flag?**

Answer: _THM{cUrling_0n_w1nd0ws}_


**Q9. What is the Challenge #9 flag?**

Answer: _THM{huntm3_pl34s3}_
