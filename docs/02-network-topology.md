# Network Topology and Configuration

## Purpose

The lab network connects the Proxmox nodes and administration
laptop through an Ethernet switch.

A Raspberry Pi is being configured to provide remote access
to the lab through Tailscale.

## Current Physical Connections

| Device | Connection | Role |
|---|---|---|
| Fujitsu pve1 | Ethernet to the switch | Proxmox node |
| Fujitsu pve2 | Ethernet to the switch | Proxmox node |
| HP Pavilion laptop | Ethernet to the switch | Administration |
| Raspberry Pi | Not yet connected to the lab switch | Remote access setup in progress |

The third Fujitsu node is awaiting replacement.

## Proxmox Network Addresses

| Setting | pve1 | pve2 |
|---|---|---|
| Hostname | `pve1` | `pve2` |
| IPv4 address | `192.168.100.11` | `192.168.100.12` |
| Prefix length | `/24` | `/24` |
| Subnet mask | `255.255.255.0` | `255.255.255.0` |
| Network | `192.168.100.0/24` | `192.168.100.0/24` |
| Default gateway | `192.168.100.1` | `192.168.100.1` |
| Management interface | `vmbr0` | `vmbr0` |

The addresses above are observed in the running configuration.
The persistent configuration and address assignment method
have not yet been recorded.

## Proxmox Bridge

On both nodes, the management IPv4 address is assigned to
`vmbr0`, which is reported as UP.

A Linux bridge acts as a virtual switch. It can connect guest
network interfaces to the physical network through an attached
Ethernet interface.

The physical interface attached to each bridge will be
documented after checking the bridge configuration.

## Verification

The following read-only commands are executed on each node:

```bash
hostname
ip -br -4 addr
ip -4 route
```

The outputs confirm:

- Unique node hostnames and IPv4 addresses.
- Both management interfaces are UP.
- A connected route to `192.168.100.0/24`.
- A default route through `192.168.100.1`.

These outputs record the network configuration; they do not
by themselves verify gateway or Internet connectivity.

## Remote Access — In Progress

The Raspberry Pi is intended to provide remote access through
Tailscale. Its final network connections, addressing, and routing
configuration will be documented after configuration and testing.

## Remaining Tasks

- Record the physical Ethernet interface attached to each bridge.
- Record the persistent IP configuration and DNS settings.
- Verify connectivity between the nodes and to the gateway.
- Complete and test Raspberry Pi remote access.
