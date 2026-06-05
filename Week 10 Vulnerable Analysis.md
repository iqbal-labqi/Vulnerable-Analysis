<h1>Vulnerable Analysis Week 10</h1>
<h2>Lab 1 (Foundation)</h2>
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
|1|Apache Tomcat AJP Connector Request Injection (Ghostcat)|CVE-2020-1745, CVE-2020-1938|9.8|8009|CRITICAL|
|2|Samba Badlock Vulnerability|CVE-2016-2118|7.5|445|HIGH|
|3|SSL Anonymous Cipher Suites Supported|CVE-2007-1858|5.9|25|MEDIUM|
|4|SSL DROWN Attack Vulnerability (Decrypting RSA with Obsolete and Weakened eNcryption)|CVE-2016-0800|5.9|25|MEDIUM|
|5|ICMP Timestamp Request Remote Date Disclosure|CVE-1999-0524|2.1|0|LOW|


<br>
<br>

<h2>Lab 2 (Core Analyst Skill)</h2>
<br>
<h3>Finding 1: Apache Tomcat Ghostcat (Web/App Category)</h3>
<br>
CVE ID: CVE-2020-1938
<br>
CVSS Base Score: 9.8 (Critical)
<br>
CVSS Vector String: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
<br>

+ Attack Vector (AV): Network (AV:N): The vulnerability can be exploited remotely across a network line.
+ Privileges Required (PR): None (PR:N): No authenticating username or password is required from the attacker.
+ User Interaction (UI): None (UI:N): It runs completely quietly without needing a victim to click or trigger anything.

<br>
CWE Mapping: CWE-20 (Improper Input Validation)
<br>
Context & Exploitation Decision:
<br>

- Is the service running & port reachable? Yes, Tomcat is active on port 8009
- Conclusion: Likely Exploitable. In this sandbox environment, you have an unauthenticated, remote exploit route exposed cleanly to your Kali IP, giving you an immediate green light for exploitation.

<br>

<h3>Finding 2: Samba Badlock Vulnerability (Service Category)</h3>

<br>
CVE ID: CVE-2016-2118
<br>
CVSS Base Score: 7.5 (High)
<br>
CVSS Vector String: CVSS:3.0/AV:N/AC:H/PR:N/UI:R/S:U/C:H/I:H/A:N
<br>

- Attack Vector (AV): Network (AV:N) — Probed over standard network pathways.
- Attack Complexity (AC): High (AC:H) — Requires precise execution timing or a Man-in-the-Middle position.
- User Interaction (UI): Required (UI:R) — An actual user needs to interact or create server traffic for this vulnerability to link up.

<br>
CWE Mapping: CWE-200 (Exposure of Sensitive Information)
<br>
Context & Exploitation Decision:
<br>

- Is the service running & port reachable? Yes, Samba is listening on port 445
- Conclusion: Not Exploitable in this environment. Even though the port is reachable, the High Complexity (AC:H) combined with User Interaction Required (UI:R) means it's dead in the water inside an isolated sandbox lab where no actual system users are triggering network traffic.

<br>

<h3>Finding 3: SSL DROWN Attack Vulnerability (Crypto Category)</h3>

<br>
CVE ID: CVE-2016-0800
<br>
CVSS Base Score: 5.9 (Medium)
<br>
CVSS Vector String: CVSS:3.0/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:N/A:N
<br>

- Attack Vector (AV): Network (AV:N) — Targets the internet/network protocol socket.
- Attack Complexity (AC): High (AC:H) — Requires specialized math tools to decode the Bleichenbacher RSA padding oracle and decrypt text.
- User Interaction (UI): None (UI:N) — No victim behavior required to make the server perform the mathematical computations.

<br>
CWE Mapping: CWE-327 (Use of a Broken or Risky Cryptographic Algorithm).
<br>
Context & Exploitation Decision:
<br>

- Is the service running & port reachable? Yes, the flawed protocol is active on port 25
- Conclusion: Not Likely Exploitable in this environment. While it doesn't need an active victim interaction (UI:N), a DROWN attack demands an incredible amount of intercepted ciphertext and computational overhead to successfully decrypt data. For a basic penetration assessment environment, you don't have the active TLS data stream required to make this realistic.

<br>

<h2>Lab 3 (Real-World Exercise)</h2>

<br>
<br>
<br>
<br>
<br>
<br>
