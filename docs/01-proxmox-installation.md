# Proxmox VE Installation and Initial Verification

## Purpose

Proxmox VE is installed on two Fujitsu Futro S930 thin clients
to provide the virtualization hosts for the home lab.

This document records the installed software versions, storage
layout, and commands used to verify the initial setup.

## Hardware

Each operational node has:

- AMD GX-424CC processor with 4 cores at 2.4 GHz
- 8 GB RAM
- 64 GB SSD, reported by Linux as approximately 59.6 GiB
- An Ethernet connection to the lab switch

A third Fujitsu unit is faulty and awaiting replacement.

## Installed Software

| Setting | Node 1 | Node 2 |
|---|---|---|
| Hostname | `pve1` | `pve2` |
| Proxmox VE manager version | `9.2.2` | `9.2.2` |
| Running kernel | `7.0.2-6-pve` | `7.0.2-6-pve` |
| Installation disk | `/dev/sda` | `/dev/sda` |
| Root filesystem | ext4 | ext4 |

The HP Pavilion laptop is used to access the Proxmox web
interface for administration.

## Storage Layout

The following sizes are rounded values reported by `lsblk`.

| Component | Purpose | pve1 | pve2 |
|---|---|---|---|
| `sda` | Physical SSD | 59.6 GiB | 59.6 GiB |
| `sda2` | EFI system partition, mounted at `/boot/efi` | 512 MiB | 512 MiB |
| `sda3` | Partition containing LVM storage | 58.5 GiB | 59.1 GiB |
| `pve-root` | ext4 root filesystem, mounted at `/` | 24.8 GiB | 25.9 GiB |
| `pve-swap` | Swap space | 7.3 GiB | 3.5 GiB |
| `pve-data` | LVM thin pool for guest storage | 17.2 GiB | 20.3 GiB |

The nodes have different allocations for root, swap, and guest
storage. These values describe the current configuration.

The `pve-data` sizes represent pool capacity, not measured free
space. The `_tmeta` and `_tdata` entries shown by `lsblk` are
components of the thin pool.

## Verification

The following read-only commands are executed on each node
through the Proxmox web interface under Node → Shell.

### Check the hostname

```bash
hostname
```

Expected output: `pve1` on the first node and `pve2` on the second.

### Check the Proxmox version

```bash
pveversion
```

Both nodes report Proxmox VE manager version `9.2.2` and
running kernel `7.0.2-6-pve`.

### Inspect the storage layout

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

The output confirms the installation disk, EFI partition,
LVM storage, root filesystem, and swap allocation.

## Result

- Both nodes boot into Proxmox VE.
- The web interface is accessible.
- Each node has a unique hostname.
- Both nodes run the same Proxmox VE manager and kernel versions.
- The storage layout of each node is recorded.

Cluster membership and network configuration are documented
separately.
