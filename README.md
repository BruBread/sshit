# SSHIT!

**"Just get an HDMI bro"**

![Repo size](https://img.shields.io/github/repo-size/BruBread/sshit)
![Stars](https://img.shields.io/github/stars/BruBread/sshit?style=social)
![Last commit](https://img.shields.io/github/last-commit/BruBread/sshit)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## What This Is

Turns your Raspberry Pi into its own Wi-Fi access point so you can SSH into it directly.

---

## How It Works

After install, the Pi creates an open Wi-Fi network named after your username (e.g. `pi-john`).

Connect → SSH → done.

```
Laptop → connects to "pi-john" → ssh john@10.0.0.1
```

* Use `10.0.0.1` or `username@hostname.local`
* If the Pi connects to another network (`piwifi`, `piconnect`), the AP shuts off
* If that connection drops, the AP comes back automatically

---

## First-Time Setup

Only needed once.

### 1 — Flash Pi OS Lite

Download and flash:
https://www.raspberrypi.com/software/

Path:

> Raspberry Pi 4 → Other OS → Debian 64 Lite

Set:

* **Hostname:** `francispi` (keep it consistent)
* **Username:** `francis` (same)
* **Password**
* Your Wi-Fi SSID + password

> Using the same hostname + username avoids SSH key conflicts later.

---

### 2 — Boot

Insert storage, power on, wait ~60 seconds.

---

### 3 — SSH In

```bash
ssh francis@francispi.local
```

If it fails (usually Windows), get the IP from your router:

```bash
ssh francis@<ip>
```

---

### 4 — Install

```bash
sudo apt install git
git clone https://github.com/BruBread/sshit.git
cd sshit
sudo bash install.sh
```

This installs:

* `hostapd` + `dnsmasq`
* Wi-Fi AP (`pi-<username>`)
* `pi*` commands
* auto-start + recovery

Reboot after:

```bash
sudo reboot
```

---

### 5 — Connect

Join:

```
pi-francis
```

Then:

```bash
ssh francis@10.0.0.1
```

---

## Commands

| Command                      | Description        |
| ---------------------------- | ------------------ |
| `pihelp`                     | List commands      |
| `pistatus`                   | Mode, IPs, clients |
| `sudo piap`                  | Force AP mode      |
| `sudo pilock [password]`     | Add AP password    |
| `sudo piunlock`              | Remove password    |
| `sudo piwifi`                | Scan + connect     |
| `sudo piconnect <ssid> [pw]` | Direct connect     |
| `pisaved`                    | Saved networks     |
| `piadd [ssid] [password]`    | Add network        |
| `sudo piupdate`              | Update             |
| `pirestart`                  | Reboot             |

### Examples

```bash
pistatus

sudo pilock mypass

sudo piwifi

sudo piap

sudo piupdate

pisaved

piadd myssid mypassword

pirestart
```

---

## Supported Hardware

* Raspberry Pi 4
* Pi OS Lite (Bookworm, 64-bit)
* USB or SD boot

---

## Troubleshooting

### SSH key warning

```bash
ssh-keygen -R francispi.local
```

Or:

```bash
rm ~/.ssh/known_hosts
```

Reconnect and accept the new key.

---

### AP not showing

```bash
sudo piap
pistatus
```

---

### hostapd fails

```bash
sudo journalctl -u hostapd --no-pager -n 30
```

Usually `wlan0` is being held by something else. Reboot if needed.

---

### Can't SSH on AP

Use:

```bash
ssh francis@10.0.0.1
```

---

### AP didn’t come back

```bash
sudo piap
```

Watcher will also bring it back automatically.

---

## Project Structure

```
sshit/
├── install.sh
├── picommands.sh
├── netwatcher.sh
└── README.md
```

---

## License

MIT
