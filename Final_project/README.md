# Ethical Hacking Project

## Rogue Access Point and Credential Harvesting using a Raspberry Pi Captive Portal

**Author:** Isak Lagerberg
**Course:** Ethical Hacking – GDT3CR

---

# 1. Goal

The goal of this lab is to demonstrate how a **rogue access point attack** can be performed using a portable attack platform to harvest user credentials through a malicious captive portal.

This project demonstrates how attackers can create fake Wi-Fi networks that impersonate legitimate networks and redirect users to phishing login pages.

The objectives of this project are:

* Demonstrate how rogue Wi-Fi access points are created
* Show how captive portal phishing attacks work
* Capture user credentials through a malicious portal
* Map the attack to the MITRE ATT&CK framework
* Analyze detection and mitigation techniques

---

# 2. Background

Wireless networks are commonly used in public environments such as cafés, airports, hotels, and universities. These networks are often open or weakly secured, which makes them attractive targets for attackers.

One common attack technique is the **Rogue Access Point (RAP)** attack. In this attack, the adversary creates a wireless network that impersonates a legitimate network. When victims connect to the malicious network, the attacker gains control of their network traffic. This allows the attacker to intercept or redirect communications.

A common method used together with rogue access points is the **captive portal phishing attack**. The victim is redirected to a login page before gaining internet access. The login page may impersonate legitimate services such as:

* Wi-Fi authentication portals
* Email login pages
* Social media platforms
* Enterprise authentication portals

If the victim enters their credentials, the attacker can capture them.

---

# 3. Lab Environment

The experiment was performed in a **controlled and isolated lab environment** to avoid affecting real networks.

To ensure that no university infrastructure or external networks were impacted, the attack platform was connected to a **personal mobile hotspot**, which served as the upstream internet connection.

The overall lab setup used in this experiment is illustrated in **Figure 1**.

### Attacker System

* **Device:** Raspberry Pi 4
* **Operating System:** Kali Linux (ARM)
* **Wireless Adapter:** Alfa AWUS036ACH

The Raspberry Pi functions as a **portable penetration testing platform** capable of running wireless attack tools and hosting rogue access points.

### Tools Used

| Tool       | Purpose                                  |
| ---------- | ---------------------------------------- |
| Kali Linux | Penetration testing platform             |
| hostapd    | Creating the rogue wireless access point |
| dnsmasq    | DHCP server and DNS redirection          |
| Lighttpd   | Hosting the captive portal               |
| PHP        | Logging captured credentials             |
| tcpdump    | Network traffic analysis                 |

*Figure 1 – Lab environment overview*

![Lab environment overview](Screenshots/lab_setup.png)

---

# 4. Network Architecture

The attack environment consists of four main components:

1. Raspberry Pi attacker device
2. Rogue wireless access point
3. Victim device
4. Mobile hotspot providing isolated internet connectivity

The overall architecture of this setup is shown in **Figure 2**.

**Network Attack Flow**

1. Victim device connects to the rogue wireless network.
2. The rogue access point assigns an IP address using DHCP.
3. DNS redirection forces all web traffic to the attacker’s server.
4. The victim is presented with a fake captive portal login page.
5. Credentials entered into the portal are captured and stored.

*Figure 2 – Rogue access point attack architecture*

![Attack architecture diagram](Screenshots/network_architecture.png)

The attacker controls the wireless network and all traffic that passes through it.

---

# 5. Setup

The lab environment was prepared using a **Raspberry Pi running Kali Linux**.

**Steps performed:**

1. Start the Raspberry Pi running Kali Linux.
2. Connect the Alfa AWUS036ACH wireless adapter.
3. Configure **hostapd** to create a rogue wireless access point.
4. Configure **dnsmasq** for DHCP services and DNS redirection.
5. Deploy a captive portal using a web server.
6. Enable credential logging using a PHP script.

The rogue access point configuration includes:

* Wireless interface
* SSID name
* DHCP address range
* DNS redirection

The captive portal is hosted locally on the Raspberry Pi and captures credentials submitted by connected clients.

---

# 6. Initial Access

The attacker creates a rogue Wi-Fi network designed to look similar to a legitimate network.

Example:

**Legitimate network**

```
Campus_WiFi
```

**Malicious network**

```
Campus_Guest_WiFi
```

Users may connect automatically if the network appears trustworthy or if the signal strength is stronger than the legitimate network.

An example of the rogue wireless network visible to the victim is shown in **Figure 3**.

*Figure 3 – Rogue access point visible in Wi-Fi network list*

![Rogue access point visible in WiFi list](Screenshots/rogue_wifi_network.png)

This stage represents the **Initial Access** phase of the attack.

---

# 7. Execution

When a victim connects to the rogue access point, the attacker intercepts and redirects network traffic.

This is achieved through:

* DHCP configuration
* DNS redirection
* HTTP redirection

Any web request made by the victim is redirected to the captive portal through DNS manipulation.

As shown in **Figure 4**, the victim is redirected to a malicious captive portal page.

*Figure 4 – Victim redirected to malicious captive portal*

![Victim redirected to captive portal](Screenshots/captive_portal_page.png)

The login page is designed to appear legitimate in order to convince users to enter their credentials.

---

# 8. Credential Access

When the victim enters login information into the portal, the data is captured and written to a log file stored on the attacker's Raspberry Pi.

Example captured credentials:

```
username: example@email.com
password: password123
```

These credentials are stored locally and can later be exported or analyzed.

---

# 9. Discovery

After a device connects to the rogue network, the attacker can observe information about connected devices.

This may include:

* IP addresses
* MAC addresses
* Signal strength
* Connection duration

Connected client devices detected on the rogue network are shown in **Figure 5**.

*Figure 5 – Connected client devices on rogue network*

![Connected client devices](Screenshots/connected_clients.png)

This information can help attackers identify potential targets.

---

# 10. Command and Control

The attacker maintains control over the rogue network and collects captured credentials through the local logging system.

Captured data is stored on the Raspberry Pi and can be monitored in real time.

Captured credentials recorded during testing are shown in **Figure 6**.

*Figure 6 – Credential log output*

![Credential log output](Screenshots/credential_logs.png)

---

# 11. Exfiltration

Captured credentials may be exported from the log files for later use.

The attacker may store information such as:

* Usernames
* Passwords
* Email addresses

This information may later be used to compromise accounts or perform additional phishing attacks.

---

# 12. Impact

Successful rogue access point attacks may result in:

* Account compromise
* Identity theft
* Unauthorized access
* Corporate network intrusion

Organizations relying on weak authentication mechanisms are particularly vulnerable to these attacks.

---

# 13. Indicators of Compromise

Possible indicators of a rogue access point attack include:

* Duplicate wireless network names
* Unexpected captive portal pages
* Certificate warnings
* Network connectivity issues

Users should treat these indicators as potential signs of malicious activity.

---

# 14. Detection

Organizations can detect rogue access points using several security mechanisms.

### Wireless Intrusion Detection Systems (WIDS)

WIDS systems monitor wireless networks and detect unauthorized access points.

### Network Monitoring

Unusual DNS redirections or traffic anomalies may indicate rogue network activity.

### Security Awareness

User awareness training significantly reduces the success rate of phishing attacks.

---

# 15. MITRE ATT&CK Mapping

The attack demonstrated in this project can be mapped to several MITRE ATT&CK techniques.

| MITRE Technique         | ID    | Description                              |
| ----------------------- | ----- | ---------------------------------------- |
| Adversary-in-the-Middle | T1557 | Intercepting network communication       |
| Phishing                | T1566 | Capturing credentials using fake portals |
| Input Capture           | T1056 | Collecting credentials entered by users  |
| Collection              | T1119 | Storing captured information             |

A visual representation of the MITRE ATT&CK technique mapping is shown in **Figure 7**.

*Figure 7 – MITRE ATT&CK technique mapping*

![MITRE ATT\&CK mapping diagram](Screenshots/mitre_mapping.png)

---

# 16. Preventive Protection

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

# 17. Ethical Considerations

This experiment was conducted strictly within a **controlled laboratory environment** using privately owned devices.

A personal mobile hotspot was used as the upstream internet connection to ensure that the experiment did not interfere with university infrastructure or external networks.

The techniques demonstrated in this report are intended solely for **educational purposes within an ethical hacking context**.

---

# 18. Conclusion

This project demonstrated how attackers can exploit insecure wireless environments by deploying rogue access points and malicious captive portals.

Using a Raspberry Pi attack platform, the experiment showed how victims can be redirected to phishing login pages and how their credentials can be harvested.

The experiment highlights the importance of wireless network security, user awareness, and monitoring technologies.

Organizations must implement proper security controls in order to prevent attackers from exploiting insecure wireless networks.

---

# References

1. MITRE ATT&CK Framework – https://attack.mitre.org
2. Kali Linux Documentation – https://www.kali.org/docs/
3. OWASP Wireless Security Testing Guide – https://owasp.org
4. Scarfone, K. & Padgette, J. (NIST). Guide to Enterprise Wireless Network Security.
