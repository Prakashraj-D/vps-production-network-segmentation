# Design Decisions

A record of the key architectural choices behind this project and the
reasoning for each — useful context beyond just the diagram.

## Why VLAN segmentation instead of separate physical switches per segment

VLANs give the same isolation guarantees (with correctly configured deny
rules) at a fraction of the hardware cost and cabling complexity. Physical
separation was considered but rejected: three segments would have meant
three sets of switch gear, more failure domains, and no meaningful security
gain over properly enforced 802.1Q tagging plus firewall policy.

## Why dual ISP instead of a single link with LTE failover

LTE failover is cheaper but throughput drops sharply under failover,
which would have made the VM network unusable during an outage rather than
just degraded. Two wired ISPs from different providers gives near-parity
bandwidth on either link, at the cost of a second monthly bill.

## Why LACP bonding on the uplinks

Single-link uplinks were a hard single point of failure between the
hypervisor cluster and the core switch. LACP gives both link redundancy and
aggregate throughput; active-backup was considered but LACP's throughput
benefit made it the better fit given the switch supported it.

## Why the storage network is fully WAN-isolated

Cluster/storage replication traffic is high-volume and has no reason to
ever leave the local network. Isolating it removes an entire class of
exposure (no accidental NAT rule can ever expose it) and also protects
replication performance from being affected by WAN-bound traffic shaping.

## Why management traffic is on its own VLAN rather than sharing the VM VLAN

Putting hypervisor GUI, IPMI, and switch/firewall admin access on the same
segment as internet-facing VMs means a single compromised VM is one hop from
administrative interfaces. A dedicated, non-routed-to-WAN management VLAN
keeps administrative access reachable only from trusted internal paths.

## Why NAT (rather than public IP per VM) for the VM network

Centralizing outbound/inbound translation at the firewall gives one place
to enforce and audit access policy per VM, rather than relying on
per-host firewall rules that are easy to drift out of sync over time.
