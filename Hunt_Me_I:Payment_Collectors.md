On Friday, September 15, 2023, Michael Ascot, a Senior Finance Director from SwiftSpend, was checking his emails in Outlook and came across an email appearing to be from Abotech Waste Management regarding a monthly invoice for their services. Michael actioned this email and downloaded the attachment to his workstation without thinking.

<img width="685" height="569" alt="image" src="https://github.com/user-attachments/assets/c29f2dfd-ff38-428f-9c9c-7e959f2f67af" />

The following week, Michael received another email from his contact at Abotech claiming they were recently hacked and to carefully review any attachments sent by their employees. However, the damage has already been done. Use the attached Elastic instance to hunt for malicious activity on Michael's workstation and within the SwiftSpend domain!

Connection Details

First, click Start Machine to start the VM attached to this task. You may access the VM using the AttackBox or your VPN connection. You can start the AttackBox by pressing the Start AttackBox button on the top-right of this room. Note: The Elastic Stack may take up to 5 minutes to fully start up. If you receive any errors, give it a few minutes and refresh the page.

Once online, navigate to http://MACHINE_IP using a web browser.

You should see the Elastic login page. Please log in using the following credentials:

<img width="546" height="148" alt="image" src="https://github.com/user-attachments/assets/01e3588a-c8ad-4e10-9cf0-90c4c20e7246" />


Q1. What was the name of the ZIP attachment that Michael downloaded?

Answer: _Invoice_AT_2023-227.zip_

Q2. What was the contained file that Michael extracted from the attachment?

Answer: _Payment_Invoice.pdf.lnk.lnk_

Q3. What was the name of the command-line process that spawned from the extracted file attachment?

Answer: _powershell.exe_

Q4. What URL did the attacker use to download a tool to establish a reverse shell connection?

Answer: _https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1_

Q5. What port did the workstation connect to the attacker on?

Answer: _19282_

Q6. What was the first native Windows binary the attacker ran for system enumeration after obtaining remote access?

Answer: _systeminfo.exe_

Q7. What is the URL of the script that the attacker downloads to enumerate the domain?

Answer: _https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1_

Q8. What was the name of the file share that the attacker mapped to Michael's workstation?

Answer: _SSF-FinancialRecords_

Q9. What directory did the attacker copy the contents of the file share to?

Answer: _C:\Users\michael.ascot\downloads\exfiltration_

Q10. What was the name of the Excel file the attacker extracted from the file share?

Answer: _ClientPortfolioSummary.xlsx_

Q11. What was the name of the archive file that the attacker created to prepare for exfiltration?

Answer: _exfilt8me.zip_

Q12. What is the MITRE ID of the technique that the attacker used to exfiltrate the data?

Answer: _T1048_

Q13. What was the domain of the attacker's server that retrieved the exfiltrated data?

Answer: _haz4rdw4re.io_

Q14. The attacker exfiltrated an additional file from the victim's workstation. What is the flag you receive after reconstructing the file?

Answer: _THM{1497321f4f6f059a52dfb124fb16566e}_
