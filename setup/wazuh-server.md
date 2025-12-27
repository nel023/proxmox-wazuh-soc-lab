# Wazuh Server Setup

## VM Configuration
- OS: Ubuntu Server
- NIC 1: vmbr0 (Internet)
- NIC 2: vmbr1 (SOC Internal)

## Network Configuration
- Internet NIC has default gateway
- SOC NIC has static IP with no gateway

## Wazuh Installation
Installed using the all-in-one installer:

sudo ./wazuh-install.sh -a

## Result
Wazuh Manager, Indexer, and Dashboard successfully deployed.
