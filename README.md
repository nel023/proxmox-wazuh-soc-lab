# **Proxmox-Based Wazuh SOC Home Lab**



### **Overview**

This project documents a \*\*self-built Security Operations Center (SOC) home lab\*\* deployed on \*\*Proxmox VE\*\*, centered around \*\*Wazuh SIEM\*\*. The lab is designed to mirror how monitoring, detection, and supporting infrastructure are implemented in real enterprise environments—not just to make tools work, but to understand \*why\* they are deployed the way they are.



Rather than relying on pre-packaged appliances, each component is intentionally designed, deployed, and documented to demonstrate foundational blue team and SOC engineering skills.



### **Goals of This Lab**

* Build a realistic SOC environment using open-source tools
* Understand infrastructure dependencies behind a SIEM deployment
* Simulate attacker activity and observe detections
* Practice log analysis, alert triage, and investigation workflows
* Document decisions, trade-offs, and limitations clearly



This repository is meant to reflect **how a junior SOC analyst or blue team engineer thinks**, not just what commands they can run.



### **Lab Environment Summary**

The lab runs on a **dedicated Proxmox host** with multiple isolated virtual networks:

* **vmbr0** – Management / temporary internet access
* **vmbr1** – SOC internal network (monitored environment)
* **vmbr2** – Attacker network (isolated)



#### **Virtual Machines**

| **VM**                                  | **Purpose**                              |

| ----------------------------------- | ------------------------------------ |

| Wazuh Server (Ubuntu)               | SIEM manager, indexer, and dashboard |

| Infrastructure Services VM (Ubuntu) | DHCP, DNS, NTP, Syslog               |

| Windows 10 Endpoint                 | Monitored user workstation           |

| Ubuntu Endpoint                     | Monitored Linux system               |

| Kali Linux                          | Attack simulation platform           |



Each VM is documented with its role, configuration, and security considerations.



### **What This Lab Demonstrates**

This project is focused on \*\*fundamentals\*\*, not shortcuts:

* Network segmentation and traffic control
* Centralized logging and time synchronization
* Endpoint monitoring with agents
* Detection of common attack techniques
* Understanding how infrastructure logs support investigations



The intent is to build a solid base that can later be expanded into:

* Detection engineering
* Threat hunting
* Automation
* Purple team exercises



### **Repository Structure**

proxmox-wazuh-soc-homelab/

├── 01-architecture/

├── 02-proxmox-setup/

├── 03-infra-services-vm/

├── 04-wazuh-server/

├── 05-agents/

├── 06-attack-simulation/

├── 07-detection-use-cases/

└── assets/





#### **Why Proxmox + Wazuh**

* **Proxmox** provides flexibility, visibility, and realistic network segmentation
* **Wazuh** offers endpoint visibility, log analysis, and detection without vendor lock-in



#### **Assumptions**

* This lab is built for learning and demonstration purposes
* High availability and redundancy are out of scope
* Security controls are intentionally balanced with observability



These assumptions are documented openly rather than hidden.



#### **Who This Is For**

* Aspiring SOC analysts
* Blue team learners
* IT professionals transitioning into cybersecurity
* Anyone wanting hands-on SIEM experience beyond theory



#### **How to Use This Repository**

* Start with **01-architecture** to understand design decisions
* Follow the setup sections to reproduce the lab
* Review attack simulations and detections to understand SOC workflows
* Use the documentation as a reference during interviews or learning reviews



#### **Disclaimer**

This lab is for **educational purposes only**. Attack simulations are conducted in an isolated environment against systems owned and controlled by the author.



#### **Author**

Built and documented as part of a personal cybersecurity learning journey, with a focus on practical SOC and blue team skills.

