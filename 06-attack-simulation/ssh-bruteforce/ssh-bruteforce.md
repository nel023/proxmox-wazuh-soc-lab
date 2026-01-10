# SSH Brute Force Attack Simulation

## Use Case Overview

This use case documents a simulated **SSH brute-force–style authentication attack** against a **Windows 10 endpoint** monitored by Wazuh.

Instead of using automated tools, this simulation uses **manual SSH login attempts** via OpenSSH to generate repeated authentication failures. This approach reflects scenarios where attackers test credentials manually or through scripted but low-noise techniques.

The objective is to validate Wazuh’s ability to:
- Detect repeated failed SSH authentication attempts on Windows
- Capture attacker source details
- Generate meaningful alerts for SOC investigation

---

## Lab Context

| Component | Description |
|--------|------------|
| Attacker | Kali Linux VM |
| Target | Windows 10 VM |
| Wazuh Agent | Installed on Windows 10 |
| Network | SOC internal network |
| Wazuh Server | Ubuntu Server (dual-NIC configuration) |

---

## Attack Walkthrough

### Step 1: Preparation

Before executing the attack:
- Confirmed that the **OpenSSH Server** feature was enabled on the Windows 10 VM
- Verified that the Windows 10 VM was reachable from the Kali VM
- Identified the Windows 10 VM IP address within the SOC network

### Step 2: Attack Execution

From the Kali Linux VM, multiple SSH login attempts were performed using an invalid or incorrect password.

Example command:

```bash
ssh ronel@10.20.20.100
```

#### Evidence: Attack Execution

![Failed SSH login attempts from Kali VM](./assets/screenshots/kali-vm/kali-ssh-failed-attempts.png)

The command was executed repeatedly with incorrect credentials, intentionally generating authentication failure events on the Windows endpoint.

### Step 3: Target Behavior

On the Windows 10 VM:
- Failed SSH authentication attempts were recorded as **Windows Security Event ID 4625**
- Events confirmed repeated logon failures for the targeted user account
- Logs were successfully collected by the Wazuh agent and forwarded to the Wazuh server

However, during analysis, it was observed that **source network address and hostname fields were not populated** in the Windows Security logs for these failed SSH attempts.

This behavior is consistent with limitations in **Windows OpenSSH and Windows Security Auditing**, where certain failed SSH logons do not include attacker network attribution details.

---

## Detection and Observation

### Wazuh Alerts

#### Evidence: Wazuh Detection

![Wazuh alert table showing failed SSH authentication attempts](./assets/screenshots/wazuh/wazuh-ssh-bruteforce-alerts.png)

In the Wazuh Dashboard:
- Alerts were generated for repeated SSH authentication failures
- Events were correctly correlated as failed logon attempts
- Alert severity reflected authentication abuse behavior

However:
- **Source IP address and attacker hostname were not present**
- Wazuh could not populate attacker attribution fields due to missing data in the originating Windows Security Event (4625)

This limitation originated from the log source itself, not from Wazuh parsing or rule logic.


### Log Analysis

- Logs originated from **Windows Security Event ID 4625**
- Wazuh successfully detected and correlated repeated failed SSH authentication attempts
- Source network address and workstation name fields were empty in the original events
- As a result, Wazuh alerts lacked attacker IP and hostname attribution

Attacker origin (Kali Linux VM) was instead validated through:
- Lab network topology
- Controlled attack execution
- Temporal correlation between attack execution and alert timestamps


## Recommendations

Based on this simulation and observed limitations:

- Collect **OpenSSH operational logs** in addition to Windows Security logs
- Correlate **Windows Firewall logs** to improve attacker attribution
- Tune Wazuh rules to enrich alerts when SSH failures lack source context
- Treat repeated authentication failures without source IP as a detection gap, not a false positive

This highlights the importance of multi-source log correlation in SOC environments, especially on Windows endpoints running OpenSSH.

## Notes
- This attack was executed in a controlled lab environment.
- No valid credentials were compromised.
- Kali Linux is intentionally not enrolled as a Wazuh agent.
- The purpose of this simulation is detection validation, not exploitation.
