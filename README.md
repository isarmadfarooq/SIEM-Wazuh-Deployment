<div align="center">

<!-- Animated Header Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a1a2e,100:16213e&height=200&section=header&text=SIEM%20Solution%20Deployment&fontSize=40&fontColor=00d4ff&animation=fadeIn&fontAlignY=38&desc=Wazuh%20%7C%20MITRE%20ATT%26CK%20%7C%20Threat%20Detection%20Lab&descAlignY=60&descColor=7fdbff" width="100%"/>

<br/>

<!-- Animated Typing -->
<a href="https://git.io/typing-svg">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&duration=3000&pause=800&color=00D4FF&center=true&vCenter=true&multiline=true&repeat=true&width=700&height=80&lines=🛡️+Advanced+Network+Security+Assignment+%233;🔍+Wazuh+SIEM+%7C+Real-Time+Threat+Detection;⚔️+MITRE+ATT%26CK+%7C+Atomic+Red+Team+Testing" alt="Typing SVG" />
</a>

<br/><br/>

<!-- Badges -->
![Wazuh](https://img.shields.io/badge/Wazuh-4.14-00d4ff?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyTDIgN2wxMCA1IDEwLTV6TTIgMTdsOCA0IDgtNE0yIDEybDggNCA4LTQiLz48L3N2Zz4=&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-Integrated-red?style=for-the-badge&logo=target&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-VM-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-FlareVM-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-AtomicRedTeam-5391FE?style=for-the-badge&logo=powershell&logoColor=white)
![License](https://img.shields.io/badge/Academic-NUCES%20Islamabad-green?style=for-the-badge&logo=graduation-cap&logoColor=white)

<br/>

<!-- Student Info Card -->
| 🎓 Field | 📋 Details |
|:---:|:---:|
| **Student** | Sarmad Farooq |
| **Student ID** | 25I-7722 |
| **Program** | MS Cybersecurity |
| **Course** | Advanced Network Security |
| **Instructor** | Dr. Zafar Iqbal |
| **Institution** | NUCES (Islamabad) |

</div>

---

## 📌 Table of Contents

<details open>
<summary><b>Click to expand / collapse</b></summary>

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

</details>

---

## 🔭 Project Overview

<div align="center">
<img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square"/>
<img src="https://img.shields.io/badge/Type-Academic%20Lab-blue?style=flat-square"/>
<img src="https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-orange?style=flat-square"/>
</div>

<br/>

> **SIEM (Security Information and Event Management)** is a platform that collects, analyzes, and correlates security events from multiple sources across an IT environment.

This assignment demonstrates a **full end-to-end SIEM deployment** using **Wazuh** — a free, open-source security monitoring platform — in a multi-VM lab environment. It covers:

- ✅ Automated installation of all Wazuh components on a single node
- ✅ Multi-platform agent deployment (Linux Ubuntu + Windows 10 FlareVM)
- ✅ Real-time log collection and event streaming
- ✅ Simulated adversarial attacks using **Atomic Red Team**
- ✅ Alert mapping to **MITRE ATT&CK** techniques
- ✅ Dashboard analysis of detected threats

---

## 🏗️ Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        LAB ENVIRONMENT                          │
│                                                                 │
│  ┌──────────────────────────────┐                               │
│  │    🖥️  WAZUH MANAGER NODE     │   IP: 192.168.134.129        │
│  │  ┌────────────────────────┐  │                               │
│  │  │   Wazuh Manager        │  │   Receives all agent data     │
│  │  │   Wazuh Indexer        │  │   Runs correlation rules      │
│  │  │   Wazuh Dashboard      │  │   Hosts web UI (HTTPS:443)    │
│  │  └────────────────────────┘  │                               │
│  └──────────────┬───────────────┘                               │
│                 │  Encrypted Channel (Port 1514)                │
│        ┌────────┴────────┐                                      │
│        │                 │                                      │
│  ┌─────▼──────┐   ┌──────▼──────┐                              │
│  │ 🐧 AGENT 1  │   │ 🪟 AGENT 2   │                             │
│  │ Ubuntu VM  │   │ Windows 10  │                              │
│  │  (Clone)   │   │  (FlareVM)  │                              │
│  └────────────┘   └─────────────┘                              │
│                                                                 │
│  📡 Log Sources: /var/log/auth.log · syslog · Windows EventLog  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ Phase 1 — Wazuh Installation

<details>
<summary><b>🔽 Expand Installation Steps</b></summary>

### Step 1.1 — Prepare Ubuntu Virtual Machine
Boot the Ubuntu VM in **VirtualBox** or **VMware**. This VM will host all three Wazuh components in a single-node "all-in-one" setup.

### Step 1.2 — Navigate to Official Wazuh Website
Open a browser inside the VM and go to:
```
https://wazuh.com
```

### Step 1.3 — Select the Quickstart Installation Method
From the documentation page, choose **Quickstart** — this installs Wazuh Manager, Indexer, and Dashboard using a single automated script.

### Step 1.4 — Run the Installation Command

Open a terminal and execute:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && \
sudo bash ./wazuh-install.sh --a
```

> 💡 The `--a` flag triggers an **all-in-one installation**. Wazuh Manager, Indexer, and Dashboard are installed and configured automatically.

> ⏳ Installation takes several minutes. Progress messages are shown in the terminal.

</details>

---

## 🌐 Phase 2 — Environment Setup

<details>
<summary><b>🔽 Expand Environment Setup</b></summary>

### Step 2.1 — Get the Wazuh Server IP
```bash
ip a
# Example output: 192.168.134.129
```

### Step 2.2 — Access the Dashboard
Navigate in your browser to:
```
https://192.168.134.129
```
> ⚠️ Accept the self-signed SSL certificate warning in your browser.

### Step 2.3 — Login
```
Username: admin
Password: <generated during installation — shown in terminal output>
```

### Step 2.4 — Deploy Agent on Cloned Ubuntu VM

Clone the existing Ubuntu VM in VirtualBox and uninstall the server components so it functions only as an agent endpoint:

```bash
# Uninstall manager from clone
sudo bash wazuh-install.sh --u

# Download and install the Wazuh Agent
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.4-1_amd64.deb && \
sudo WAZUH_MANAGER='192.168.134.129' \
     WAZUH_AGENT_NAME='linux' \
     dpkg -i ./wazuh-agent_4.14.4-1_amd64.deb

# Enable and start agent service
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### Step 2.5 — Deploy Agent on Windows 10 (FlareVM)

Run in **PowerShell (Admin)**:
```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.4-1.msi `
  -OutFile $env:tmp\wazuh-agent

msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.134.129'

NET START Wazuh
```

### Step 2.6 — Restart Wazuh Services (on Manager)
```bash
sudo systemctl start wazuh-manager
sudo systemctl start wazuh-indexer
sudo systemctl start wazuh-dashboard
```

</details>

---

## 📋 Phase 3 — Log Collection

<details>
<summary><b>🔽 Expand Log Collection Details</b></summary>

### How It Works

```
Endpoint (Agent)
   │
   ├─ Reads: /var/log/auth.log
   ├─ Reads: /var/log/syslog
   └─ Reads: Windows Event Log
   │
   ▼  Encrypted (Port 1514)
Wazuh Manager
   │
   ├─ Processes & correlates logs
   └─ Indexes via Wazuh Indexer
   │
   ▼
Wazuh Dashboard
   └─ Displays Security Events & Alerts
```

### Configuring Additional Log Sources

Edit the main config file on the Manager:
```bash
sudo nano /var/ossec/etc/ossec.conf
```

Add custom sources such as:
- Apache / Nginx web server logs
- Firewall logs
- Database audit logs

After editing, restart the Manager:
```bash
sudo systemctl restart wazuh-manager
```

</details>

---

## ⚡ Phase 4 — Rule & Alert Creation

<details>
<summary><b>🔽 Expand Attack Simulation Steps</b></summary>

### Wazuh Rule Engine

Each rule in Wazuh contains:

| Property | Description |
|----------|-------------|
| **Rule ID** | Unique identifier |
| **Severity (0–15)** | 15 = Most Critical |
| **Description** | What the rule detects |
| **Conditions** | Log patterns that trigger it |

### Installing Atomic Red Team on Linux

```bash
# Step 1: Install PowerShell
sudo snap install powershell --classic

# Step 2: Launch PowerShell
pwsh

# Step 3: Install Invoke-AtomicRedTeam (inside pwsh)
Install-Module -Name invoke-atomicredteam,powershell-yaml -Scope CurrentUser -Force
Import-Module invoke-atomicredteam

# Step 4: Get Atomic definitions
Invoke-AtomicTest ALL -GetPrereqs
```

### Executing a MITRE ATT&CK Technique

Technique tested: **T1003.008** — *OS Credential Dumping: /etc/passwd and /etc/shadow*

```powershell
# Execute inside PowerShell on Linux
Invoke-AtomicTest T1003.008
```

> 🎯 This simulates credential dumping behavior that Wazuh's rules are designed to detect.

</details>

---

## 📊 Phase 5 — Event Monitoring & Analysis

<details>
<summary><b>🔽 Expand Monitoring Details</b></summary>

### Security Events View

After running the simulations, the Wazuh Dashboard shows:

| Field | Description |
|-------|-------------|
| **Timestamp** | When the event was detected |
| **Agent** | Which machine generated the event |
| **Rule ID & Description** | Type of activity detected |
| **Severity Level** | How critical the alert is |
| **MITRE Technique ID** | Mapped ATT&CK technique |

### Failed Login Detection

Wazuh's built-in authentication failure rules automatically flag:

```
🔴 Multiple "Authentication Failure" events → Possible brute-force attack
```

These are visible in the **Security Events** section of the dashboard.

</details>

---

## 📈 Results Summary

<div align="center">

| 🔬 Capability | ✅ Result |
|:---|:---|
| **Log Collection** | Successfully collected from Linux and Windows agents |
| **Agent Deployment** | Agents deployed on Ubuntu Clone + Windows 10 FlareVM |
| **Threat Detection** | Detected MITRE ATT&CK technique T1003.008 |
| **Alert Generation** | Alerts visible in Security Events dashboard |
| **Failed Login Detection** | Multiple failed authentication attempts detected |
| **MITRE Mapping** | Events correctly mapped to ATT&CK techniques |

</div>

### 🎯 Conclusion

> This lab successfully demonstrated the **complete SIEM lifecycle** using Wazuh — from installation and multi-platform agent deployment, through real-time log collection and rule-based threat detection, to MITRE ATT&CK-mapped alert analysis.
>
> Wazuh's integration with the MITRE ATT&CK framework enables analysts to understand not just *what* happened, but *which phase of an attack* it represents — making it an effective solution for academic labs, SMEs, and enterprise environments.

---

## 🛠️ Tech Stack

<div align="center">

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-00d4ff?style=for-the-badge)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Windows](https://img.shields.io/badge/Windows_10-FlareVM-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-VM-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?style=for-the-badge&logo=powershell&logoColor=white)
![MITRE](https://img.shields.io/badge/MITRE_ATT%26CK-Framework-FF0000?style=for-the-badge)

</div>

---

## 🚀 Getting Started

```bash
# 1. Clone this repository
git clone https://github.com/<your-username>/SIEM-Wazuh-Deployment.git
cd SIEM-Wazuh-Deployment

# 2. Read the full report
open report/SIEM_Report_25i-7722.docx

# 3. Follow the deployment guide in the README above
#    or refer to: https://documentation.wazuh.com/current/quickstart.html
```

**Prerequisites:**
- VirtualBox or VMware
- Ubuntu 22.04+ VM (minimum 4GB RAM, 2 vCPUs)
- Windows 10 VM (FlareVM optional)
- Internet access inside VMs

---

## 📚 References

- 📖 [Wazuh Official Documentation](https://documentation.wazuh.com)
- ⚔️ [Atomic Red Team](https://atomicredteam.io)
- 🗺️ [MITRE ATT&CK Framework](https://attack.mitre.org)
- 🔧 [Invoke-AtomicRedTeam GitHub](https://github.com/redcanaryco/invoke-atomicredteam)

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:16213e,50:1a1a2e,100:0d1117&height=120&section=footer&text=Made%20with%20🛡️%20for%20Cybersecurity&fontSize=18&fontColor=00d4ff&animation=fadeIn" width="100%"/>

**Sarmad Farooq · 25I-7722 · MS Cybersecurity · NUCES Islamabad**

</div>
