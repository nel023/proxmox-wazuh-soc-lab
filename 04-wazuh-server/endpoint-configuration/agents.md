# Wazuh Agent Installation Guide (Windows & Linux)

## Purpose

This document describes the installation and enrollment of **Wazuh agents** on both **Windows** and **Ubuntu Server** endpoints.  
These agents represent monitored systems inside the SOC internal network and are used to validate detection and alerting in Wazuh.

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
- Wazuh Manager IP (SOC network): `10.10.10.10`
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

> ⚠️ The installer **must be run as Administrator**, even if the user belongs to the local Administrators group.

### Option B:
1. On the Wazuh Dashboard, choose **deploy new agent**, and enter the details. A command will be created and can be run directly to the Windows 10 VM.
2. On the Windows 10 VM, open Powershell and run it as Administrator.
3. Paste the command generated from Wazuh Dashboard.
![Wazuh Agent installation](../assets/screenshots/wazuh/wazuh-agent-installation.png)
---

## Service Verification

Open PowerShell (Run as Administrator):

```powershell
Get-Service wazuh*
```
