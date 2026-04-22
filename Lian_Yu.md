***Recon***

Deployed IP: 10.49.172.132

<img width="975" height="135" alt="image" src="https://github.com/user-attachments/assets/c390b19d-f6d6-41ea-acec-5500650203be" />

Check connection: ping 10.48.140.232

<img width="975" height="389" alt="image" src="https://github.com/user-attachments/assets/57bb3d55-ccd6-4730-abe2-d971a8efa058" />

Scan for the target machine: nmap 10.48.140.232




***Scanning***

Find open port: nmap -sC -sV -vv 10.48.140.232

<img width="975" height="111" alt="image" src="https://github.com/user-attachments/assets/d446d815-bd5d-43e6-9a4e-28594475ecd3" />

<img width="975" height="380" alt="image" src="https://github.com/user-attachments/assets/c378487c-554b-433f-a08d-f253c1bdc660" />

Found 4 open port:
1. Port 21/tcp - FTP - vsftpd 3.0.2
2. Port 22/tcp - ssh - OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
3. Port 80/tcp - http - Apache httpd
4. Port 111/tcp - rpcbind 2-4 (RPC #100000)





***Gaining Access***

My first thought is to go to the website of the IP

<img width="975" height="487" alt="image" src="https://github.com/user-attachments/assets/34b9d92c-378f-42b6-947c-269d949ce100" />

From here I decide to use gobuster to find hidden director:
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.48.140.232

<img width="975" height="397" alt="image" src="https://github.com/user-attachments/assets/c41ad69e-c693-4914-a0fa-51ce1c667700" />

http:// 10.48.140.232/island
*Clue: vigilante*

<img width="975" height="213" alt="image" src="https://github.com/user-attachments/assets/98ea4309-6d35-4d84-bc98-cddd2c16874f" />

gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.48.140.232/island

<img width="975" height="374" alt="image" src="https://github.com/user-attachments/assets/49bcd89e-fde3-474a-9866-30ddfe2f6e3e" />
<img width="975" height="108" alt="image" src="https://github.com/user-attachments/assets/06509854-4bf5-4151-92e3-41feec03718b" />

http:// 10.48.140.232/island/2100

<img width="975" height="471" alt="image" src="https://github.com/user-attachments/assets/d2b32690-7193-4385-8ffe-6d4a5ddc53ba" />

Press inspect

<img width="975" height="245" alt="image" src="https://github.com/user-attachments/assets/8006d27b-d247-4986-b74e-14cce796fe49" />

Using the gobuster again to find the file with .ticket extension
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.48.140.232/island/2100 -x .ticket

<img width="975" height="429" alt="image" src="https://github.com/user-attachments/assets/494838af-39b9-4f9d-8096-2ec738a35259" />
<img width="975" height="94" alt="image" src="https://github.com/user-attachments/assets/78c0473a-a9e6-45fb-b7c4-dddd7f9378cf" />

http://10.48.140.232/island/2100/green_arrow.ticket

<img width="975" height="526" alt="image" src="https://github.com/user-attachments/assets/8aabfc5c-fdb9-497d-9483-70b3ba8dc935" />

I acquire this hash, and use cyberchef to encode it

<img width="975" height="426" alt="image" src="https://github.com/user-attachments/assets/1b00c977-0ece-4de6-8596-d6606f9fb810" />

Out of all Base number, ONLY Base58 showing a make sense result. Others just random gibberish 
*Base58: !#th3h00d*

<img width="975" height="101" alt="image" src="https://github.com/user-attachments/assets/67ff583c-458e-4a0b-b260-c8f19ddebedc" />

From the clue we got above; we could try login using FTP
Username: vigilante
password: !#th3h00d

<img width="958" height="439" alt="image" src="https://github.com/user-attachments/assets/636d1580-db66-4a90-9c16-fa4e03cf0fa9" />
<img width="975" height="671" alt="image" src="https://github.com/user-attachments/assets/4fa13086-f87f-493f-ad60-74591673aeea" />

Using ls command, we can list down all the file in the system. We have Leave_me_alone.png, Queen’s_Gambit.png, aa.jpg. A bit exploration and we could see there is another user, slade

*Clue: slade*

<img width="975" height="262" alt="image" src="https://github.com/user-attachments/assets/4760d5bc-4166-496b-bc09-2268a8794d6d" />

Using the get command to download the image file
Get Leave_me_alone.png
Get Queen’s_Gambit.png
Get aa.jpg

<img width="975" height="84" alt="image" src="https://github.com/user-attachments/assets/c89fc732-2da6-4970-a45e-2a1ded4c4be9" />

My home directory…

<img width="975" height="457" alt="image" src="https://github.com/user-attachments/assets/0d3d81c6-3968-47a7-a19a-ce66e8aa543f" />

My file manager…

Check why the Leave_me_alone.png cant open using Hexeditor
<img width="975" height="277" alt="image" src="https://github.com/user-attachments/assets/288c8544-9dc7-4787-a6dd-16ce57ddff52" />

Wrong header, so we can use the Queen’s_Gambit.png as reference
<img width="975" height="129" alt="image" src="https://github.com/user-attachments/assets/e8f7d471-056b-486d-b9f1-4751515f5d7e" />
<img width="975" height="412" alt="image" src="https://github.com/user-attachments/assets/7f9f7faf-c66c-48b5-bcb2-9ddc4e79712e" />

And finally, we fix the png file and got the ‘password’

<img width="975" height="231" alt="image" src="https://github.com/user-attachments/assets/1119858e-94f8-4c64-b8bb-d7317ba53d2c" />

And from here we can run steghide to find hidden files inside the remaining picture, aa.jpg using passphrase: password. Extract the hidden zip, and we got 2 file

<img width="975" height="626" alt="image" src="https://github.com/user-attachments/assets/59b2de44-1500-4928-b5bc-be0e9119e86c" />

Nothing worth looking inside passwd.txt. But we acquire the SSH password inside ‘shado’
*Clue: M3tahuman*

<img width="975" height="94" alt="image" src="https://github.com/user-attachments/assets/cc321969-8581-467b-a4d6-eee916ea4de3" />

I proceed with the next step of the CTF. From the previous information I get;
Username: slade
Password: M3tahuman

<img width="975" height="618" alt="image" src="https://github.com/user-attachments/assets/aaa8ce02-8ea4-4bc9-8aae-8da392bfdcf6" />

Using ls command, I found user.txt.
Cat user.txt, and i got the first flag!
THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T }

<img width="975" height="97" alt="image" src="https://github.com/user-attachments/assets/925a71ce-388b-4622-80a2-54c1c9d814d1" />





***Escalate Privileges***

Next step is to find the root.
<img width="975" height="556" alt="image" src="https://github.com/user-attachments/assets/3c2215fe-6072-4ba9-b261-793bbec14921" />

Using sudo -l, to know what sudo command I can use. From the result, I use sudo pkexec /bin/bash. Immediately, I gain the root access!

Using the ls command and found the last flag:

THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}

<img width="975" height="104" alt="image" src="https://github.com/user-attachments/assets/6f4935d7-a381-4ac5-9ee4-ac49becbc53a" />

***Maintaining Access***
Maintain...






***Clear tracks***
<img width="975" height="334" alt="image" src="https://github.com/user-attachments/assets/a5969a6b-befd-463f-83f0-60c131aae355" />

Exit the SSH
