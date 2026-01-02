# **DNS Setup – Infrastructure Services VM**

## **Overview**

This document is reserved for the future implementation of an **internal DNS service** within the SOC home lab.

At this stage of the project, DNS is considered **optional** and is intentionally deferred to prioritize:

* Endpoint attack simulations
* Host-based monitoring with Wazuh
* Alert analysis and incident investigation workflows

All endpoints currently rely on DHCP-assigned IP addresses and direct IP communication, which is sufficient for the current learning objectives.



## **Notes**

DNS will be implemented in a later phase to simulate name resolution in a larger SOC environment

Future use cases may include:

* Hostname-based log correlation
* Service discovery
* Enterprise-like network segmentation
