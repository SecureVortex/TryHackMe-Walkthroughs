**AUTOPSY - DIGITAL FORENSICS INVESTIGATION**

**Introduction**

Welcome to the Autopsy digital forensics challenge! Autopsy is a powerful open-source digital forensics platform that provides a comprehensive suite of tools for investigating digital evidence. In this room, you'll learn to use Autopsy's capabilities to analyze a compromised system, recover deleted files, and build a timeline of events.

Autopsy integrates multiple forensic tools and provides an intuitive interface for conducting thorough digital investigations. You'll explore file system analysis, keyword searching, timeline generation, and evidence correlation while investigating a real cybercrime scenario.

**Learning Objectives**

Upon completion of this room, you will be able to:
- Navigate the Autopsy interface and create new cases
- Analyze file systems and recover deleted data
- Generate and interpret forensic timelines
- Perform keyword searches across digital evidence
- Extract and analyze metadata from various file types
- Document findings and generate forensic reports

**Scenario: Employee Data Theft Investigation**

SecureTech Industries suspects that a recently terminated employee, Alex Thompson, may have stolen sensitive company data before leaving the organization. The employee had access to intellectual property, customer databases, and financial information. 

A forensic image of Alex's workstation hard drive has been obtained, and you've been hired as a digital forensics investigator to:
- Determine if data theft occurred
- Identify what information was accessed or stolen
- Establish a timeline of suspicious activities
- Provide evidence suitable for legal proceedings

**Case Setup and Analysis**

Launch Autopsy and create a new case for this investigation. The disk image `workstation.dd` is available for analysis. The system was a Windows 10 workstation used primarily for data analysis and report generation.

<img width="580" height="380" alt="autopsy-case-setup" src="https://github.com/user-attachments/assets/n7o8p9q0-r1s2-t3u4-v5w6-x7y8z9a0b1c2" />

**Q1. What is the total size of the disk image?**

After adding the disk image as a data source, I checked the properties in the Data Sources view:

<img width="750" height="320" alt="disk-image-properties" src="https://github.com/user-attachments/assets/o8p9q0r1-s2t3-u4v5-w6x7-y8z9a0b1c2d3" />

Answer: _500 GB_

**Q2. How many files were deleted from the Recycle Bin?**

Using the Deleted Files view and examining the $Recycle.Bin folder structure:

<img width="820" height="400" alt="deleted-files-analysis" src="https://github.com/user-attachments/assets/p9q0r1s2-t3u4-v5w6-x7y8-z9a0b1c2d3e4" />

Answer: _23_

**Q3. What is the name of the external storage device that was connected?**

Examining the Registry and Windows Event Logs for USB device connections:

Using the OS Accounts and Registry analysis, I found evidence of external storage devices in the Windows Registry entries.

<img width="780" height="350" alt="usb-device-registry" src="https://github.com/user-attachments/assets/q0r1s2t3-u4v5-w6x7-y8z9-a0b1c2d3e4f5" />

Answer: _SanDisk Cruzer Glide_

**Q4. What time was the suspicious ZIP file created? (Format: YYYY-MM-DD HH:MM:SS)**

Searching for ZIP files and examining their metadata in the File Types view:

<img width="860" height="420" alt="zip-file-timeline" src="https://github.com/user-attachments/assets/r1s2t3u4-v5w6-x7y8-z9a0-b1c2d3e4f5g6" />

Answer: _2024-01-10 16:45:32_

**Q5. How many customer records were found in the database backup file?**

Analyzing database files found on the system and examining their contents:

Using Autopsy's database analysis capabilities and examining SQL dump files found in the user's documents:

Answer: _15847_

**Q6. What password was used to encrypt the confidential documents?**

Performing keyword searches for common password patterns and examining configuration files:

```
Keyword Search: password, passwd, pwd, secret, key
File Types: .txt, .doc, .docx, .pdf
```

<img width="740" height="360" alt="password-search" src="https://github.com/user-attachments/assets/s2t3u4v5-w6x7-y8z9-a0b1-c2d3e4f5g6h7" />

Answer: _CompanyData2024!_

**Q7. What email address was used to send the stolen data?**

Examining email artifacts, browser history, and system logs for evidence of data transmission:

Looking at the web browser history and email client data:

<img width="800" height="300" alt="email-evidence" src="https://github.com/user-attachments/assets/t3u4v5w6-x7y8-z9a0-b1c2-d3e4f5g6h7i8" />

Answer: _alex.temp.account@protonmail.com_

**Q8. What was the size of the largest file transferred to external storage?**

Analyzing file access patterns and examining large files that were accessed near the USB connection time:

Using the timeline analysis and correlating with USB device connections:

Answer: _2.3 GB_

**Advanced Autopsy Analysis Techniques**

**Timeline Analysis:**
Autopsy's timeline feature provides a comprehensive view of system activity:

<img width="900" height="450" alt="forensic-timeline" src="https://github.com/user-attachments/assets/u4v5w6x7-y8z9-a0b1-c2d3-e4f5g6h7i8j9" />

Key timeline events identified:
- USB device connection at 16:30:00
- Large file access patterns 16:30-17:15
- ZIP file creation at 16:45:32
- Email client activity 17:20-17:35
- System shutdown at 17:40:00

**File Signature Analysis:**
Using Autopsy's file type identification to find hidden or renamed files:
- Identified documents renamed with .tmp extensions
- Found database files disguised as image files
- Discovered encrypted archives with misleading names

**Registry Analysis:**
Key registry artifacts examined:
- USB device installation history
- Recent document access lists
- Network connection history
- Application usage patterns

**Browser Forensics:**
Analysis of web browser artifacts revealed:
- Visits to cloud storage services
- Searches for data encryption tools
- Email account creation activities
- File sharing service usage

**Detailed Investigation Findings**

**Data Theft Timeline:**

**January 10, 2024:**
- 16:28:45 - USB device "SanDisk Cruzer Glide" connected
- 16:30:12 - Access to customer database backup file
- 16:32:08 - Multiple large files copied to external device
- 16:45:32 - Creation of encrypted ZIP archive "quarterly_reports.zip"
- 16:58:23 - Email client (Outlook) started
- 17:05:17 - Large file attachment prepared
- 17:18:42 - Email sent to external account
- 17:22:15 - Browser activity to cloud storage service
- 17:35:48 - Final file uploads completed
- 17:40:12 - System shutdown

**Stolen Data Inventory:**

**Customer Information:**
- Database backup containing 15,847 customer records
- Customer contact information and purchase history
- Credit card processing logs (last 4 digits)

**Intellectual Property:**
- Product development roadmaps
- Source code for proprietary algorithms
- Market research and competitive analysis

**Financial Data:**
- Quarterly financial reports
- Budget projections and cost analyses
- Vendor contracts and pricing information

**Evidence of Deliberate Actions:**

**Data Preparation:**
- Files were systematically organized before copying
- Password protection applied to sensitive documents
- Use of external email account for data transmission
- Deletion of temporary files and browser history

**Anti-Forensic Attempts:**
- Clearing of recent documents lists
- Deletion of email traces
- Overwriting of temporary files
- Use of privacy browsing mode

**Digital Evidence Summary**

**File System Artifacts:**
```
Path: C:\Users\athompson\Documents\backup\
- customer_db_backup.sql (45.2 MB)
- product_roadmap_2024.docx (12.8 MB)
- financial_q4_2023.xlsx (8.4 MB)
- source_code_archive.zip (156.7 MB)
```

**Registry Evidence:**
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR
- SanDisk Cruzer Glide USB device (16GB capacity)
- First installed: 2024-01-10 16:28:45
- Last connected: 2024-01-10 17:35:22
```

**Network Artifacts:**
```
Browser History:
- protonmail.com (secure email service)
- mega.nz (cloud storage service)
- 7-zip.org (encryption software download)
- wikihow.com/Encrypt-Files
```

**Email Evidence:**
```
From: alex.temp.account@protonmail.com
To: personal.backup@gmail.com
Subject: Backup Files - Q4 Data
Attachments: quarterly_reports.zip (2.3 GB)
Timestamp: 2024-01-10 17:18:42
```

**Legal and Compliance Considerations**

**Evidence Chain of Custody:**
- Disk image created using write-blocking hardware
- MD5/SHA-256 hashes calculated and verified
- All analysis conducted on forensic copies
- Detailed documentation of all procedures

**Admissibility Requirements:**
- Standard forensic imaging procedures followed
- Tools and techniques are court-accepted
- Analysis methodology is reproducible
- Expert testimony available if required

**Recommendations**

**Immediate Actions:**
1. Report findings to legal counsel and HR department
2. Change passwords for all systems accessed by the employee
3. Monitor for use of stolen data in competitors or dark web
4. Notify affected customers if legally required
5. Review and revoke all access credentials

**Security Improvements:**
1. Implement Data Loss Prevention (DLP) solutions
2. Enhanced monitoring of USB device usage
3. Email monitoring and filtering improvements
4. Regular security awareness training
5. Endpoint detection and response (EDR) deployment

**Process Improvements:**
1. Enhanced employee offboarding procedures
2. Regular access reviews and privilege audits
3. Implementation of need-to-know access principles
4. Digital forensic readiness planning
5. Incident response procedure updates

**Conclusion**

The Autopsy investigation conclusively demonstrated that employee Alex Thompson engaged in systematic theft of sensitive company data. The evidence shows deliberate and premeditated actions to steal customer information, intellectual property, and financial data before termination.

Key evidence includes:
- Systematic copying of sensitive files to external storage
- Use of encryption to conceal stolen data
- Creation of external email accounts for data transmission
- Attempts to cover tracks through anti-forensic activities

This case highlights the importance of:
- Comprehensive digital forensics capabilities
- Proper employee access controls and monitoring
- Effective offboarding procedures
- Digital forensic readiness in corporate environments

The evidence collected through Autopsy provides a solid foundation for potential legal action and demonstrates the value of digital forensics in corporate security incidents.