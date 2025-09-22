# 🛡️ Single VM Cowrie Honeypot Lab Project

## Project Overview

This project demonstrates the deployment of a **Cowrie SSH honeypot** on a single Kali Linux VM. The honeypot captures SSH brute-force attempts and attacker commands for SOC monitoring and analysis.

**Skills Demonstrated:**

* Linux system setup
* Cowrie honeypot installation & configuration
* SSH attack simulation
* Log analysis and monitoring
* SOC fundamentals

---

## Lab Architecture

```
+----------------+          +----------------+
|                |          |                |
|  Kali Linux VM |   ---->  |  Cowrie SSH HP  |
| (Attacker +    |  SSH     |  (localhost)   |
|  Honeypot)     |          |                |
+----------------+          +----------------+
```

* Both attacker and honeypot run on the **same VM**.
* Cowrie listens on **localhost:2222** for SSH connections.

---

## Tools Used

| Tool            | Purpose                             |
| --------------- | ----------------------------------- |
| Cowrie Honeypot | Simulate SSH server and log attacks |
| Kali Linux      | Attack simulation & lab environment |
| Hydra           | Brute-force SSH login attempts      |
| Nmap            | Port scanning                       |
| Python3         | Run Cowrie                          |

---

## Setup Steps

### 1. Update Kali Linux

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Dependencies

```bash
sudo apt install -y git python3 python3-venv python3-pip virtualenv authbind
```

### 3. Set Up Cowrie

```bash
sudo adduser --disabled-password cowrie
sudo su - cowrie
git clone https://github.com/cowrie/cowrie.git
cd cowrie
python3 -m venv cowrie-env
source cowrie-env/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
cp etc/cowrie.cfg.dist etc/cowrie.cfg
```

* Edit `etc/cowrie.cfg` if needed (e.g., `listen_port = 2222`).
![Editing port number](screenshots/ssh%20config%20for%20same%20device%20using%201VM.png)

### 4. Start Cowrie

```bash
bin/cowrie start
```
![Cowrie START](screenshots/Cowrie%20start.png)

* Logs are stored in `var/log/cowrie/cowrie.log`

### 5. Simulate Attacks Locally

```bash
ssh root@127.0.0.1 -p 2222
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1:2222
```
![Cowrie Attack](screenshots/Attack%201.png)
* Execute commands like `ls` and `whoami` to generate logs.

---

## Logs and Analysis

* **Log file:** `var/log/cowrie/cowrie.log`
* **Captured data:**

  * Login attempts
  * Commands executed
  * Session timestamps
  * 
![Cowrie SSH Log](screenshots/Cowrie%20Logs.png)

**Example Log Entry:**

```
2025-09-21 14:22:10+0530 [SSHService ssh-connection,1845,127.0.0.1] login attempt [username: root, password: 123456]
2025-09-21 14:22:15+0530 [SSHService ssh-connection,1845,127.0.0.1] command: ls
```

* Logs help understand attacker behavior and SOC monitoring processes.

---

## Optional Enhancements

* Forward logs to **Splunk** or **ELK** for visualization.
* Add fake files and banners for realism.
* Simulate more attack types with custom scripts.

---

## Project Deliverables

* `README.md` (this file)
* Screenshots of log entries or dashboards
* Optional Splunk/ELK dashboards

---

## Learning Outcomes

* Understand SSH brute-force attacks
* Hands-on honeypot deployment and monitoring
* Log analysis and attacker behavior observation
* Practical SOC monitoring experience

---

## Conclusion

This single-VM lab allows safe simulation of cyber attacks and monitoring with Cowrie. It demonstrates SOC-relevant skills and can be used as a **portfolio project** for cybersecurity roles.

---

## GitHub Repository Structure Suggestion

```
cowrie-single-vm/
│
├─ README.md             # Project documentation
├─ screenshots/          # Screenshots of logs or terminal
```
