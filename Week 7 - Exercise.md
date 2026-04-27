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

Different from the first exercise, ICMP is repetitive, no anomaly, and i conclude ICMP is not where the flag is. Then, i moved on to the TCP based communication.

Based on my observation, i noticed an unusual port which is **port 4444**. Port 4444 commonly associate as the default listener for the Metasploit Framework's reverse shells and payloads.

<img width="1001" height="829" alt="image" src="https://github.com/user-attachments/assets/0213f31f-6585-4ee9-a341-f1ee3ecdfa69" />

Scroll around, Found global_thermonuclear_war.gamerules.txt. Based on my experience, CTF sometime hide the flag inside txt file, so this might be the clue

<img width="1917" height="1021" alt="image" src="https://github.com/user-attachments/assets/49ca98ea-5e82-4ab1-8698-a29b974e663f" />

https://tinyurl.com/yr5zprz4

From there, i see a link and decide to use my chrome to search for it

<img width="1012" height="411" alt="image" src="https://github.com/user-attachments/assets/fe9f248f-8641-4efb-a80d-ac59b3012a1e" />

The is some kind of cipher. So i use google lens to find what cipher it using.

*TIC-TAC-TOE CODE from Club Penguin*

<img width="540" height="1206" alt="WhatsApp Image 2026-04-27 at 02 18 40" src="https://github.com/user-attachments/assets/743c2692-addd-4c73-a09a-547f28e6df59" />

The flag is: EXMACHINAAVA



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
      1.    Exploit vsftpd backdoor to gain access (FTP)
      2.	Enumerate system
      3.	Data Extraction or any Hidden content
      4.	Use credential to access SSH
      6.	Escelate privilege
     
   5.  What should be the remediation?
      - Upgrade or remove vsftpd 2.3.4 entirely
      - Patch Windows
 	  - Disable unused ports
	  - Update Apache & SSH
     
    
<h1>Question 4: Identify the OS (OS Fingerprinting) - TTL</h1>
____________________________

*Image 1*
<img width="794" height="278" alt="image" src="https://github.com/user-attachments/assets/717eb5b8-17f1-4210-946f-d24ee4c53ebf" />

Answer: Linux

*Image 2*
<img width="440" height="301" alt="image" src="https://github.com/user-attachments/assets/c1dacc11-cbe4-4257-9779-8572ab89f428" />

Answer: Networking equipment

*Image 3*
<img width="810" height="186" alt="image" src="https://github.com/user-attachments/assets/1154c373-d3ee-4d5c-acf6-d51ce2dfc255" />

Answer: Windows


<h1>Question 5: Analyse the Nessus file</h1>
______________________

Upload to your nessus (Network_Scan.nessus) and analyse the files. Focus on critical or high findings that was identifies in analysis named “Ghostcat”.

<img width="945" height="401" alt="image" src="https://github.com/user-attachments/assets/6ec500a0-2fc6-4a8a-ad6f-36d2cd583c16" />

<img width="945" height="502" alt="image" src="https://github.com/user-attachments/assets/0805a935-d6ca-4f90-9fe3-19ddceae3afe" />

<img width="945" height="254" alt="image" src="https://github.com/user-attachments/assets/a51718a7-7f5d-4646-bed0-6e85f3995b72" />

^| Nessus interface

1.	What is the affected Port number
    8009

2.	What is the Affected protocol
	TCP AJP protocol

3.	What is the CVSS Score of vulnerability found
	9.8

4.	Can you find any exploit related to this vulnerability?
	<img width="516" height="331" alt="image" src="https://github.com/user-attachments/assets/14243dbe-b04b-48a6-b8dc-3c1ffe9de3a2" />

5. Find CVE for this vulnerability.
	CVE-2020-1938, CVE-2020-1745
