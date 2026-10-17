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
| Raspberry Pi | 4 GB RAM, 128 GB microSD card | Remote access gateway; potential quorum device |
| Ethernet switch | Connects the wired lab devices | Lab network |

Two Fujitsu nodes are currently operational. The third unit is
faulty and awaiting replacement.

## Current Progress

- [x] Prepare the available Fujitsu hardware.
- [x] Install Proxmox VE on two nodes.
- [x] Access the Proxmox web interface.
- [x] Document hostnames and network configuration.
- [x] Document and verify the cluster configuration.
- [ ] Verify persistent time synchronization on both nodes.
- [ ] Configure Raspberry Pi connectivity and SSH access.
- [ ] Configure the Raspberry Pi Internet gateway and verify Internet access and DNS resolution.
- [ ] Configure remote access through Tailscale.
- [ ] Evaluate and configure a quorum device for the two-node setup.
- [ ] Add the replacement Fujitsu node and reassess quorum.
- [ ] Complete and document practical lab exercises.


## Available Guides

1. [Proxmox installation and initial verification](docs/01-proxmox-installation.md)
2. [Network topology and configuration](docs/02-network-topology.md)
3. [Cluster creation and quorum verification](docs/03-cluster-and-quorum.md)

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
