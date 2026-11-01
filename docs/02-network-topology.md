# Network Topology and Configuration

## Current Topology

Status reported by the operator on 1 November 2026:

- All three Fujitsu nodes are members of the Proxmox cluster.
- The Raspberry Pi connects to eduroam over Wi-Fi.
- The Pi provides the wired lab with Internet access.
- Tailscale on the Pi provides remote access to the lab.

| Device | Connection | Role |
|---|---|---|
| pve1 | Ethernet to the lab switch | Proxmox node |
| pve2 | Ethernet to the lab switch | Proxmox node |
| pve3 | Ethernet to the lab switch | Proxmox node |
| Raspberry Pi | Ethernet to the lab switch; Wi-Fi to eduroam | Internet gateway and Tailscale remote access |
| HP Pavilion laptop | Lab network when connected, or remote access | Administration |

The Internet path is: Proxmox nodes → lab switch → Raspberry Pi →
eduroam → Internet. The remote access entry point is Tailscale on the Pi.
Whether access onward to the nodes uses subnet routing, SSH forwarding,
or another method still needs to be captured from the working configuration.

See [Raspberry Pi gateway and Tailscale](04-raspberry-pi-gateway-and-tailscale.md)
for the completed milestone and remaining configuration evidence.

## Address and Interface Records

The following values were recorded on 17 October 2026, before the Pi
gateway and third node were operational. They are historical references,
not a fresh capture of the current configuration.

| Setting | pve1 | pve2 |
|---|---|---|
| IPv4 address | `192.168.100.11/24` | `192.168.100.12/24` |
| Address assignment | Static | Static |
| Default gateway | `192.168.100.1` | `192.168.100.1` |
| Physical interface | `nic0` | `nic0` |
| Linux bridge | `vmbr0` | `vmbr0` |
| DNS server | `1.1.1.1` | `1.1.1.1` |
| DNS search domain | `home.arpa` | `home.arpa` |

Later setup work introduced USB Ethernet adapters and DNS changes.
Capture the current configuration on each node before treating the
historical interface or DNS values as current. The address of pve3 and
the Pi's current LAN address are not yet recorded in this document.

## Bridge Configuration at the Initial Milestone

The persistent network configuration was recorded in
`/etc/network/interfaces`:

- The physical interface used `inet manual`.
- `vmbr0` held the management address and default gateway.
- `bridge-ports` attached the physical interface to `vmbr0`.
- `auto vmbr0` enabled the bridge at startup.
- `bridge-stp off` disabled Spanning Tree Protocol.
- `bridge-fd 0` set the bridge forwarding delay to zero.

The bridge connects guest network interfaces to the physical network.

## Historical Connectivity Tests — 17 October 2026

| Source | Destination | Result |
|---|---|---|
| pve1 | pve2 — `192.168.100.12` | Passed: 0% packet loss |
| pve2 | pve1 — `192.168.100.11` | Passed: 0% packet loss |
| pve2 | Gateway — `192.168.100.1` | Failed: Destination Host Unreachable |
| pve2 | DNS lookup for `debian.org` | No addresses returned |

These failures describe the earlier isolated network. The operator
now confirms that the Pi supplies Internet access; the historical
failures are not the current reported status.

## Remaining Documentation and Verification

- Capture addresses, routes, bridge ports, and DNS on all three nodes.
- Capture the Pi's LAN and Wi-Fi addresses and routing configuration.
- Record node-to-gateway, Internet, and DNS checks on each node.
- Record remote access tests from outside the lab network.
- Verify the configuration survives a Pi reboot.
