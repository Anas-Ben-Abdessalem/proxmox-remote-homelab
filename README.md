# Proxmox Remote Home Lab

A home lab built with Fujitsu thin clients, Proxmox VE, and a
Raspberry Pi to learn virtualization, clustering, networking,
and secure remote administration.

This repository documents the setup process, configuration
decisions, troubleshooting, and practical exercises.

## Objectives

- Install and configure Proxmox VE on physical machines.
- Build a cluster and understand how quorum works.
- Create and manage virtual machines and containers.
- Configure secure remote access using a Raspberry Pi and Tailscale.
- Practice backups, recovery, and node maintenance.

## Hardware

| Device | Specifications | Intended role |
|---|---|---|
| 3 × Fujitsu Futro S930 | AMD GX-424CC, 4 cores, 8 GB RAM, 64 GB SSD per node | Proxmox cluster nodes |
| HP Pavilion laptop | Intel Core i7, 11th generation, 20 GB RAM | Administration and additional local VMs |
| Raspberry Pi | 4 GB RAM, 128 GB microSD card | Internet gateway and Tailscale remote access |
| Ethernet switch | Connects the wired lab devices | Lab network |

All three Fujitsu nodes (`pve1`, `pve2`, and `pve3`) are now members
of the cluster. The Raspberry Pi connects to eduroam over Wi-Fi and
provides Internet access to the wired lab, plus remote access through
Tailscale.

Status updated on 1 November 2026 from the operator's confirmation.
Configuration captures and reboot verification remain to be documented.

## Current Progress

- [x] Prepare the available Fujitsu hardware.
- [x] Install Proxmox VE on all three nodes.
- [x] Access the Proxmox web interface.
- [x] Document hostnames and network configuration.
- [x] Document and verify the cluster configuration.
- [x] Connect the Raspberry Pi to eduroam.
- [x] Use the Raspberry Pi as the lab's Internet gateway.
- [x] Configure remote access through Tailscale on the Raspberry Pi.
- [x] Add the third Fujitsu node to the cluster.
- [ ] Capture current membership and quorum output from all three nodes.
- [ ] Verify persistent time synchronization on all nodes.
- [ ] Record current addresses, interfaces, DNS, forwarding, and NAT configuration.
- [ ] Record the Tailscale access method and any configured subnet routes.
- [ ] Verify DNS from each node and connectivity after a Raspberry Pi reboot.
- [ ] Complete and document practical lab exercises.


## Available Guides

1. [Proxmox installation and initial verification](docs/01-proxmox-installation.md)
2. [Network topology and configuration](docs/02-network-topology.md)
3. [Cluster creation and quorum verification](docs/03-cluster-and-quorum.md)
4. [Raspberry Pi gateway and Tailscale remote access](docs/04-raspberry-pi-gateway-and-tailscale.md)

## Documentation Plan

1. Hardware and network topology
2. Proxmox installation
3. Network configuration
4. Cluster creation and quorum
5. Raspberry Pi setup
6. Tailscale remote access
7. Virtual machines and containers
8. Backups and recovery
9. Troubleshooting and lessons learned

## Project Status

Work in progress. Completed tasks and tested configurations
are added as the lab develops.
