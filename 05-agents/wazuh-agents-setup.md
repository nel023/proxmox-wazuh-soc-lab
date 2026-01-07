# Wazuh Agent Installation Guide (Windows & Linux)

## Purpose

This document describes the installation and enrollment of **Wazuh agents** on both **Windows** and **Ubuntu Server** endpoints. These agents represent monitored systems inside the SOC internal network and are used to validate detection and alerting in Wazuh.

---

## Lab Architecture Context

- **Wazuh Server:** Ubuntu Server  
  - vmbr0 → Management / Dashboard access  
  - vmbr1 → SOC internal network  
- **Endpoints (Agents):**
  - Windows 10 VM (primary monitored target)
  - Ubuntu Server VM (lightweight Linux endpoint)
- **Attacker VM:** Kali Linux (not enrolled as agent)

---

## Common Requirements

- Network connectivity to Wazuh Server over **vmbr1**
- Wazuh Manager IP (SOC network): `10.10.10.10/24`
- Time synchronization working (local or NTP)
- Administrator / sudo privileges

---

# Windows 10 Agent Setup

## Environment

| Item | Value |
|----|------|
| OS | Windows 10 |
| Role | Wazuh Agent |
| Network | vmbr1 |
| IP Assignment | DHCP |

---

## Installation Steps

### Option A:
1. Download the Windows Wazuh Agent installer from the Wazuh Dashboard.
2. Right-click the installer → **Run as administrator**.
3. During installation, provide:
   - **Wazuh Manager IP:** `10.10.10.10`
   - **Agent name:** Windows hostname
4. Complete the installation.

> The installer **must be run as Administrator**, even if the user belongs to the local Administrators group.

### Option B:
1. On the Wazuh Dashboard, choose **deploy new agent**, and enter the details. A command will be created and can be run directly to the Windows 10 VM.
2. On the Windows 10 VM, open Powershell and run it as Administrator.
3. Paste the command generated from Wazuh Dashboard.
![Wazuh Agent installation](/assets/screenshots/wazuh/wazuh-agent-installation.png)
---

## Service Verification

Open PowerShell (Run as Administrator):

```powershell
Get-Service wazuh*
```

If the service is not running:

```powershell
Start-Service wazuh-agent
```

# Ubuntu Server Agent Setup

## Environment

| Item | Value |
|-----|------|
| OS | Ubuntu Server (Minimal Install) |
| Role | Wazuh Agent |
| Network | vmbr1 (SOC Internal Network) |
| IP Assignment | DHCP |

---

## Package Selection

From the Wazuh Dashboard, select the following agent package:

- **DEB amd64**

This package is appropriate for Ubuntu Server running on x86_64 architecture.

---

## Installation Steps

1. From the Ubuntu Server VM, download and install the Wazuh agent using the command provided by the Wazuh Web UI.

Example:

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-agent_amd64.deb
sudo WAZUH_MANAGER='10.10.10.10' dpkg -i wazuh-agent_*.deb
```

2. Reload system services and start the agent:
```bash
sudo systemctl daemon-reexec
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

## Agent Status Verification (Ubuntu)

Verify that the agent is running correctly:
```bash
sudo systemctl status wazuh-agent
```

The service should be in an active (running) state.

## Agent Enrollment Verification (Server Side)

On the Wazuh Server, list enrolled agents:

```bash
sudo /var/ossec/bin/agent_control -lc
```
![Wazuh list of Agents](/assets/screenshots/wazuh/wazuh-list-agents.png)
Expected result:
- Ubuntu agent listed as Active
- Windows 10 agent listed as Active
  
Agents should also appear in the Wazuh Dashboard under Agents.
![Wazuh endpoints](/assets/screenshots/wazuh/wazuh-endpoints.png)

## Design Notes
- All agents communicate with the Wazuh Server over vmbr1 (SOC internal network).
- Agents use DHCP-assigned IP addresses to demonstrate infrastructure services.
- Kali Linux is intentionally not enrolled as a Wazuh agent, as it represents an attacker system.
- This setup focuses on endpoint visibility, detection, and attack simulation, rather than full enterprise infrastructure complexity.
