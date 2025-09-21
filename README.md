# 🛡️ Cowrie SSH Honeypot Lab Project

## Project Overview
This project demonstrates the deployment of a **honeypot using Cowrie** to monitor and log malicious SSH attacks. The honeypot simulates a real Linux SSH server, captures brute-force attempts, attacker commands, and provides logs for analysis.

**Skills Demonstrated:**
- Linux system setup
- Cowrie honeypot installation & configuration
- SOC monitoring & log analysis
- Brute-force attack simulation
- Data collection & visualization

## Lab Architecture

```
+----------------+          +----------------+
|                |          |                |
|   Attacker VM  |  ---->   |  Honeypot VM   |
|  (Kali Linux)  |  SSH     |  (Ubuntu +     |
|                |          |   Cowrie)      |
+----------------+          +----------------+
```

- **Honeypot VM IP:** 192.168.56.101  
- **Attacker VM IP:** 192.168.56.102  
- **Network Type:** Host-Only / Internal Network  
- **Purpose:** Safe simulation of attacks without internet exposure

## Tools Used
| Tool | Purpose |
|------|---------|
| Cowrie Honeypot | Simulates SSH/Telnet server, captures attacks |
| Kali Linux | Simulated attacker machine for penetration testing |
| Hydra | Performs SSH brute-force attacks |
| Nmap | Scans open ports on the honeypot |
| Python3 | Required to run Cowrie |
| VirtualBox / VMware | Virtual environment to safely host VMs |

## Setup Steps

### 1. Prepare the Environment
- Installed Ubuntu/Debian on Honeypot VM  
- Installed Kali Linux on Attacker VM  
- Configured Host-Only network for isolated communication

### 2. Install Cowrie on Honeypot
```bash
sudo adduser --disabled-password cowrie
git clone https://github.com/cowrie/cowrie.git
cd cowrie
python3 -m venv cowrie-env
source cowrie-env/bin/activate
pip install -r requirements.txt
cp etc/cowrie.cfg.dist etc/cowrie.cfg
bin/cowrie start
```

### 3. Simulate Attacks from Kali
- Port scanning: `nmap -sV 192.168.56.101`  
- Brute-force SSH:  
```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.101
```
- Commands executed in fake shell are recorded by Cowrie.

## Logs and Analysis

**Captured Data:**
- Attacker IP addresses
- Usernames/password attempts
- Commands executed (e.g., `ls`, `cat /etc/passwd`)
- Timestamped session recordings

**Example Log Snippet:**
```
2025-09-21 14:22:10+0530 [SSHService ssh-connection,1845,192.168.56.102] login attempt [username: root, password: 123456]
2025-09-21 14:22:15+0530 [SSHService ssh-connection,1845,192.168.56.102] command: ls
2025-09-21 14:22:20+0530 [SSHService ssh-connection,1845,192.168.56.102] command: cat /etc/passwd
```

**Observations:**
- Brute-force attacks occurred within seconds of starting simulation.  
- Cowrie successfully captured all commands executed in fake SSH environment.  
- Honeypot provides safe environment to study attacker behavior.

## Project Deliverables
1. VM Setup Scripts / Instructions  
2. Cowrie Configuration Files  
3. Captured Logs  
4. Analysis Report  
5. Optional Dashboard (Splunk/ELK)

## Learning Outcomes
- Understanding of SSH brute-force attacks  
- Hands-on experience with honeypot deployment and log management  
- Skills in setting up safe lab environments for cybersecurity analysis  
- Insights into attacker behavior and threat monitoring techniques

## Conclusion
This project demonstrates the deployment of a honeypot for **practical SOC learning**. It provides a controlled environment to study malicious activity, monitor attack patterns, and improve incident response skills.

## GitHub Repository Structure Suggestion
```
cowrie-honeypot-lab/
│
├─ README.md             # This project report
├─ cowrie-config/        # Configuration files
├─ logs/                 # Sample captured attack logs
├─ scripts/              # VM setup or helper scripts
├─ screenshots/          # Screenshots of Cowrie logs & dashboards
└─ analysis/             # Log analysis & observations
```

