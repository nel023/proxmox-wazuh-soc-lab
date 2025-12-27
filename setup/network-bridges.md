\# Proxmox Network Bridges



\## Design Goal

Replicate VirtualBox NAT + Host-Only networking in an enterprise-style environment.



\## Bridges Created



| Bridge | Purpose |

|------|--------|

| vmbr0 | Internet + Management |

| vmbr1 | SOC Internal Network |



\## Security Considerations

\- Physical NICs set to manual

\- vmbr1 has no gateway

\- Prevents unintended outbound traffic



\## Validation

Routing verified using:

ip route



