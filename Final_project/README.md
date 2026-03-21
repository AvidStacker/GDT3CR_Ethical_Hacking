# Ethical Hacking Project

## Evil Twin Attack and Credential Harvesting using a Raspberry Pi Captive Portal

**Author:** Isak Lagerberg  
**Course:** Ethical Hacking – GDT3CR  

---

# 1. Goal

The goal of this lab is to demonstrate how a **rogue access point attack** can be performed using a portable attack platform to harvest user credentials through a malicious captive portal.

This project demonstrates how attackers can create fake Wi-Fi networks that impersonate legitimate networks and redirect users to phishing login pages.

The objectives of this project are:

- Demonstrate how rogue Wi-Fi access points are created  
- Show how captive portal phishing attacks work  
- Capture user credentials through a malicious portal  
- Map the attack to the MITRE ATT&CK framework  
- Analyze detection and mitigation techniques  

---

# 2. Background

Wireless networks are commonly used in public environments such as cafés, airports, hotels, and universities. These networks are often open or weakly secured, which makes them attractive targets for attackers.

One common attack technique is the **Rogue Access Point (RAP)** attack. In this attack, the adversary creates a wireless network that impersonates a legitimate network. When victims connect to the malicious network, the attacker gains control of their network traffic. This allows the attacker to intercept or redirect communications.

A common method used together with rogue access points is the **captive portal phishing attack**. The victim is redirected to a login page before gaining internet access.

If the victim enters their credentials, the attacker can capture them.

---

# 3. Lab Environment

The experiment was performed in a **controlled and isolated lab environment** to avoid affecting real networks.

The attack platform was connected to a **personal mobile hotspot**, which served as the upstream internet connection.

### Attacker System

- Device: Raspberry Pi 4  
- Operating System: Kali Linux (ARM)  
- Wireless Adapter: Alfa AWUS036ACH  

### Tools Used

| Tool       | Purpose                                  |
| ---------- | ---------------------------------------- |
| Kali Linux | Penetration testing platform             |
| hostapd    | Creating the rogue wireless access point |
| dnsmasq    | DHCP server and DNS redirection          |
| Lighttpd   | Hosting the captive portal               |
| PHP        | Logging captured credentials             |
| tcpdump    | Network traffic analysis                 |

![Lab environment overview](Screenshots/lab_setup.png)

---

# 4. Network Architecture

## Network Attack Flow


Victim Device
↓
Rogue Access Point
↓
DNS Redirect
↓
Fake Captive Portal
↓
Credential Harvesting


![Attack architecture diagram](Screenshots/network_architecture.png)

---

# 5. Setup

The lab environment was prepared using a Raspberry Pi running Kali Linux.

Steps performed:

1. Start the Raspberry Pi  
2. Connect Alfa AWUS036ACH  
3. Configure hostapd  
4. Configure dnsmasq  
5. Deploy captive portal  
6. Enable credential logging  

---

# 6. Initial Access

Example networks:

Legitimate:


Campus_WiFi


Malicious:


Campus_Guest_WiFi


![Rogue access point visible](Screenshots/rogue_wifi_network.png)

---

# 7. Execution

Traffic is redirected using:

- DHCP  
- DNS spoofing  
- HTTP redirection  

![Captive portal](Screenshots/captive_portal_page.png)

---

# 8. Credential Access

Example captured credentials:


username: example@email.com

password: password123


![Captured credentials](Screenshots/credentials_captured.png)

---

# 9. Discovery

Information gathered:

- IP addresses  
- MAC addresses  
- device info  

![Connected clients](Screenshots/connected_clients.png)

---

# 10. Command and Control

Credentials are stored locally.

![Logs](Screenshots/credential_logs.png)

---

# 11. Exfiltration

Data stored:

- usernames  
- passwords  
- emails  

---

# 12. Impact

- Account compromise  
- Identity theft  
- Unauthorized access  

---

# 13. Indicators of Compromise

- Duplicate WiFi names  
- Captive portals  
- Certificate warnings  

---

# 14. Detection

- WIDS  
- Network monitoring  
- User awareness  

---

# 15. MITRE ATT&CK Mapping

| Technique | ID | Description |
|---|---|---|
| Adversary-in-the-Middle | T1557 | Traffic interception |
| Phishing | T1566 | Credential harvesting |
| Input Capture | T1056 | Logging credentials |
| Collection | T1119 | Storing data |

![MITRE ATT&CK mapping](Screenshots/mitre_mapping.png)

---

# Adversary-in-the-Middle Analysis

Although this project focuses on credential harvesting via a captive portal, the attack can be extended into a full Adversary-in-the-Middle (AiTM) scenario.

In such a scenario, the attacker would forward traffic between the victim and the internet, acting as an intermediary.

However, modern security mechanisms significantly limit this attack:

- HTTPS encryption prevents inspection of sensitive data  
- HSTS prevents SSL stripping  
- certificate validation blocks interception attempts  

As a result, attackers are often limited to:

- metadata (domains, IPs)  
- DNS requests  
- unencrypted HTTP traffic  

This highlights the importance of encryption in modern network security.

---

# 16. Preventive Protection

### User-Level

- Avoid unknown WiFi  
- Verify HTTPS  
- Use VPN  

### Organizational

- WPA3  
- 802.1X  
- WIDS  

---

# 17. Ethical Considerations

This experiment was conducted in a controlled environment using privately owned devices.

---

# 18. Conclusion

This project demonstrated how attackers can exploit wireless networks using rogue access points and captive portals.

It also highlighted the limitations of such attacks due to modern security protections.

---

# References

1. MITRE ATT&CK – https://attack.mitre.org  
2. Kali Linux Docs – https://www.kali.org/docs/  
3. OWASP – https://owasp.org  
