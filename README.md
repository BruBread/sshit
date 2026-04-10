
# SSHIT!

**"Just get an hdmi bro"**

This allows the Pi to broadcast an AP that YOU can connect and finally SSH into!


---

How It Works
Once installed, your Pi broadcasts an open Wi-Fi network named after your username (e.g. pi-john). Connect your laptop to that network and SSH to 10.0.0.1. The Pi's AP stays on permanently — even after reboots — alongside your normal home Wi-Fi connection.
Laptop → connects to "pi-john" Wi-Fi → ssh john@10.0.0.1 → you're in

First-Time Setup (One-Time Steps)
Before installing PiAccess, you need to SSH in just once the old way. Here's how.
Step 1 — Flash Pi OS Lite
Download and flash Raspberry Pi OS Lite (64-bit) using Raspberry Pi Imager.
In the Imager, click the gear icon ⚙️ and set:

Hostname: raspberrypi
Username and password (remember these)
Your home Wi-Fi SSID and password

This pre-configures SSH and Wi-Fi so you can connect on first boot.
Step 2 — Boot the Pi
Insert the SD card, power on the Pi, and wait ~60 seconds.
Step 3 — Find the Pi on Your Network (One Time Only)
Try these in order until one works:
bashssh yourname@raspberrypi.local      # Works on macOS/Linux, unreliable on Windows
If that fails, log into your router and find the Pi's IP in the device list, then:
bashssh yourname@<pi-ip-address>
Step 4 — Install PiAccess
Once you're SSHed in:
bashcurl -sSL https://raw.githubusercontent.com/BruBread/piaccess/main/install.sh | sudo bash
That's it. The installer:

Installs hostapd and dnsmasq
Starts a Wi-Fi AP named pi-<yourusername>
Adds pi* commands to your shell
Enables the AP to auto-start on every boot

Step 5 — Connect the Easy Way, Forever
From now on, never hunt for an IP again:

On your laptop, connect to Wi-Fi: pi-yourusername (no password by default)
SSH in: ssh yourusername@10.0.0.1


Commands
CommandDescriptionpihelpShow all commandspistatusAP status, IP addresses, connected clientspiapRestart the access pointpilock [password]Add a password to the APpiunlockRemove AP password (go back to open)piwifiScan and connect to a Wi-Fi network interactivelypiconnect <ssid> [pw]Connect to a specific Wi-Fi networkpiupdatePull the latest version from GitHub
Examples
bash# Check everything at a glance
pistatus

# Lock the AP with a password
sudo pilock mysecretpass

# Or just run pilock and it will prompt you
sudo pilock

# Connect to a new home Wi-Fi network
piwifi

# Update PiAccess to the latest version
piupdate

Supported Hardware

Raspberry Pi 4 (all RAM variants)
Pi OS Lite (Bookworm, 64-bit recommended)


Troubleshooting
The AP isn't showing up
bashsudo piap          # restart the AP
pistatus           # check what's happening
hostapd fails to start
bashsudo journalctl -u hostapd --no-pager -n 30
Most common cause: another process (NetworkManager, wpa_supplicant) is holding wlan0. piap handles this automatically but a reboot fixes persistent issues.
I can't SSH after connecting to the AP
Make sure you're SSHing to 10.0.0.1, not the Pi's hostname:
bashssh yourname@10.0.0.1
I want to re-run the installer
The installer is idempotent — safe to run multiple times:
bashcurl -sSL https://raw.githubusercontent.com/BruBread/piaccess/main/install.sh | sudo bash

Project Structure
piaccess/
├── install.sh        # One-line installer, entry point
├── picommands.sh     # All pi* bash functions (sourced by .bashrc)
└── README.md

License
MIT
