 Penetration-testing-Report
Footprinting and network scanning
WEEK2 PROJECT REPORT | CYBERSECURITY | NETWORKWALKS

Pentester Name
(Cybersecurity interns)
Gande Stephen
Program/Batch
B084-Networkwalks
Date
11 Novermber 2026
Modules completed
W2-PM1 (Multiple Kali Tools)
W2-PM5 (Zenmap Scanning)
  Target
1. Networkwalks (secured written permission already)
2. My own local LAN Network
Permission secured from client?
yes
Phases covered
Phase 1: Reconnaissance & Footprinting
Phase 2: Scanning & Network Discovery

Liability Disclaimer
I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

	theHarvester on microsoft.com; Zenmap local network scan
   Introduction
In this practical, I carried out domain reconnaissance using Kali Linux and a collection of information-gathering tools. My main focus was to understand what information about a domain and its web infrastructure could be identified before attempting any security testing. I worked mainly with networkwalks.com, using WHOIS, WhatWeb, nslookup, curl, WAFW00F and DNSRecon. I also practised theHarvester against microsoft.com and used Zenmap to discover active devices on a local network.
The exercise helped me connect the commands I had learned in cybersecurity with actual outputs. Instead of only reading about reconnaissance, I ran the tools, observed their responses and considered what each result could mean to a security analyst. I also learned that different tools reveal different types of information, so one result should not be treated as a complete picture.
   Aim and Objectives
The aim was to practise reconnaissance and identify publicly available technical information about a domain and a network.
●	Collect domain registration and name-server information using WHOIS.
●	Identify website technologies and server details with WhatWeb.
●	Resolve a domain name to an IP address using nslookup.
●	Inspect HTTP response headers using curl.
●	Identify whether a web application firewall was detected using WAFW00F.
●	Enumerate DNS records with DNSRecon.
●	Practise public-source information gathering with theHarvester.
●	Use Zenmap to identify active hosts on a local network.
 
   Tools and Methodology
I used Kali Linux as my working environment. I approached the exercise in stages, starting with basic domain registration information, then moving to website fingerprinting and DNS checks. After that, I checked the web server response and firewall fingerprint before carrying out DNS enumeration. This order made it easier for me to build an understanding of the target from general information to more specific technical details.
    WHOIS
I used the whois command to examine the registration record for networkwalks.com. The record identified GoDaddy.com, LLC as the registrar and showed that the domain was created on 6 November 2019, with a displayed registration expiry date of 6 November 2027. The record also listed domain status protections, including clientTransferProhibited, clientUpdateProhibited, clientRenewProhibited and clientDeleteProhibited.
The registrant information was privacy-protected through Domains By Proxy, LLC. This meant that the personal registrant details were not openly displayed in the record I reviewed. I also noted the listed name servers and that DNSSEC was marked as unsigned. From this step, I understood how WHOIS can provide useful administrative information without necessarily revealing the identity of the domain owner.
    WhatWeb
I ran WhatWeb against networkwalks.com to identify the technologies used by the website. The HTTP request to the non-secure version returned a connection-reset message, but the HTTPS check succeeded with a 200 OK response. WhatWeb identified Apache as the web server, WordPress 7.1.4 as the content management system, Bootstrap 7.1.4, jQuery 3.7.1 and WordPress Download Manager 3.3.76. It also reported the website title as Networkwalks Academy and displayed the IP address 192.232.216.135.
This result gave me an overview of the website's technology stack. I learned that technology fingerprinting can help a security team understand what software may need to be maintained and checked for known vulnerabilities. However, the presence of a particular product or version alone does not prove that the website is vulnerable.
    nslookup
I used nslookup with Google's DNS resolver at 8.8.8.8. The command returned a non-authoritative answer showing that networkwalks.com resolved to 192.232.216.135. This confirmed that the domain name could be translated into an IP address at the time of the test. I understood from this step that DNS is an important starting point because it connects human-readable domain names to network addresses.
 
   Web Response and DNS Findings
    curl -I
I used curl -I https://networkwalks.com to request the HTTP response headers without downloading the full webpage. The server returned HTTP/2 200, which indicated that the HTTPS endpoint responded successfully to my request. The headers identified Apache as the server and included a content type of text/html; charset=UTF-8. I also observed a secure, HttpOnly cookie named __wpdm_client and headers such as referrer-policy and permissions-policy.
This exercise helped me understand that response headers can reveal useful information about how a website communicates with browsers. They can also show whether certain security-related settings are present. I did not treat the headers as proof that the site was fully secure; they were only the information returned by this particular request.
    WAFW00F
I ran WAFW00F against networkwalks.com to check for a web application firewall. The tool identified the site as being behind ModSecurity (SpiderLabs) WAF and reported that it made two requests. This was an important observation because a web application firewall can help filter or monitor potentially harmful web requests.
From this result, I learned how defensive technologies can be identified during reconnaissance. The detection does not tell me the exact firewall rules, configuration quality or whether every attack would be blocked. It only indicates that the tool fingerprinted ModSecurity as the protection in front of the website.
    DNSRecon
I used dnsrecon -d networkwalks.com to enumerate DNS information. The output reported the SOA record with ns6135.hostgator.com, and name servers including ns6135.hostgator.com and ns6136.hostgator.com. It also identified an MX record for mail.networkwalks.com, which resolved to 192.232.216.135, and an A record for networkwalks.com pointing to the same IP address.
DNSRecon also returned TXT records. One was a Google site-verification value, while another contained an SPF policy beginning with v=spf1 and listing permitted sending sources. The tool identified SRV records associated with cPanel email autodiscovery and reported eight records found. It also displayed an error indicating that it received no answer for its DNSSEC query. This was consistent with the WHOIS information that marked DNSSEC as unsigned.
I found this step useful because it showed that DNS contains more than the IP address of a website. Records can provide information about mail services, name-server infrastructure, verification and service discovery. I also learned to report errors accurately rather than assuming that a failed query means the entire domain is unavailable.
 
    theHarvester and Zenmap Practical Exercises
     theHarvester
For another part of the exercise, I practised theHarvester against microsoft.com. My first command used an incorrect option, -1, and the tool returned an unrecognized-arguments error. I reviewed the usage information and corrected the command to use -l 50. This was a useful learning moment because it reminded me to check a tool's help or usage output when a command does not run as expected.
The corrected command was theHarvester -d microsoft.com -l 50 -b all. The run attempted to use multiple public data sources. Several sources could not be queried because API keys were missing or invalid. Despite those limitations, the output reported 111 IP addresses, seven email addresses and 9,966 hosts. It also showed that the Hudson Rock module extracted 44 hosts from employee URLs, while reporting zero IPs, emails and stealer records from that module.
I understood that theHarvester can collect publicly indexed information such as domains, hosts, IP addresses and email addresses. However, the results depend on the available sources, API access and the search engine responses. The missing-key messages meant that the scan was not a complete collection from every listed source. I therefore treated the figures as the output of this particular run, not as a complete inventory of Microsoft's infrastructure.
    Zenmap
I also used Zenmap, the graphical interface for Nmap, to practise host discovery on a local network. The scan output showed the network range being checked and reported that two hosts were up: 10.160.116.66 and 10.160.116.3. For 10.160.116.66, the output showed a response time of approximately 0.0051 seconds and a MAC address of AE:7B:F1:FF:01:AA, with the vendor listed as unknown. The scan completed after checking 256 IP addresses in approximately six seconds.
This part of the practical helped me understand the difference between domain reconnaissance and local network discovery. The Zenmap output showed which devices responded on the scanned local network; it did not provide evidence that those devices were part of networkwalks.com. The output also did not list open ports or services, so I have not claimed that any particular port was open.
   Summary of Key Findings
●	networkwalks.com resolved to 192.232.216.135 during the checks.
●	WHOIS identified GoDaddy as registrar and privacy protection for registrant details.
●	WhatWeb identified Apache, WordPress and related web technologies.
●	The HTTPS endpoint returned HTTP/2 200 to the curl header request.
●	WAFW00F fingerprinted ModSecurity (SpiderLabs) as the WAF.
●	DNSRecon identified SOA, NS, MX, A, TXT and SRV records, and reported eight records.
●	theHarvester's microsoft.com run reported 111 IPs, seven emails and 9,966 hosts, with multiple data-source/API limitations.
●	Zenmap identified two responsive hosts on the scanned 256-address local network.
 
   Challenges and Lessons Learned
I encountered a few limitations while carrying out the practical. The HTTP version of the website reset the connection, while the HTTPS version worked. My first theHarvester command also failed because I used an invalid option. In addition, several theHarvester data sources required API keys that were not configured. DNSRecon returned no answer for its DNSSEC query. These issues did not stop the whole exercise, but they affected what I could confirm.
The main lesson I learned is that reconnaissance is a process of collecting and carefully interpreting evidence. A tool may identify a technology, return a partial list or fail to reach a data source. I should record exactly what happened, understand the limitations and avoid making claims that go beyond the output. I also learned to use more than one tool because WHOIS, DNSRecon, WhatWeb, curl, WAFW00F, theHarvester and Zenmap each answer different questions.
   Recommendations
●	Keep website software, plugins and server components updated and remove components that are no longer required.
●	Review HTTP-to-HTTPS handling and maintain appropriate TLS configuration.
●	Review response headers and cookie settings periodically to ensure they match the organisation's security requirements.
●	Maintain and review DNS records, SPF policy and DNSSEC configuration as appropriate for the domain's operational needs.
●	Keep the web application firewall properly configured, monitored and updated; detection alone does not establish that it is effective.
●	Use authorised asset inventories and periodic vulnerability assessments to validate externally visible services.
●	When using OSINT tools, configure approved API keys where appropriate and document sources that could not be queried.
   Conclusion
Overall, this practical gave me hands-on experience with domain reconnaissance and basic network discovery in Kali Linux. I was able to collect registration details, identify website technologies, resolve DNS information, inspect HTTP headers, detect a web application firewall and enumerate DNS records. I also practised gathering public information with theHarvester and discovering active hosts with Zenmap.
The most important thing I gained from this exercise was confidence in using reconnaissance tools and interpreting their output. I now understand that reconnaissance does not automatically mean a system has been compromised or is vulnerable. It is an information-gathering stage that helps a security analyst understand the environment and decide what should be examined further. I carried out the activities as a learning exercise and recognise that scans and testing must only be performed on systems for which permission has been granted.
Reference
Practical evidence and command outputs: W2PM1.docx, including WHOIS, WhatWeb, nslookup, curl, WAFW00F, DNSRecon, theHarvester and Zenmap results.
Note: The theHarvester exercise used microsoft.com, while the main domain-enumeration activities used networkwalks.com. The Zenmap result relates to a local network scan. These results are reported separately to avoid mixing different targets.

   Evidence collected

  <img width="1348" height="629" alt="cap3" src="https://github.com/user-attachments/assets/024c453f-49cb-4f1a-abd7-e421c107427e" />
<img width="1358" height="627" alt="cap2" src="https://github.com/user-attachments/assets/c48a2efe-e1b3-4e44-b20b-2a2f578d9c44" />
<img width="1364" height="616" alt="cap1" src="https://github.com/user-attachments/assets/2e0497cd-b7c6-45e4-b688-cc380238293b" />
<img width="1365" height="621" alt="cap4" src="https://github.com/user-attachments/assets/a581ee6d-292a-4866-aaf6-b455e28cf6d4" />
<img width="1365" height="395" alt="cap5" src="https://github.com/user-attachments/assets/b28057c0-0d8f-4523-a4b0-a8d897f4b618" />
<img width="1354" height="354" alt="cap6" src="https://github.com/user-attachments/assets/14ba80a9-e6b7-4512-95be-d5331274a7c5" />
<img width="1356" height="503" alt="cap7" src="https://github.com/user-attachments/assets/1f7dc7c2-346c-4f0d-8d4b-9e693560342a" />
<img width="1353" height="570" alt="cap8" src="https://github.com/user-attachments/assets/91c32c8d-03ef-43ea-9679-2fc15c66fdfc" />
<img width="1364" height="595" alt="cap9" src="https://github.com/user-attachments/assets/caa4a492-ff75-4f1d-8ad7-6dcf29b8bf5d" />
<img width="1359" height="616" alt="Hcap1" src="https://github.com/user-attachments/assets/6bcc639b-25f7-4205-a20c-4bbff6e26685" />
<img width="1366" height="599" alt="Hcap2" src="https://github.com/user-attachments/assets/5a5ca72e-cb55-4b76-a19c-749a0a95dd5b" />
<img width="1359" height="547" alt="Hcap3" src="https://github.com/user-attachments/assets/a8586763-a9e4-447d-ae9b-22d56b3e6659" />
<img width="1354" height="629" alt="Hcap4" src="https://github.com/user-attachments/assets/2a10fb20-e529-4257-a77c-41af88cec93d" />
<img width="1366" height="612" alt="Hcap5" src="https://github.com/user-attachments/assets/33d3da1d-3a96-4944-aa2f-976797ac430e" />
<img width="1361" height="614" alt="Hcap12" src="https://github.com/user-attachments/assets/fc879e96-e990-41e4-b9a0-66b19c3f2af2" />
<img width="1364" height="623" alt="Hcap13" src="https://github.com/user-attachments/assets/4b6bb02e-8d7c-42c7-a3f4-7a5e6fd10c78" />
<img width="1359" height="515" alt="zencap5" src="https://github.com/user-attachments/assets/cafc4b75-2fec-4216-89c4-c121020067ee" />
<img width="1354" height="723" alt="zencap6" src="https://github.com/user-attachments/assets/58242ed6-4550-4a9f-8934-399dbdfd2466" />
