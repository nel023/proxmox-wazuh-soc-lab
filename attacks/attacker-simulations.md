### Attacker Simulation (Kali Linux)

##### 

##### Role

The Kali Linux VM is used to simulate realistic attack scenarios against monitored endpoints.



##### Example Attacks

* SSH brute-force attempts
* Credential abuse
* Suspicious network scans
* Malware file drops (test samples)



##### Isolation Strategy

* Kali VM connected only to vmbr2
* No direct route to Wazuh Server
* Attacks routed via endpoints where applicable
