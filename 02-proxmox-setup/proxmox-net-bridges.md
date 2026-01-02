# **Proxmox Network Bridges (vmbr0, vmbr1, vmbr2)**

## **Overview**

This document describes how Linux bridges are used in Proxmox to create **network segmentation** for the SOC Home Lab. Each bridge serves a specific purpose aligned with security and observability goals.



Rather than relying on a flat network, multiple bridges are intentionally used to simulate enterprise-style separation of concerns.



## **vmbr0 – Management / Temporary Internet Access**

**Purpose:**

* Proxmox web interface access
* emporary internet connectivity for VM updates



**Characteristics:**

* Connected to the physical NIC
* Provides outbound internet access
* Not used for attack simulations



**Security Considerations:**

* Only attached to VMs during installation or patching
* Removed once services are configured
* Minimizes exposure of internal SOC systems



## **vmbr1 – SOC Internal Network**

**Purpose:**

* Internal communication between monitored systems
* Log forwarding and service dependencies



**Connected Systems:**

* Wazuh Server
* Infrastructure Services VM
* Windows 10 Endpoint
* Ubuntu Endpoint



**Characteristics:**

* No internet access
* Private IP range: **10.10.10.0/24**
* DHCP provided internally



**Why No Internet Access:**

* Forces reliance on internal services
* Reduces noise in logs
* Simulates restricted enterprise networks



## **vmbr2 – Attacker Network**

**Purpose:**

* Dedicated network for attack simulation



**Connected Systems:**

* Kali Linux VM



**Characteristics:**

* No internet access
* Private IP range: 10.20.20.0/24
* Static IP assignment



**Security Rationale:**

* Prevents accidental access to SOC internal systems
* Keeps attack traffic controlled and observable
* Supports safe offensive testing



## **Traffic Isolation Model**

* vmbr0 traffic never mixes with attack traffic
* vmbr1 only carries SOC-related communication
* vmbr2 is isolated and used solely for simulation

This isolation allows accurate detection and correlation in Wazuh without unintended cross-network noise.



## **Operational Workflow**

1. Attach vmbr0 temporarily for OS installation and updates
2. Configure services and agents
3. Detach vmbr0 from SOC and attacker VMs
4. Operate lab using only vmbr1 and vmbr2

This workflow mirrors how staging and production networks are handled in real environments.



**Common Pitfalls Avoided**

* Flat network design
* Permanent internet access for internal systems
* Mixing attacker and monitored traffic

Avoiding these issues improves both security posture and learning outcomes.



**Summary**

Using multiple Proxmox bridges enables a realistic, controlled SOC lab environment. Network segmentation is a foundational skill for blue team and SOC roles, and this setup reinforces that principle throughout the lab.



