# 🛡️ SIEM Solution Deployment & Analysis
### `Advanced Network Security — Assignment No. 03`

---

```
 ██████╗ ██╗███████╗███╗   ███╗
██╔════╝ ██║██╔════╝████╗ ████║    Wazuh · MITRE ATT&CK · Threat Detection Lab
╚█████╗  ██║█████╗  ██╔████╔██║    ══════════════════════════════════════════
 ╚═══██╗ ██║██╔══╝  ██║╚██╔╝██║    Student   :  Sarmad Farooq  (25I-7722)
██████╔╝ ██║███████╗██║ ╚═╝ ██║    Program   :  MS Cybersecurity
╚═════╝  ╚═╝╚══════╝╚═╝     ╚═╝    Course    :  Advanced Network Security
                                   Instructor :  Dr. Zafar Iqbal
  D E P L O Y M E N T             Institute  :  NUCES Islamabad
```

---

## 📌 Table of Contents

- [🔭 Project Overview](#-project-overview)
- [🏗️ Architecture Diagram](#️-architecture-diagram)
- [⚙️ Phase 1 — Wazuh Installation](#️-phase-1--wazuh-installation)
- [🌐 Phase 2 — Environment Setup](#-phase-2--environment-setup)
- [📋 Phase 3 — Log Collection](#-phase-3--log-collection)
- [⚡ Phase 4 — Rule & Alert Creation](#-phase-4--rule--alert-creation)
- [📊 Phase 5 — Event Monitoring & Analysis](#-phase-5--event-monitoring--analysis)
- [📈 Results Summary](#-results-summary)
- [🛠️ Tech Stack](#️-tech-stack)
- [🚀 Getting Started](#-getting-started)
- [📚 References](#-references)

---

## 🔭 Project Overview

> **SIEM (Security Information and Event Management)** is a platform that collects, analyzes, and correlates security events from multiple sources across an IT environment.

This assignment demonstrates a **complete end-to-end SIEM deployment** using **Wazuh** — a free, open-source security monitoring platform — in a multi-VM lab environment.

### ✅ What was accomplished

- 🔧 Automated all-in-one Wazuh installation (Manager + Indexer + Dashboard)
- 🐧 Multi-platform agent deployment: **Ubuntu Clone** + **Windows 10 FlareVM**
- 📡 Real-time log collection and encrypted event streaming
- 🎯 Simulated adversarial attacks using **Atomic Red Team**
- 🗺️ Alert mapping to **MITRE ATT&CK** techniques (T1003.008)
- 📊 Full dashboard-level analysis of detected threats

---

## 🏗️ Architecture Diagram

```
╔══════════════════════════════════════════════════════════════════╗
║                      LAB ENVIRONMENT                            ║
║                                                                  ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │            🖥️  WAZUH MANAGER NODE                       │      ║
║  │            IP: 192.168.134.129                          │      ║
║  │                                                         │      ║
║  │  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐   │      ║
║  │  │Wazuh Manager│  │Wazuh Indexer │  │  Dashboard   │   │      ║
║  │  │ (Detection) │  │  (Storage)   │  │  (Web UI)    │   │      ║
║  │  └─────────────┘  └──────────────┘  └──────────────┘   │      ║
║  └──────────────────────────┬───────────────────────────────┘     ║
║                             │                                     ║
║               Encrypted Channel  (Port 1514)                     ║
║                   ┌─────────┴──────────┐                         ║
║                   │                    │                         ║
║  ┌────────────────▼──┐   ┌─────────────▼──────────┐             ║
║  │  🐧 AGENT 1        │   │  🪟 AGENT 2             │            ║
║  │  Ubuntu (Clone)   │   │  Windows 10 (FlareVM)  │            ║
║  │                   │   │                         │            ║
║  │ /var/log/auth.log │   │  Windows Event Log      │            ║
║  │ /var/log/syslog   │   │  Security / System      │            ║
║  └───────────────────┘   └─────────────────────────┘            ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## ⚙️ Phase 1 — Wazuh Installation

<details>
<summary><b>▶ Click to expand — Installation Steps</b></summary>

<br/>

### 1.1 — Prepare Ubuntu Virtual Machine
Boot the Ubuntu VM in **VirtualBox** or **VMware**. This will host Wazuh Manager, Indexer, and Dashboard on a single node.

### 1.2 — Navigate to Wazuh Website
```
https://wazuh.com  →  Click "Install Wazuh"  →  Select "Quickstart"
```

### 1.3 — Run the All-in-One Installation Script

Open a terminal and run:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && \
sudo bash ./wazuh-install.sh --a
```

> 💡 `--a` = all-in-one mode. Installs and configures all 3 components automatically.  
> ⏳ Takes several minutes. Watch the progress in the terminal output.

</details>

---

## 🌐 Phase 2 — Environment Setup

<details>
<summary><b>▶ Click to expand — Environment Setup</b></summary>

<br/>

### 2.1 — Identify Server IP

```bash
ip a
# Look for your active interface → e.g., 192.168.134.129
```

### 2.2 — Access the Dashboard

```
Browser → https://192.168.134.129
Accept self-signed SSL certificate → Advanced → Accept Risk & Continue
```

### 2.3 — Login Credentials

```
Username : admin
Password : <auto-generated during install — shown in terminal>
```

### 2.4 — Deploy Agent on Cloned Ubuntu VM

**Clone the VM in VirtualBox**, then on the clone:

```bash
# Remove server components from clone
sudo bash wazuh-install.sh --u

# Install Wazuh Agent (replace IP with your Manager IP)
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.4-1_amd64.deb && \
sudo WAZUH_MANAGER='192.168.134.129' \
     WAZUH_AGENT_NAME='linux' \
     dpkg -i ./wazuh-agent_4.14.4-1_amd64.deb

# Start the agent
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### 2.5 — Deploy Agent on Windows 10 (FlareVM)

Run in **PowerShell (Admin)**:

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.4-1.msi `
  -OutFile $env:tmp\wazuh-agent

msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.134.129'

NET START Wazuh
```

### 2.6 — Restart Manager Services

```bash
sudo systemctl start wazuh-manager
sudo systemctl start wazuh-indexer
sudo systemctl start wazuh-dashboard
```

> ✅ Both agents (Linux + Windows) should appear as **Active** in the Dashboard → Agents section.

</details>

---

## 📋 Phase 3 — Log Collection

<details>
<summary><b>▶ Click to expand — Log Collection</b></summary>

<br/>

### How the Log Pipeline Works

```
  Endpoint (Agent)
       │
       ├── Reads: /var/log/auth.log      (Linux)
       ├── Reads: /var/log/syslog        (Linux)
       └── Reads: Windows Event Log      (Windows)
       │
       │  🔒 Encrypted over Port 1514
       ▼
  Wazuh Manager
       │
       ├── Decodes & parses log entries
       ├── Applies detection rules
       └── Indexes via Wazuh Indexer
       │
       ▼
  Wazuh Dashboard
       └── Security Events view → real-time alerts
```

### Configuring Additional Log Sources

Edit the config file on the Manager VM:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

You can add:
- Apache / Nginx web server logs
- Firewall logs (iptables, pf)
- Database audit logs

Then restart to apply:

```bash
sudo systemctl restart wazuh-manager
```

</details>

---

## ⚡ Phase 4 — Rule & Alert Creation

<details>
<summary><b>▶ Click to expand — Atomic Red Team Attack Simulation</b></summary>

<br/>

### Wazuh Rule Structure

| Property | Description |
|:---|:---|
| **Rule ID** | Unique numeric identifier |
| **Severity (0–15)** | 15 = Most Critical |
| **Description** | What the rule detects |
| **Conditions** | Log patterns that trigger an alert |

### Installing Invoke-AtomicRedTeam on Linux

```bash
# Step 1: Install PowerShell on Ubuntu
sudo snap install powershell --classic

# Step 2: Launch PowerShell
pwsh
```

```powershell
# Step 3: Inside PowerShell — install the module
Install-Module -Name invoke-atomicredteam, powershell-yaml -Scope CurrentUser -Force
Import-Module invoke-atomicredteam

# Step 4: Download atomic test definitions
Invoke-AtomicTest ALL -GetPrereqs
```

### Executing MITRE ATT&CK Technique T1003.008

> 🎯 **T1003.008** — OS Credential Dumping: `/etc/passwd` and `/etc/shadow`

```powershell
# Run inside PowerShell on the Linux agent VM
Invoke-AtomicTest T1003.008
```

This simulates real credential dumping behavior and triggers Wazuh's built-in detection rules.

</details>

---

## 📊 Phase 5 — Event Monitoring & Analysis

<details>
<summary><b>▶ Click to expand — Monitoring & Analysis</b></summary>

<br/>

### Dashboard — Security Events Fields

| Field | Description |
|:---|:---|
| **Timestamp** | When the event was detected |
| **Agent** | Which machine generated the event |
| **Rule ID & Description** | Type of activity detected |
| **Severity Level** | Criticality of the alert |
| **MITRE Technique ID** | Mapped ATT&CK technique |

### Failed Login Detection

```
🔴 Rule triggered:  "Authentication Failure"
📍 Source agent:    linux (Ubuntu Clone)
🗺️  ATT&CK mapping:  T1110 — Brute Force
⚡ Action:          Alert generated in Security Events dashboard
```

Wazuh automatically flags repeated failed login attempts using its built-in auth failure rules — no custom rule needed.

</details>

---

## 📈 Results Summary

| 🔬 Capability | ✅ Result |
|:---|:---|
| **Log Collection** | Successfully collected from Linux and Windows agents |
| **Agent Deployment** | Deployed on Ubuntu Clone + Windows 10 FlareVM |
| **Threat Detection** | Detected MITRE ATT&CK technique T1003.008 |
| **Alert Generation** | Alerts visible in Security Events dashboard |
| **Failed Login Detection** | Multiple failed authentication attempts detected |
| **MITRE Mapping** | Events correctly mapped to ATT&CK techniques |

### 🎯 Conclusion

> This lab successfully demonstrated the **complete SIEM lifecycle** using Wazuh — from installation and multi-platform agent deployment, through real-time log collection and rule-based threat detection, to MITRE ATT&CK-mapped alert analysis.
>
> Wazuh's integration with the MITRE ATT&CK framework enables analysts to understand not just *what* happened, but *which phase of an attack* it represents — making it an effective SIEM solution for labs, SMEs, and enterprise environments alike.

---

## 🛠️ Tech Stack

| Tool | Role |
|:---|:---|
| **Wazuh 4.14** | SIEM Platform (Manager + Indexer + Dashboard) |
| **Ubuntu 22.04 VM** | Manager node + Linux agent endpoint |
| **Windows 10 FlareVM** | Windows agent endpoint |
| **VirtualBox / VMware** | Hypervisor for lab isolation |
| **PowerShell + Invoke-AtomicRedTeam** | Adversary simulation framework |
| **MITRE ATT&CK** | Threat intelligence mapping framework |

---

## 🚀 Getting Started

```bash
# 1. Clone this repository
git clone https://github.com/<your-username>/SIEM-Wazuh-Deployment.git
cd SIEM-Wazuh-Deployment

# 2. Read the full report
open report/SIEM_Report_25i-7722.docx

# 3. Follow deployment phases in the README above
#    Official docs: https://documentation.wazuh.com/current/quickstart.html
```

**Prerequisites:**
- VirtualBox or VMware Workstation
- Ubuntu 22.04+ VM — minimum 4 GB RAM, 2 vCPUs, 50 GB disk
- Windows 10 VM (FlareVM — optional for Windows agent testing)
- Internet access inside VMs

---

## 📚 References

| Resource | Link |
|:---|:---|
| 📖 Wazuh Documentation | https://documentation.wazuh.com |
| ⚔️ Atomic Red Team | https://atomicredteam.io |
| 🗺️ MITRE ATT&CK Framework | https://attack.mitre.org |
| 🔧 Invoke-AtomicRedTeam | https://github.com/redcanaryco/invoke-atomicredteam |

---

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🛡️  Sarmad Farooq  ·  25I-7722  ·  MS Cybersecurity  ·  NUCES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
