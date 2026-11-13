# NETWORKWALKS-KANDAHURA_CALLIMUS_AYINE-B084N-WEEK2-PM1-5-CYBERSECURITY-RECON-AND-SCANNING
PENETRATION TESTING & RECONNAISSANCE ENGAGEMENT REPORT
Module Reference: W2-PM-FINAL | Cybersecurity & Ethical Hacking Internship (Networkwalks)
Pentester Name: Kandahura Callimus Ayine
Batch/Program: B084-Networkwalks
Date of Execution: 12 November 2026
1. Executive Summary
This comprehensive security report documents the execution of Phase 1 (External Footprinting & Reconnaissance) and Phase 2 (Active Network Scanning & Host Discovery) performed during Week 2 of the cybersecurity internship program. The objective of this assessment was to evaluate how publicly accessible information and internal network configurations expose infrastructure details to potential attackers.
Testing was performed under explicit written consent against the target domain networkwalks.com and against the pentester's local laboratory network. No exploitation or disruptive payload injection was performed; the engagement remained strictly within the bounds of information gathering, fingerprinting, and host enumeration.
2. Engagement Scope & Methodology
The assessment followed a structured reconnaissance framework, mirroring real-world threat intelligence and adversary tactics (Kill Chain Phase 1: Reconnaissance).
Parameter	Scope Detail
Target Domain	networkwalks.com (Hosted on HostGator infrastructure)
Local Subnet Target	10.205.137.0/24 (Local Area Network segment)
Authorization	Written authorization secured prior to testing
Primary Toolsets	Kali Linux (Reconnaissance suite), Windows CMD, Zenmap (Nmap GUI)
3. Phase 1: External Footprinting & Reconnaissance (W2-PM1)
External footprinting focuses on gathering unauthenticated intelligence about an organization's public-facing digital footprint without interacting directly with application business logic in a destructive manner.
3.1 Domain Registration Intelligence (whois)
•	Command Executed: whois networkwalks.com
•	Technical Findings: Disclosed registrar details (GoDaddy), registration timeline, abuse contact channels, and authoritative name servers (ns6135.hostgator.com, ns6136.hostgator.com).
•	Attacker Perspective: Administrative contacts and hosting provider details serve as foundational inputs for phishing campaigns, domain spoofing assessments, or infrastructure-level targeting. 
3.2 Web Stack Fingerprinting (whatweb)
•	Command Executed: whatweb networkwalks.com
•	Technical Findings: Identified the underlying Apache web server, WordPress CMS (Version 7.1.4), WP Download Manager plugin (Version 3.3.76), front-end frameworks (Bootstrap, jQuery 3.7.1), contact metadata (info@networkwalks.com), and direct target IPv4 address (192.232.216.135).
•	Attacker Perspective: Version-specific software exposure allows an attacker to query common vulnerability and exposure (CVE) databases for known unpatched exploits. 
3.3 DNS Infrastructure & IP Resolution (nslookup & dnsrecon)
•	Commands Executed: nslookup networkwalks.com and dnsrecon -d networkwalks.com
•	Technical Findings:
o	Resolved non-authoritative IPv4 address to 192.232.216.135.
o	Enumerated Start of Authority (SOA), Name Server (NS) entries, Mail Exchange (mail.networkwalks.com), Sender Policy Framework (SPF/TXT) strings, cPanel autodiscovery service records (SRV), and BIND DNS server software version (9.16.23-RH).
•	Attacker Perspective: Mapping peripheral DNS services uncovers auxiliary mail servers or forgotten subdomains that might lack proper security hardening. 
3.4 HTTP Protocol & Firewall Inspection (curl & wafw00f)
•	Commands Executed: curl -I [https://networkwalks.com](https://networkwalks.com) and wafw00f networkwalks.com
•	Technical Findings:
o	curl exposed HTTP/2 protocol headers, Apache server banners, caching guidelines, HttpOnly cookie flags, and active REST API pathways (/wp-json/).
o	wafw00f confirmed the presence of a ModSecurity (SpiderLabs) Web Application Firewall protecting the perimeter.
•	Attacker Perspective: WAF detection dictates whether automated scanners can run aggressively or if stealth/evasion techniques must be integrated into the testing pipeline.  
4. Phase 2: Internal Network Discovery & Scanning (W2-PM5)
Transitioning from external assets to local subnet analysis, active host discovery was performed to map active hardware nodes on the local network segment. 
4.1 Subnet Identification & Ping Sweep
•	Commands Executed:
o	Windows CMD: ipconfig and ipconfig /all (for interface validation)
o	Zenmap Execution: nmap -sn 10.205.137.0/24
•	Discovered Inventory Metrics:
o	Active Live Hosts: 5 devices
o	Discovered IPv4 Addresses: 10.205.137.62, 10.205.137.65, 10.205.137.182, 10.205.137.239, 10.205.137.7
o	Associated MAC Hardware Addresses: 8A:94:EB:CB:33:37, A2:AA:30:ED:87:51, CA:AA:8E:9F:2C:EB, A6:5B:45:59:4D:03, 38-BA-F8-05-F3-25
•	Topology Mapping: Visual node interconnections and layout mapping were reviewed via Zenmap's topology visualizer and exported for documentation archiving.
5. Risk Analysis & Threat Modeling Matrix
The observations compiled throughout both phases have been evaluated for security impact. Note that these entries represent reconnaissance findings and exposure vectors rather than verified vulnerabilities.
Ref #	Finding / Observation	Technical Evidence	Potential Security Impact	Risk Level
1	Software Version Disclosure	WhatWeb identified WordPress 7.1.4 and WP Download Manager 3.3.76	Facilitates targeted vulnerability research against specific component versions	Medium
2	DNS & Infrastructure Exposure	DNSRecon detailed mail servers, Bind version, and service records	Aids adversaries in building a complete organizational infrastructure map	Medium
3	Local Subnet Device Visibility	Zenmap discovered 5 active hosts and MAC identifiers	Exposes internal network layout and device presence to adjacent network users	Medium
4	Direct IP Exposure	Domain resolves directly to 192.232.216.135	Bypasses superficial hostname abstractions, revealing direct hosting locations	Low
5	HTTP Header & API Exposure	REST API endpoints (/wp-json/) and server banners exposed via curl	Exposes functional application pathways useful for endpoint enumeration	Low
6	Web Application Firewall Presence	ModSecurity (SpiderLabs) identified by wafw00f	Outlines perimeter defense mechanisms requiring evasion strategies	Low
6. Strategic Security Recommendations
To mitigate information leakage and harden the monitored environments against premature footprinting, the following measures are recommended:
•	Minimise Banner Grabbing: Restrict web server and application configuration files to suppress explicit version strings (e.g., hiding exact WordPress and plugin release numbers where feasible).
•	Audit HTTP Response Headers: Review application configurations to ensure sensitive internal headers or unnecessary API root paths (/wp-json/) are tightly controlled or rate-limited.
•	Harden DNS Records: Periodically review external DNS zones to ensure only business-critical service records, MX entries, and TXT configurations are published.
•	Maintain WAF Rulesets: Keep the ModSecurity Web Application Firewall updated and finely tuned to detect automated scanning signatures.
•	Internal Network Monitoring: Implement regular internal network discovery audits to maintain real-time visibility over connected assets and investigate unknown or unauthorized hardware connections.
7. Conclusion
This comprehensive assessment successfully demonstrates that intelligence can be gathered on target perimeters prior to active exploitation. Phase 1 highlighted how passive and semi-passive tooling (whois, whatweb, nslookup, curl, wafw00f, dnsrecon) extracts critical stack and architecture data, while Phase 2 proved the efficacy of active subnet scanning (Zenmap) for local asset discovery. Maintaining strict adherence to scope, ethical guidelines, and rigorous documentation remains foundational to professional penetration testing.

