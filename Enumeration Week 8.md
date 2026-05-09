***Challenge 1 — NetBIOS Enumeration***

Command:
nbtstat -a <target_IP>

Expected output:
NETBIOS Remote Machine Name Table

Name	Type  Status

WIN-SERVER		<00> UNIQUE Registered DOMAIN	<00> GROUP Registered ADMINISTRATOR <03> UNIQUE Registered WIN-SERVER		<20> UNIQUE Registered

Interpretation:
- <00> UNIQUE → hostname
- <00> GROUP → domain/workgroup
- <03> → Messenger service
- <20> → File server service (SMB enabled)

<img width="1050" height="876" alt="image" src="https://github.com/user-attachments/assets/d3a60e45-4e24-47bf-a168-0a80c10fade5" />
Tried to scan from windows to Metasploit but the netbios port is closed, so from the windows terminal it said unreachable

***Challenge 2 — Fast Nmap Scan***

<img width="1050" height="771" alt="image" src="https://github.com/user-attachments/assets/314de994-d4f8-480b-b9cd-69d348e7e4ed" />
Scanned to Metasploit

<img width="1050" height="909" alt="image" src="https://github.com/user-attachments/assets/3dd28db8-6d9d-4943-8d87-4e59adf676f7" />
Zenmap interface

Security Impact:
Multiple exposed services open increase the chance of to be attack by attacker


***Challenge 3 — DNS Records***

Find any public DNS.
Commands:
- nslookup <domain> dig ANY <domain> dig MX <domain>
Expected findings:

- A record → IPv4 dig 
- AAAA record → IPv6
- MX → mail server
- NS → primary + secondary DNS

nslookup bing.com
<img width="897" height="534" alt="image" src="https://github.com/user-attachments/assets/9c141047-1924-483f-9701-51371be32bd8" />

dig ANY bin.com
<img width="897" height="534" alt="image" src="https://github.com/user-attachments/assets/ebbbd7ff-7ec5-4052-b599-0702ee1146df" />

dig MX bing.com
<img width="1050" height="604" alt="image" src="https://github.com/user-attachments/assets/b94c6f59-e387-4823-80d0-ecc0a60925e8" />

Finding
|Record             | Result                                 |
|-------------------|----------------------------------------|
| A                 |    150.171.27.10, 150.171.28.10        |
| AAAA              |    2620:1ec:33::10                     |
| NS                |    Azure DNS and NS1 servers           |
| NS                |    bing-com.mail.protection.outlook.com|

My Analysis:
Bing uses both the IPV4 and IPV6 infrastructure. DNS emuration allso revealed Microsoft Azure DNS architecture and Outlook-based infrastructure

Security Impact:
Public DNS information may assist attackers in infrastructure mapping, phishing preparation, and service targeting.


***Challenge 4 — SNMPwalk***

SNMP is on port 161.
Command:
snmpwalk -v1 -c public <IP>

Expected leaked info (depends on VM):

- sysName
- sysDescr
- network interface list
- system uptime
- sometimes processes

<img width="609" height="206" alt="image" src="https://github.com/user-attachments/assets/0a53d824-0e5f-4eab-92c3-eaf711a8e02c" />

Finding:
Port 161/UDP was found to be closed, tells that the SNMP service is not available on the target host. BUt, it still tells the Mac address vendor, which tells the system is running in a virtualized environment

Security Impact:
Since SNMP is unavailable, SNMP-based information leakage is prevented on this target. Disabling unnecessary services reduces the attack surface.
