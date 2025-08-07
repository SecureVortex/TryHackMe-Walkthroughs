**Scenario**

<img width="704" height="503" alt="image" src="https://github.com/user-attachments/assets/d82e8094-9369-4a0c-b67a-f39fae8e863c" />

Eric Fischer from the Purchasing Department at Bartell Ltd has received an email from a known contact with a Word document attachment.  Upon opening the document, he accidentally clicked on "Enable Content."  The SOC Department immediately received an alert from the endpoint agent that Eric's workstation was making suspicious connections outbound. The pcap was retrieved from the network sensor and handed to you for analysis. 
Task: Investigate the packet capture and uncover the malicious activities. 
*Credit goes to Brad Duncan for capturing the traffic and sharing the pcap packet capture with InfoSec community. 
NOTE: DO NOT directly interact with any domains and IP addresses in this challenge. 
________________________________________
Deploy the machine attached to this task; it will be visible in the split-screen view once it is ready.
If you don't see a virtual machine load, then click the Show Split View button.

<img width="940" height="84" alt="image" src="https://github.com/user-attachments/assets/7ecfb47c-386c-4dd1-aa15-5c7fb48b8d43" />

**Traffic Analysis**
Are you ready for the journey?
Please, load the pcap file in your Analysis folder on the Desktop into Wireshark to answer the questions below.
Answer the questions below

**Q1. What was the date and time for the first HTTP connection to the malicious IP? (answer format: yyyy-mm-dd hh:mm:ss)**
First change the viewing preferences
View -> Time Display Format -> Date and Time of the day 

<img width="940" height="631" alt="image" src="https://github.com/user-attachments/assets/e333e3e1-302d-4b26-9c64-5ce786e2890b" />

Use the display filter: http.request
Sort by time and identify the first HTTP connection made to a known malicious IP.

<img width="940" height="205" alt="image" src="https://github.com/user-attachments/assets/dba75658-a81a-4ddd-b3d5-97b68882f453" />

Answer: _2021-09-24 16:44:38_

**Q2. What is the name of the zip file that was downloaded?**
The file name was mentioned in the previous question in the log; however, we will proceed with reviewing the HTTP stream. 
Follow the HTTP stream or inspect HTTP GET requests to spot file downloads. The filename appears in the URI or Content-Disposition header.

<img width="940" height="228" alt="image" src="https://github.com/user-attachments/assets/93762b33-89a5-484a-b55a-9262dda0ab09" />

Answer: _documents.zip_

**Q3. What was the domain hosting the malicious zip file?**
The answer was in the same HTTP stream. 
Look at the Host field in HTTP request headers during the zip download.

<img width="940" height="248" alt="image" src="https://github.com/user-attachments/assets/571f8192-a910-4a93-84cf-8c2805af1ecc" />

Answer: _attirenepal.com_

**Q4. Without downloading the file, what is the name of the file in the zip file?**
Inspecting the HTTP response payload, we can find the filename by analyzing the zip structure.

<img width="940" height="414" alt="image" src="https://github.com/user-attachments/assets/caa39cc7-dc3d-4c09-bc9e-8e3515463446" />

Answer: _chart-1530076591.xls_

**Q5. What is the name of the webserver of the malicious IP from which the zip file was downloaded?**
Look for the Server header in the HTTP response

<img width="903" height="413" alt="image" src="https://github.com/user-attachments/assets/b943b165-92e4-4ed5-8437-9f7182214c12" />

Answer: _LiteSpeed_

**Q6. What is the version of the webserver from the previous question?**
The webserver version is listed under the 'x-powered-by' field.

<img width="750" height="353" alt="image" src="https://github.com/user-attachments/assets/77b03f63-fa55-4a8a-b6ce-acf04b5e708c" />

Answer: _PHP/7.2.34_

**Q7. Malicious files were downloaded to the victim host from multiple domains. What were the three domains involved with this activity?**
The room provides a hint to check HTTPS traffic and narrow down the timeframe from 16:45:11 to 16:45:30.
We can apply the following filter:
dns && (frame.time >= "Sep 24, 2021 16:45:11") && (frame.time <= "Sep 24, 2021 16:45:30")
Filter DNS traffic and identify different domains from which executable or suspicious payloads were downloaded.

<img width="940" height="208" alt="image" src="https://github.com/user-attachments/assets/919944cf-04fb-4f6a-84d6-3de7c9863b25" />

Answer: _finejewels.com.au, thietbiagt.com, new.americold.com_

**Q8. Which certificate authority issued the SSL certificate to the first domain from the previous question?**
The stream of the first domain is the previous question was 90. So, now we will search with the filter tcp.stream eq 90

<img width="940" height="250" alt="image" src="https://github.com/user-attachments/assets/25930b60-e3ca-4e89-ac60-d1124714a8d1" />

Answer: _GoDaddy_

**Q9. What are the two IP addresses of the Cobalt Strike servers? Use VirusTotal (the Community tab) to confirm if IPs are identified as Cobalt Strike C2 servers. (answer format: enter the IP addresses in sequential order)**
For this we’ll have to see the conversations.
Navigate to Statistics -> Conversations and sort by Bytes. Identify the external IP addresses that communicated with the specified IP address.

<img width="940" height="162" alt="image" src="https://github.com/user-attachments/assets/09e3e947-fd7c-4146-a46e-f432171abc3a" />

I checked the two IP addresses on VirusTotal to see if they were flagged as Cobalt Strike C2s.
Answer : _185.106.96.158, 185.125.204.174_

**Q10. What is the Host header for the first Cobalt Strike IP address from the previous question?**
Enter the query ip.addr == 185.106.96.158

<img width="940" height="296" alt="image" src="https://github.com/user-attachments/assets/27c0f7a2-8f70-4412-ad9f-3b6912c8a628" />

Follow the TCP stream to get the Host header.

<img width="940" height="180" alt="image" src="https://github.com/user-attachments/assets/0e6f947a-7b41-4cb8-9017-9b75326afcb9" />

Answer: _ocsp.verisign.com_

**Q11. What is the domain name for the first IP address of the Cobalt Strike server? You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).**
I searched the first IP on Alienvault OTX and checked the passive DNS connections

<img width="933" height="258" alt="image" src="https://github.com/user-attachments/assets/b15dd2b8-1ba1-4080-bd4d-0ad8b75743a9" />

Answer: _survmeter.live_

**Q12. What is the domain name of the second Cobalt Strike server IP?  You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).**
I searched the second IP on Alienvault OTX and checked the Passive DNS connections

<img width="940" height="209" alt="image" src="https://github.com/user-attachments/assets/4dcefd10-8139-49d4-baba-68beeaf70829" />

Answer: _securitybusinpuff.com_

**Q13. What is the domain name of the post-infection traffic?**
A hint has been given to filter Post HTTP traffic, we’ll enter the same query
http.request.method == “POST” and follow the HTTP stream

<img width="940" height="199" alt="image" src="https://github.com/user-attachments/assets/0bfdac67-04eb-4148-981e-c796c941d301" />

Answer: _maldivehost.net_

**Q14. What are the first eleven characters that the victim host sends out to the malicious domain involved in the post-infection traffic?**

<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/0e835df6-ac8e-493c-8e77-0b9b5938f357" />

Answer: _zLIisQRWZI9_

**Q15. What was the length for the first packet sent out to the C2 server?**
Here we can see  the length of the first packet sent to the C2 server.

<img width="940" height="94" alt="image" src="https://github.com/user-attachments/assets/178efbfe-548a-4a81-bfb0-f4cc41b3afbd" />

Answer: _281_

**Q16. What was the Server header for the malicious domain from the previous question?**
Following the HTTP stream, we can see the server header

<img width="940" height="187" alt="image" src="https://github.com/user-attachments/assets/d829cf2d-f83d-4bf4-a6cb-1efbc4ae63dd" />

Answer: _Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4_

**Q17. The malware used an API to check for the IP address of the victim’s machine. What was the date and time when the DNS query for the IP check domain occurred? (answer format: yyyy-mm-dd hh:mm:ss UTC)**
The victim machine uses the IP 10.9.23.102. To filter for frames containing 'api' and DNS traffic, use the query:
Ip.addr == 10.9.23.102 && frame contains "api" and dns.

<img width="940" height="385" alt="image" src="https://github.com/user-attachments/assets/d263c98e-4a7f-4854-a9cf-92ced7621d47" />

Answer: _2021-09-24 17:00:04_

**Q18. What was the domain in the DNS query from the previous question?**
The domain is depicted in the screenshot referenced in the preceding question.
Answer: _api.ipify.org_

**Q19. Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic?**
By checking SMTP traffic, we find the sender's domain. Examining the TCP stream reveals the sender's email address.

<img width="789" height="294" alt="image" src="https://github.com/user-attachments/assets/63f25244-487f-4551-b82f-ad45dfdbfa4f" />

answer: _farshin@mailfa.com_

**Q20. How many packets were observed for the SMTP traffic?**
The number of packet is given below

<img width="940" height="474" alt="image" src="https://github.com/user-attachments/assets/1ecfd871-51f3-453b-9738-1b04e9e02ffd" />

Answer: _1439_










