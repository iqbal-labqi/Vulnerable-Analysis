<h1>SECTION A — BASIC ENUMERATION</h1>

<h2>Challenge 1 — NetBIOS Enumeration</h2>

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

<h2>Challenge 2 — Fast Nmap Scan</h2>

<img width="1050" height="771" alt="image" src="https://github.com/user-attachments/assets/314de994-d4f8-480b-b9cd-69d348e7e4ed" />
Scanned to Metasploit

<img width="1050" height="909" alt="image" src="https://github.com/user-attachments/assets/3dd28db8-6d9d-4943-8d87-4e59adf676f7" />
Zenmap interface

Security Impact:
Multiple exposed services open increase the chance of to be attack by attacker


<h2>Challenge 3 — DNS Records</h2>

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


<h2>Challenge 4 — SNMPwalk</h2>

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

<h2>Challenge 5 — TTL OS Fingerprinting</h2>



TTL values reference: TTL	OS Guess
- 64	Linux / Unix
- 128	Windows
- 255	Cisco / BSD


Example:
Reply from <IP>: TTL=128 → Windows

<img width="986" height="319" alt="image" src="https://github.com/user-attachments/assets/745dbf14-25d9-48be-8918-6b13001e6782" />
TTL=64 ---Linux

<img width="641" height="102" alt="image" src="https://github.com/user-attachments/assets/95230005-df61-4ef4-a520-58b86b2079a0" />
Supposedly be windows, but no ICMP response

Possible cause:
- Host offline
- ICMP blocked (likely my case)
- Wrong IP
- Network issue

<h2>Challenge 6 — Anonymous LDAP Ǫuery</h2>

LDAP run on port 389. Command:
ldapsearch -x -H ldap://<IP> -b "dc=example,dc=com"


Expected outcomes:
 
- If anonymous bind allowed: returns user objects, descriptions, CN entries
- If not allowed: invalid credentials or empty results

<img width="1027" height="444" alt="image" src="https://github.com/user-attachments/assets/d357a6b1-5f5c-4c5f-983b-96bd90727501" />

Finding:
The target is alive but LDAP serviese is not running or accessible<br>

The target mentioned---Oracle VirtualBox virtual NIC<br>
<br>
Means, the target is virtualized, likely a VM

<h2>Challenge 7 — SMTP VRFY / EXPN</h2>

Command: <br>
nc <IP> 25 <br>
VRFY root <br>
EXPN admin
<br>
Expected:

•	Weak server: responds with 250 <user> → valid
•	Hardened server: 550 User unknown / 252 Cannot VRFY user


<img width="809" height="247" alt="image" src="https://github.com/user-attachments/assets/0d01a5b2-d87a-4fe7-abe3-93bafc476475" />

Finding:

|Item            |     Result
|----------------|------------
|SMTP Software   |Postfix                   |
|Operating System|Ubuntu                    |
|Hostname        |metasploitable.localdomain|
|Enumerated Users|root, msfadmin            |

<br>
My Analysis:<br>
The SMTP server responded to VRFY requests and disclosed potentially valid usernames. The mail banner also revealed the underlying mail software and operating system.
<br>
<br>

Security Impact:<br>
SMTP enumeration may help attackers identify valid accounts for password attacks and phishing campaigns.

<h2>Challenge 8 — NTP Enumeration</h2>
Command:
<br>ntpq -p <IP>

Expected:
<br>
List of time peers:
<br>remote	refid	st when poll reach
<br>*time1	.GPS.	1	32	64	377

Interpretation:

- * = currently selected peer
- st = stratum level
<br>
<img width="715" height="557" alt="image" src="https://github.com/user-attachments/assets/85f30277-4a8b-460b-b66b-8989be924280" />

Finding:<br>

|Target|REsult|
|------|------|
|192.168.0.235|123/udp closed|
|192.168.0.234|123/udp open|filtered|


192.168.0.235 port 123 is closed, so enumeration is impossible<br>
The ntpq query against 192.168.0.234 timed out and did not disclose synchronization peers.<br>

<br>
My analysis:<br>
The target may have NTP enabled but restrict remote peer enumeration or filter NTP query traffic. Most like because of;<br>
- the host did not allow NTP peer enumeration
- firewall blocked the query
- NTP daemon ignores remote requests

Security Impact:<br>
Restricting NTP enumeration reduces exposure of network topology and time synchronization infrastructure.

<h2>Challenge 9 — FTP Banner</h2>
Command:<br>
nc <IP> 21
<br><br>
Typical output:<br>
220 (vsFTPd 2.3.4)
 <br><br>
Students MUST note the version.<br>

<img width="579" height="352" alt="image" src="https://github.com/user-attachments/assets/6fad6808-29e4-4853-be91-87cc116c5a16" />
<img width="277" height="91" alt="image" src="https://github.com/user-attachments/assets/4901895c-8826-46db-91f7-017be01395bc" />

<br>
<br>
Finding:

|IP|Device|Result|Meaning|
|--|------|------|-------|
|192.168.0.235|Metasploitable|FTP open|- FTP service is actively running <br> - banner grabbing should work|
|192.168.0.234|Windows host|FTP filtered|- Firewall may blocking the traffic <br> - packets are silently dropped|


| State    | Meaning                              |
| -------- | ------------------------------------ |
| open     | Service actively listening           |
| closed   | No service running                   |
| filtered | Firewall/security device interfering |

<h2>Challenge 10 — Anonymous FTP Login</h2>

Command:<br>
ftp <IP>

Expected:

- If allowed: login succeeds with username anonymous
- Students list readable directories

<br>
<img width="472" height="288" alt="image" src="https://github.com/user-attachments/assets/b257e25c-855a-4d13-8afb-00a97719a91c" />

<br><br>
| Item             | Result                     |
| ---------------- | -------------------------- |
| FTP Software     | vsFTPd 2.3.4               |
| Authentication   | Anonymous login successful |
| System Type      | UNIX                       |
| Directory Access | Allowed                    |

My Analysis:<br>
The target FTP server allowed anonymous login without valid credentials. Directory listing and navigation commands were also can.
<br>
<br>
Security Impact:<br>
Anonymous FTP access may expose sensitive files and helps attackers in reconnaissance or unauthorized data access.

<h1>SECTION B — INTERMEDIATE ENUMERATION</h1>

<h2>Challenge 11 — SMB NSE Enumeration</h2>

Commands:<br>
- nmap --script smb-os-discovery -p445 <IP> 
- nmap --script smb-enum-users -p445 <IP><br>

Expected output:

- OS: Windows Server / Samba version
- Domain/workgroup name
- List of users (Administrator, Guest, etc.)

<img width="687" height="619" alt="image" src="https://github.com/user-attachments/assets/0c28564e-65c7-4be9-9cfd-b829502becc3" />
<img width="624" height="831" alt="image" src="https://github.com/user-attachments/assets/e1619fef-8d22-4209-bb7b-8ea3071991aa" />
<br>
Finding:
| Item         | Result              |
| ------------ | ------------------- |
| OS           | Unix                |
| SMB Software | Samba 3.0.20-Debian |
| Hostname     | metasploitable      |
| Domain       | localdomain         |


Users:<br>
- msfadmin
- root
- postgres
- mysql
- tomcat55
- www-data

<br>
My Analysiss:<br>
The SMB service allowed many user enumeration and disclosed detailed operating system information. Multiple service-related accounts tells that several installed applications and potential attack surfaces.<br>

Securiy Impact:<br>
SMB enumeration may expose valid usernames and infrastructure details useful for password attacks, privilege escalation, and exploitation planning.
<br>

<h2>Challenge 12 — Enum4linux</h2>

Command:<br>
enum4linux -a <IP>
<br><br>
Expected output: Users, Groups, NetBIOS info, SMB shares (IPC$, ADMIN$, etc.)
