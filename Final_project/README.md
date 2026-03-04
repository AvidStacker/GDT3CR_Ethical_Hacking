# Ethical Hacking Project
## Rogue Access Point and Credential Harvesting using EvilPortal

**Author:** Isak Lagerberg 
**Course:** Ethical Hacking – GDT3CR  

---

## 1. Goal

The goal of this lab is to demonstrate how a rogue access point attack can be performed using EvilPortal to harvest user credentials through a malicious captive portal.

This project demonstrates how attackers can create fake Wi-Fi networks that impersonate legitimate networks and redirect users to phishing login pages.

The objectives of this project are:
* Demonstrate how rogue Wi-Fi access points are created
* Show how captive portal phishing attacks work
* Capture user credentials through a malicious portal
* Map the attack to the MITRE ATT&CK framework
* Analyze detection and mitigation techniques

---

## 2. Background

Wireless networks are commonly used in public environments such as cafés, airports, hotels, and universities. These networks are often open or weakly secured, which makes them attractive targets for attackers.

One common attack technique is the **Rogue Access Point (RAP)** attack. In this attack, the adversary creates a wireless network that impersonates a legitimate network. When victims connect to the malicious network, the attacker gains control of their network traffic. This allows the attacker to intercept or redirect communications.

A common method used together with rogue access points is the **captive portal phishing attack**. The victim is redirected to a login page before gaining internet access. The login page may impersonate legitimate services such as:
* Wi-Fi authentication portals
* Email login pages
* Social media platforms
* Enterprise authentication portals

If the victim enters their credentials, the attacker can capture them.

---

## 3. Lab Environment

The experiment was performed in a controlled lab environment to avoid affecting real networks. 
The overall lab setup used in this experiment is illustrated in **Figure 1**.

### Attacker System
* **Operating System:** Kali Linux
* **Hardware:** Wireless network adapter supporting AP mode

### Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Penetration testing platform |
| EvilPortal | Captive portal phishing framework |
| hostapd | Rogue wireless access point |
| dnsmasq | DNS spoofing and DHCP |
| Web server | Hosting phishing portal |

*Figure 1 – Lab environment overview*

![Lab environment overview](Screenshots/lab_setup.png)

---

## 4. Network Architecture

The attack environment consists of three main components:

1. Attacker machine (Kali Linux)
2. Rogue wireless access point
3. Victim device

The overall architecture of this setup is shown in **Figure 2**.

**Network Attack Flow:**
1. Victim Device connects to the network.
2. Traffic routes through the Rogue Access Point.
3. DNS Redirect forces traffic to a specific server.
4. Victim is presented with the Fake Captive Portal.
5. Entering details results in Credential Harvesting.

*Figure 2 – Rogue access point attack architecture*

![Attack architecture diagram](Screenshots/network_architecture.png)

The attacker controls the wireless network and all traffic that passes through it.

---

## 5. Setup

The lab environment was prepared using Kali Linux. 

**Steps performed:**
1. Start Kali Linux.
2. Connect a wireless adapter that supports **Access Point mode**.
3. Launch EvilPortal.
4. Configure the rogue wireless network.
5. Select a phishing portal template.

**Example command:**

```bash
sudo evilportal
```

Configuration options:
* Wireless interface
* SSID name
* Captive portal template
* Web server configuration

The EvilPortal interface used to configure the rogue access point is shown in **Figure 3**.

*Figure 3 – EvilPortal configuration interface*

![EvilPortal interface configuration](Screenshots/evilportal_setup.png)

---

## 6. Initial Access

The attacker creates a rogue Wi-Fi network designed to look similar to a legitimate network.

**Example:**
* **Legitimate network:** `Campus_WiFi`
* **Malicious network:** `Campus_Guest_WiFi`

Users may connect automatically if the network appears trustworthy or if the signal strength is stronger than the legitimate network.

An example of the rogue wireless network visible to the victim is shown in **Figure 4**.

*Figure 4 – Rogue access point visible in Wi-Fi network list*

![Rogue access point visible in WiFi list](Screenshots/rogue_wifi_network.png)

This stage represents the **Initial Access** phase of the attack.

---

## 7. Execution

When a victim connects to the rogue access point, the attacker intercepts all network traffic. 

This is achieved through:
* DHCP configuration
* DNS spoofing
* HTTP redirection

Any web request made by the victim is redirected to the captive portal through DNS manipulation and HTTP redirection.

As shown in **Figure 5**, the victim is redirected to a malicious captive portal page.

*Figure 5 – Victim redirected to malicious captive portal*

![Victim redirected to captive portal](Screenshots/captive_portal_page.png)

The login page is designed to appear legitimate in order to convince users to enter their credentials.

---

## 8. Credential Access

When the victim enters login information into the portal, the data is captured and stored by EvilPortal.

**Example captured credentials:**

```text
username: example@email.com
password: password123
```

These credentials can later be exported or used for further attacks.

---

## 9. Discovery

After a device connects to the rogue network, the attacker can observe information about connected devices. 

This may include:
* IP addresses
* MAC addresses
* Device types

Connected client devices detected on the rogue network are shown in **Figure 6**.

*Figure 6 – Connected client devices on rogue network*

![Connected client devices](Screenshots/connected_clients.png)

This information can help attackers identify potential targets.

---

## 10. Command and Control

The attacker maintains control over the rogue network and collects captured credentials through the EvilPortal logging system. Captured data is stored locally on the attacker's machine.

Captured credentials recorded by EvilPortal are shown in **Figure 7**.

*Figure 7 – Credential log output*

![Credential log output](Screenshots/credential_logs.png)

---

## 11. Exfiltration

Captured credentials may be exported from the logs for later use. 

The attacker may store information such as:
* Usernames
* Passwords
* Email addresses

This information may later be used to compromise accounts or perform further phishing attacks.

---

## 12. Impact

Successful rogue access point attacks may result in:
* Account compromise
* Identity theft
* Unauthorized access
* Corporate network intrusion

Organizations relying on weak authentication mechanisms are particularly vulnerable to these attacks.

---

## 13. Indicators of Compromise

Possible indicators of a rogue access point attack include:
* Duplicate wireless network names
* Unexpected captive portal pages
* Certificate warnings
* Network connectivity issues

Users should treat these indicators as potential signs of malicious activity.

---

## 14. Detection

Organizations can detect rogue access points using several security mechanisms.

### Wireless Intrusion Detection Systems (WIDS)
WIDS systems monitor wireless networks and detect unauthorized access points.

### Network Monitoring
Unusual DNS redirections or traffic anomalies may indicate rogue network activity.

### Security Awareness
User awareness training significantly reduces the success rate of phishing attacks.

---

## 15. MITRE ATT&CK Mapping

The attack demonstrated in this project can be mapped to several MITRE ATT&CK techniques.

| MITRE Technique | ID | Description |
|---|---|---|
| Adversary-in-the-Middle | T1557 | Intercepting network communication |
| Phishing | T1566 | Capturing credentials using fake portals |
| Credential Access | T1003 / T1555 | Obtaining authentication data |
| Collection | T1119 | Storing captured information |

A visual representation of the MITRE ATT&CK technique mapping is shown in **Figure 8**.

*Figure 8 – MITRE ATT&CK technique mapping*

![MITRE ATT&CK mapping diagram](Screenshots/mitre_mapping.png)

---

## 16. Preventive Protection

Several security measures can mitigate rogue access point attacks.

### User-Level Protection
* Avoid connecting to unknown Wi-Fi networks
* Verify HTTPS certificates
* Use VPN on public networks

### Organizational Protection
* Deploy WIDS/WIPS systems
* Implement WPA3 encryption
* Use enterprise authentication (802.1X)

### Technical Protections
* Certificate pinning
* DNS security monitoring
* Network segmentation

These measures significantly reduce the effectiveness of captive portal phishing attacks.

---

## 17. Conclusion

This project demonstrated how attackers can exploit insecure wireless environments by deploying rogue access points and malicious captive portals. 

Using the EvilPortal framework, the experiment showed how victims can be redirected to phishing login pages and how their credentials can be harvested. The experiment highlights the importance of wireless network security, user awareness, and monitoring technologies.

Organizations must implement proper security controls in order to prevent attackers from exploiting insecure wireless networks.

---

## References

1. MITRE ATT&CK Framework – https://attack.mitre.org  
2. Kali Linux Documentation – https://www.kali.org/docs/  
3. OWASP Wireless Security Testing Guide – https://owasp.org  
4. Scarfone, K. & Padgette, J. (NIST). Guide to Enterprise Wireless Network Security.
