# Network Topology and Configuration

## Purpose

The lab network connects two Proxmox nodes and the administration
laptop through an Ethernet switch.

The Raspberry Pi remote-access configuration is still in progress.

## Physical Connections

| Device | Connection | Role |
|---|---|---|
| Fujitsu pve1 | Ethernet to the switch | Proxmox node |
| Fujitsu pve2 | Ethernet to the switch | Proxmox node |
| HP Pavilion laptop | Ethernet to the switch | Administration |
| Raspberry Pi | Not yet connected to the lab switch | Remote access setup in progress |

The third Fujitsu node is awaiting replacement.

## Network Configuration

| Setting | pve1 | pve2 |
|---|---|---|
| IPv4 address | `192.168.100.11/24` | `192.168.100.12/24` |
| Address assignment | Static | Static |
| Network | `192.168.100.0/24` | `192.168.100.0/24` |
| Subnet mask | `255.255.255.0` | `255.255.255.0` |
| Default gateway | `192.168.100.1` | `192.168.100.1` |
| Physical interface | `nic0` | `nic0` |
| Linux bridge | `vmbr0` | `vmbr0` |
| DNS server | `1.1.1.1` | `1.1.1.1` |
| DNS search domain | `home.arpa` | `home.arpa` |

## Bridge Configuration

The persistent network configuration is recorded in
`/etc/network/interfaces`.

On each node:

- `nic0` is configured with `inet manual` and has no IPv4
  address assigned in this file.
- `nic0` is attached to `vmbr0` through `bridge-ports nic0`.
- The static management address and default gateway are
  configured on `vmbr0`.
- `auto vmbr0` enables the bridge during network startup.
- Spanning Tree Protocol is disabled with `bridge-stp off`.
- The bridge forwarding delay is set to zero with `bridge-fd 0`.

The bridge acts as a virtual switch, allowing guest interfaces
attached to it to communicate through the physical Ethernet port.

## Runtime Verification

The following commands are executed on both nodes:

```bash
hostname
ip -br -4 addr
ip -4 route
cat /etc/network/interfaces
bridge link show
cat /etc/resolv.conf
```

The outputs confirm:

- Both nodes have unique static management addresses.
- Both management bridges are UP.
- Both physical interfaces report UP and LOWER_UP.
- Each `nic0` belongs to `vmbr0` and is in forwarding state.
- Both nodes have a default route through `192.168.100.1`.
- Both nodes are configured to use DNS server `1.1.1.1`.

## Connectivity Test Results

Tests are performed on 17 October 2026.

| Source | Destination | Result |
|---|---|---|
| pve1 | pve2 — `192.168.100.12` | Passed: 0% packet loss |
| pve2 | pve1 — `192.168.100.11` | Passed: 0% packet loss |
| pve2 | Gateway — `192.168.100.1` | Failed: Destination Host Unreachable |
| pve2 | DNS lookup for `debian.org` | No addresses returned |

Local connectivity between the Proxmox nodes is verified.
The configured default gateway is currently unreachable from
pve2, and DNS resolution is unsuccessful.

## Current Network Isolation

The switch currently connects only the two Fujitsu nodes and 
the adminstration laptop (if present). It has no Internet uplink.

Connectivity between the nodes is verified. The configured
gateway `192.168.100.1` is not currently provided by a device,
so gateway reachability and DNS resolution are unsuccessful.

Internet connectivity is planned through the Raspberry Pi,
using Wi-Fi for the Internet connection and Ethernet for the
lab network. The Pi will also provide remote access through
Tailscale. This configuration is not yet operational.

## Remote Access — In Progress

The Raspberry Pi is intended to provide remote access through
Tailscale. Its final connections, IP addresses, and routing
configuration will be documented after configuration and testing.

## Remaining Tasks

- Configure the Raspberry Pi as the lab’s Internet gateway and verify gateway reachability.
- Verify Internet access and DNS resolution from both Proxmox nodes.
- Configure and test remote access through Tailscale on the Raspberry Pi.
