<h1>Lab 1 (Foundation)</h1>
---------------------------

*Machine Info*

Name | IP |
-----|----|
Kali | 10.0.2.5|
Metasploitable2 | 10.0.2.15
---------------------------

1. Check our connection with the target<br>
<img width="781" height="525" alt="image" src="https://github.com/user-attachments/assets/38830b2a-bdad-4789-b065-680457a20d8e" />
<br>
<br>
2. Tool used: Nessus
<br>
<br>
<img width="1407" height="395" alt="image" src="https://github.com/user-attachments/assets/f4d1a14c-5991-4928-845b-776c7097e5f2" />
<br>
<br>
<img width="1915" height="920" alt="image" src="https://github.com/user-attachments/assets/c6a2c0f2-1bfa-430b-a645-a235e42b3372" />
<br>
<br>
<img width="1721" height="704" alt="image" src="https://github.com/user-attachments/assets/143aa24a-0287-41af-bb59-af712a22062c" />
<br>
<br>

| No | Vulnerability Name |CVE| CVSS Score| Affected port/service | Severity |
-----|--------------------|---|-----------|-----------------------|----------|
|  1 |Canonical Ubuntu Linux SEoL (8.04.x)|None|10.0|80|CRITICAL|
|2|VNC Server 'password' Password|None|10.0|5900|CRITICAL|
|3|Apache Tomcat AJP Connector Request Injection (Ghostcat)|CVE-2020-1745, CVE-2020-1938|9.8|8009|CRITICAL|
|4|SSL Version 2 and 3 Protocol Detection|None|9.8|25, 5432|CRITICAL|
|5|Bind Shell Backdoor Detection|None|9.8|1524|CRITICAL|
