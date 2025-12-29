## Proxmox Lab Architecture and Network Design



### Host Environment

* Hypervisor: Proxmox VE
* Host Machine: Spare desktop (bare-metal Proxmox install)

!\[Proxmox VM List](screenshots/virtual-machines.png)



### Network Design

!\[Proxmox Bridges](screenshots/promox-bridges.png)



##### Network Segmentation Goals

* Prevent attacker VM from directly accessing Wazuh Server
* Ensure monitored endpoints communicate only via SOC internal network
* Allow controlled access to Wazuh dashboard via management bridge
