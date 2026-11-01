# Raspberry Pi Gateway and Tailscale Remote Access

## Completed Milestone

On 1 November 2026, the operator confirmed:

- The Raspberry Pi is connected to eduroam.
- The Pi serves as the Internet gateway for the wired Proxmox lab.
- Tailscale on the Pi provides remote access.
- The third Fujitsu has joined the Proxmox cluster.

This records the working setup as reported by the operator.
Exact configuration files, command output, and reboot persistence
have not yet been captured for this milestone.

## Device Roles

| Component | Role |
|---|---|
| Pi Wi-Fi connection | Internet uplink through eduroam |
| Pi Ethernet connection | Connection to the wired lab switch |
| Pi gateway configuration | Routes lab Internet traffic through the Wi-Fi uplink |
| Tailscale on the Pi | Remote access entry point |
| pve1, pve2, pve3 | Wired Proxmox cluster nodes |

The Pi provides the gateway and remote access functions. Cluster
membership resides on the three Fujitsu nodes.

## Configuration to Capture

Record the working values rather than reconstructing them from earlier
setup attempts:

| Area | Evidence still needed |
|---|---|
| eduroam | Connection manager, active profile, certificate validation settings, autoconnect behavior |
| LAN | Pi address, prefix, interface name, and connection to the switch |
| Proxmox nodes | Current addresses, gateway, DNS servers, and bridge ports |
| Forwarding | Active IPv4 forwarding setting and how it is persisted |
| NAT and firewall | Active rules, rule management tool, and persistence mechanism |
| Tailscale | Device status and the actual access method used to reach Proxmox |
| Subnet routing, if used | Advertised routes, approval status, and client route acceptance |
| Startup | Wi-Fi reconnect, gateway operation, and Tailscale recovery after reboot |

The historical gateway address was `192.168.100.1`; verify the Pi's
current address before documenting it as the live gateway.

Internet gateway traffic and remote access are separate paths. Successful
Tailscale access to the Pi alone does not demonstrate direct access to
the Proxmox web interfaces. Record how each is reached in the working setup.

## Read-Only Evidence Collection

On each Proxmox node:

```bash
hostname
ip -br -4 addr
ip -4 route
cat /etc/network/interfaces
cat /etc/resolv.conf
bridge link show
pvecm status
pvecm nodes
timedatectl status
getent ahostsv4 debian.org
```

On the Raspberry Pi:

```bash
hostname
ip -br -4 addr
ip -4 route
sysctl net.ipv4.ip_forward
tailscale status
systemctl is-enabled tailscaled
systemctl is-active tailscaled
```

After identifying the network and firewall tools actually in use,
capture their active settings and persistent configuration. Do not
publish eduroam credentials, private keys, Tailscale authentication
keys, or other secrets.

## Remaining Verification

- Record Internet and DNS checks from all three nodes.
- Record access from an external network to the Pi and Proxmox.
- Confirm whether remote access uses subnet routing or an SSH-based method.
- Verify eduroam reconnects automatically after a Pi reboot.
- Verify node Internet access and Tailscale remote access recover after that reboot.
- Add sanitized configuration excerpts and observed test results.

Keep these checks in this guide; keep the overall project milestones
in the README.
