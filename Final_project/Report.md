# Airgeddon Lab – Evil Twin + Deauthentication + Captive Portal

## Ethical Hacking Project – Rogue Access Point Attack Simulation

---

# Goal

The goal of this lab is to demonstrate how an attacker can perform an Evil Twin attack using Airgeddon by combining deauthentication, access point impersonation, and a captive portal to achieve credential harvesting in a controlled environment.

---

# Summary

This lab demonstrates how multiple wireless attack techniques can be chained together. The attacker forces clients to disconnect from a legitimate network, lures them into connecting to a fake access point, and presents a captive portal to collect credentials.

---

# Background

This lab assumes basic knowledge of:

* Wi-Fi networking (SSID, BSSID, channels)
* WPA2 authentication
* Linux/Kali Linux usage
* Basic networking concepts

---

# Lab Scenario

In this scenario, an attacker targets a WPA2-protected wireless network. By performing a deauthentication attack, the attacker forces connected clients to disconnect. The attacker then presents a rogue access point with the same SSID. When the victim reconnects, they are redirected to a captive portal where credentials can be harvested.

---

# Lab Architecture

```
Real WiFi Network
        │
   (Deauthentication Attack)
        ↓
Victim Device
        ↓
Fake Access Point (Evil Twin)
        ↓
Captive Portal
        ↓
Credential Harvesting
```

---

# Hardware Requirements

## Attacker Device

* Raspberry Pi (Kali Linux)
* USB Wi-Fi adapter (monitor mode + packet injection support)

## Victim Device

* Smartphone or laptop

---

# Step 1 – Start Airgeddon

```bash
sudo airgeddon
```

---

# Step 2 – Interface Selection

Airgeddon will prompt:

```
Select an interface to work with:
```

Select your external wireless interface (e.g., wlan1).

**Important:**

* Ensure the interface supports monitor mode and packet injection
* External adapters are strongly recommended

---

# Step 3 – Main Menu Navigation

You will now see the main menu.

Select:

```
7. Evil Twin attacks menu
```

---

# Step 4 – Enable Monitor Mode

Inside the Evil Twin menu:

```
Put interface in monitor mode
```

This will create a monitor interface (e.g., wlan1mon).

**Why this is required:**

* Enables packet sniffing
* Allows injection of deauthentication frames

---

# Step 5 – Scan for Target Networks

Select:

```
Explore for targets (monitor mode needed)
```

Airgeddon will begin scanning nearby networks.

Once the scan completes:

* Identify the target network
* Note:

  * SSID
  * BSSID
  * Channel

Select the correct target network.

---

# Step 6 – Launch Evil Twin Attack

Select:

```
Evil Twin AP attack with captive portal
```

---

# Step 7 – Select Deauthentication Method

Options:

```
1. Deauth / disassoc mdk4 attack
2. Deauth aireplay-ng attack
3. Auth DoS attack
```

## Explanation of Methods

### Deauth / disassoc mdk4 attack

* High-volume packet injection
* Targets multiple clients simultaneously
* More aggressive and faster
* Higher detectability

### Deauth aireplay-ng attack (Recommended)

* Stable and widely supported
* Precise targeting
* Lower noise level
* Reliable in most environments

### Auth DoS attack

* Floods AP with authentication requests
* Prevents legitimate connections
* Does not rely on deauth frames
* Useful if deauth is blocked (e.g., 802.11w)

---

# Step 8 – DoS Pursuit Mode

Enable when prompted.

**Function:**

* Continuously sends deauthentication traffic
* Prevents victim from reconnecting to real AP

---

# Step 9 – MAC Address Spoofing

Enable when prompted.

**Function:**

* Clones BSSID of target AP
* Increases likelihood of client trust

---

# Step 10 – Handshake Handling

Choose:

* Use existing handshake
* Capture a new handshake

If capturing:

* Set timeout (recommended: 30–60 seconds)

**Purpose:**

* Verify Wi-Fi password entered in captive portal
* Enable offline cracking as backup

---

# Step 11 – Captive Portal Selection

Choose:

* Default captive portal
* Custom captive portal

**Recommended:**
Wi-Fi password verification portal

---

# Step 12 – Save Script

Choose default or specify path.

---

# Step 13 – Execute Attack

Airgeddon will:

1. Launch deauthentication
2. Start fake AP
3. Force client reconnection
4. Display captive portal
5. Capture credentials

---

# Step 14 – Monitoring

Observe:

* Deauthentication activity
* Client reconnections
* Captive portal requests
* Captured credentials

---

# MITRE ATT&CK Mapping

| Technique ID | Name                          | Description                                            |
| ------------ | ----------------------------- | ------------------------------------------------------ |
| T1557        | Adversary-in-the-Middle       | Evil Twin places attacker between victim and network   |
| T1056        | Input Capture                 | Captive portal captures user credentials               |
| T1556        | Modify Authentication Process | Fake portal mimics authentication to steal credentials |
| T1110        | Credential Access             | Credentials obtained via portal                        |
| T1046        | Network Service Discovery     | Scanning for wireless targets                          |

---

# Attack Lifecycle Mapping

| Stage             | Description           |
| ----------------- | --------------------- |
| Reconnaissance    | Network scanning      |
| Initial Access    | Evil Twin AP          |
| Execution         | Deauthentication      |
| MitM              | Traffic interception  |
| Credential Access | Captive portal        |
| Persistence       | DoS pursuit mode      |
| Impact            | Credential compromise |

---

# Detection

Indicators of attack:

* Multiple APs with identical SSIDs
* High volume of deauthentication frames
* Sudden client disconnections
* Rogue DHCP/DNS behavior
* Unexpected captive portal redirects
* Certificate warnings in browser

---

# Prevention

**WPA3 or WPA2-Enterprise (802.1X)**
Provides strong authentication, making it difficult to replicate legitimate networks and perform Evil Twin attacks.

**Protected Management Frames (802.11w)**
Prevents deauthentication attacks by protecting management frames from being spoofed.

**Wireless IDS / WIPS**
Detects rogue access points and suspicious behavior such as duplicate SSIDs or deauth activity.

**Certificate Validation Enforcement**
Helps prevent phishing by ensuring users only trust legitimate servers and certificates.

**User Awareness Training**
Reduces risk by teaching users to recognize fake networks and suspicious login pages.


---

# Analysis

This attack is effective due to:

* Automatic Wi-Fi reconnection behavior
* Trust in known SSIDs
* User familiarity with captive portals

However, HTTPS, HSTS, and certificate validation significantly reduce the ability to intercept sensitive data.

---

# Demonstration Plan

1. Scan for nearby networks
2. Select target AP
3. Launch Evil Twin attack
4. Show deauthentication effect
5. Connect victim device
6. Display captive portal
7. Capture credentials

---

# Student Questions

1. Why is monitor mode required in this attack?
2. What is the purpose of deauthentication?
3. What is the difference between mdk4 and aireplay-ng?
4. When would an attacker use Auth DoS instead of deauth?
5. Why is MAC spoofing important?
6. What role does the handshake play?

---

# Answers

1. Monitor mode allows packet capture and injection.
2. Deauthentication forces clients to reconnect to the attacker’s AP.
3. mdk4 is more aggressive and targets multiple clients, while aireplay-ng is more stable and precise.
4. Auth DoS is used when deauthentication is blocked or ineffective.
5. MAC spoofing increases the likelihood of client trust and connection.
6. The handshake allows verification or offline cracking of the Wi-Fi password.

---

# Disclaimer

This lab is intended strictly for educational purposes in controlled environments using authorized devices only.
