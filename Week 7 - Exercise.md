<h1>Question 1: Analyse packet1.pcap and find the flag.</h1>
____________________________

<img width="945" height="474" alt="image" src="https://github.com/user-attachments/assets/618ea30f-eb5d-4e21-9148-ecb12ec932ea" />

Scroll down and we could see there is one packet that is not same as others which is packet number 37. The pattern is diferent

<img width="945" height="888" alt="image" src="https://github.com/user-attachments/assets/f60d4e11-c937-4316-a577-25967071d417" />

Open the packet and we can see a weird string and we can try to use cyberchef

<img width="945" height="622" alt="image" src="https://github.com/user-attachments/assets/7a4d49e5-fabb-49db-a3c0-99e9805cf900" />

Out of all the base number, only base64 gave us the most logical decode. So i just assume that was the answer

The flag is SUCTF2023{ai_is_cool}


<h1>Question 2: Analyse packet2.pcap and find the flag.</h1>
____________________________

<h1>Question 3: Interpret an Nmap Output</h1>
______________________

|Port    | State | Service      | Version                                  |
---------|-------|--------------|------------------------------------------|
21/tcp  | open   | ftp          | vsftpd 2.3.4
22/tcp  | open   | ssh	         | OpenSSH 5.3p1
80/tcp  | open   | http         | Apache 2.2.8
139/tcp | open   | netbios-ssn  | ddad
445/tcp | open   | microsoft-ds | Windows 7 Professional 7601 Service Pack 1
---------------------------------------------------------------------------

Questions: 
1.	What can an attacker do with each port?
   FTP
  	-	Anonymous login attempts
  	-	Upload/download files

    SSH
  	-	Brute force login
  	-	Credential stuffing

    HTTP
  	-	Web attacj
  	-	Directory navigating

    NetBIOS
  	-	Gather internal network info

    SMB
  	-	Remote Code Execution

   2. What vulnerabilities are likely present based on the version?
      vsftpd 2.3.4
      - Known for backdoor vulnerability (CVE-2011-2523)
     
      OpenSSH 5.3p1
      - Old version. Provide weak encryption, so brute-force is viable

      Apache 2.2.8
      - Outdated, vulnerable to RCE/XSS
     
      Windows 7 Professional 7601 Service Pack 1
      - EternalBlue (MS17-010). Exploitable using metasploit
     
   3. Which one is the highest risk and why?
      FTP
      - Has built-in backdoor
      - No need for brute force or any complex exploit
      - Can gain shell access immediately
     
   4. What attack path can be built from this?
      Common chain from recent lab or tryhackme
      1. Exploit vsftpd backdoor to gain access
      2.	Enumerate system
      3.	Use credential to access SSH
      4.	Pivot to SNB(445)
      5.	Escelate privilege or dump data
     
   5.  What should be the remediation?
      - Upgrade or remove vsftpd 2.3.4 entirely
      - Patch Windows
 	  - Disable unused ports
	  - Update Apache & SSH
     
    
<h1>Question 4: Identify the OS (OS Fingerprinting) - TTL</h1>
____________________________



