### Wazuh Server Setup



##### Base OS

* Ubuntu Server LTS
* Minimal installation

!\[Wazuh Server VM](wazuh-server-vm.png)



##### Wazuh Installation

* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard

###### Installation method:

* Official Wazuh installation script (all-in-one deployment)

!\[Wazuh Dashboard](screenshots/wazuh-dashboard.png)



##### Network Interfaces

* NIC 1 (vmbr0): Dashboard access
* NIC 2 (vmbr1): Agent communication



##### Post-Installation Validation

* Dashboard accessible from Windows host
* Wazuh services running
* Indexer health verified
