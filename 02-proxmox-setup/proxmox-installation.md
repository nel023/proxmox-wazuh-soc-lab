# **Proxmox VE Installation**
## **Overview**
This document outlines how **Proxmox VE** was installed and prepared as the hypervisor for the Wazuh SOC Home Lab. The focus is on stability, clarity, and features relevant to running multiple security-focused virtual machines on a single host.



The installation follows a straightforward approach, avoiding unnecessary customization while ensuring the system is suitable for SOC-style workloads.



## **Hardware Context**

The Proxmox host is a spare desktop machine repurposed to run a multi-VM lab environment.



### **General considerations:**
* Adequate CPU cores to support concurrent VMs
* Sufficient RAM to avoid swapping during attack simulations
* SSD storage for faster VM I/O and log indexing

This setup mirrors how small SOC labs or proof-of-concept environments are commonly built.



## **Installation Method**

* Proxmox VE installed directly on bare metal
* Default installer used with local storage (LVM)
* No additional packages installed during setup

Using the default installer helps reduce complexity and aligns with common operational deployments.



## **Initial Configuration**

* After installation, the following steps were performed:
* Verified access to the Proxmox web interface
* Set a strong password for the root account
* Confirmed system time and timezone

Consistent time configuration is important for later log correlation across SOC components.



## **Storage Configuration**

* Local storage used for VM disks and ISO images
* Disk layout left unchanged from installer defaults

This decision prioritizes simplicity and reliability over performance tuning, which is acceptable for a learning-focused lab.



## **Network Readiness**

Proxmox automatically creates a default Linux bridge (`vmbr0`) during installation. Additional bridges are added later to support SOC segmentation.

Network configuration details are documented separately in:

* [Proxmox Network Bridges (vmbr0, vmbr1, vmbr2)](../02-proxmox-setup/proxmox-net-bridges.md)



**Update and Maintenance**

After installation, the host was updated:

***apt update \&\& apt upgrade -y***



Regular updates ensure:

* Kernel stability
* Security patches
* Improved hardware compatibility



## **Operational Notes**

* Proxmox host is treated as trusted infrastructure
* No lab workloads are run directly on the host
* All attack simulations are confined to guest VMs



## **Why Proxmox for This Lab**

Proxmox VE was selected because it:

* Supports flexible network segmentation
* Makes VM and resource management visible
* Is widely used in labs, enterprises, and MSP environments
* Allows realistic SOC-style deployments without cloud dependencies



## **Summary**

This installation provides a clean, stable foundation for deploying the SOC lab. Subsequent documentation focuses on network segmentation, VM design, and security tooling built on top of this base.



