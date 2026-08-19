# Glossary

Short explanations of the networking terms used throughout this project,
for readers who aren't network engineers.

**VLAN (Virtual LAN)**
A way to split one physical network into multiple logically isolated
networks without needing separate switches or cabling. Devices on
different VLANs can't talk to each other unless a router or firewall
explicitly allows it.

**802.1Q**
The standard that lets a single physical link carry traffic for multiple
VLANs at once, by "tagging" each frame with its VLAN ID. This is what
makes a trunk port possible.

**Trunk port**
A switch port configured to carry traffic for more than one VLAN
(as opposed to an access port, which belongs to just one VLAN).

**LACP (Link Aggregation Control Protocol)**
Combines multiple physical network links into one logical link, giving
both higher throughput and redundancy — if one physical link fails, traffic
continues over the rest.

**Bond (bond0)**
The logical interface created by combining multiple NICs — the Linux/
Proxmox-side term for a link aggregation group.

**Bridge (vmbr0)**
A virtual switch inside the hypervisor that connects VMs to the physical
network (via the bond, in this design).

**NAT (Network Address Translation)**
Lets multiple internal devices share a smaller number of public IP
addresses, and is commonly used to control and audit what internal hosts
can reach on the internet.

**IPMI (Intelligent Platform Management Interface)**
Out-of-band hardware management — lets an administrator control a server
(power cycle, console access) independent of its operating system, useful
when the OS itself is unreachable.

**Cluster (Proxmox context)**
A group of hypervisor nodes managed as a single unit, allowing VMs to be
migrated between nodes and enabling shared storage/HA features.

**Failover / redundancy**
Design where a backup component automatically takes over if the primary
fails, minimizing downtime.
