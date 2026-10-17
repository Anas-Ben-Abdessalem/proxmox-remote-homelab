\# Proxmox Cluster Creation and Quorum Verification



\## Purpose



Create the `homelab` cluster from the two operational Fujitsu

Futro S930 nodes and verify cluster membership and quorum.



This milestone follows the

\[installation verification](01-proxmox-installation.md) and

\[network configuration](02-network-topology.md).



Verification date: \*\*17 October 2026\*\*.



\## Lab Configuration



| Setting | pve1 | pve2 |

|---|---|---|

| Proxmox VE manager version | `9.2.2` | `9.2.2` |

| Running kernel | `7.0.2-6-pve` | `7.0.2-6-pve` |

| Management address | `192.168.100.11/24` | `192.168.100.12/24` |

| Management bridge | `vmbr0` | `vmbr0` |

| Physical bridge port | `nic0` | `nic0` |

| Cluster link address | `192.168.100.11` | `192.168.100.12` |



Both nodes connect to the Ethernet switch. The existing wired

management network is also used for cluster communication.



The HP Pavilion laptop provides browser-based administration.

It is not a Proxmox cluster member.



The third Fujitsu is faulty and awaiting replacement.



The Raspberry Pi is not yet connected to the lab switch.

Its gateway, Tailscale, and quorum witness configuration

remains unfinished.



\## Checks Before Creation



\- Both nodes have distinct hostnames and static addresses.

\- Both nodes run the same Proxmox VE manager and kernel versions.

\- Node-to-node ping tests passed with 0% packet loss.

\- The clocks on both nodes were checked and confirmed aligned.



The gateway `192.168.100.1` was unreachable and DNS resolution

failed during the preceding network checks.



Working local connectivity was sufficient to create the cluster.

Internet connectivity remains a separate task.



\### UTC and Local Time



A two-hour difference was noticed when comparing UTC with German

local time.



The command `date -u` displays UTC. On 17 October 2026,

`Europe/Berlin` uses UTC+2.



This display difference did not indicate a two-hour difference

between the node clocks.



Persistent time synchronization still needs to be verified

and documented.



\## Create the Cluster on pve1



1\. Open `https://192.168.100.11:8006`.

2\. Select \*\*Datacenter → Cluster → Create Cluster\*\*.

3\. Enter `homelab` as the cluster name.

4\. Select `192.168.100.11` for \*\*Link 0\*\*.

5\. Click \*\*Create\*\* and wait for \*\*TASK OK\*\*.



The cluster is created on pve1 only. The second node joins

this existing cluster.



\## Join pve2



A joining node must contain no VMs or containers. Joining replaces

its existing `/etc/pve` configuration and imports the cluster

configuration.



1\. On pve1, select \*\*Datacenter → Cluster → Join Information\*\*.

2\. Click \*\*Copy Information\*\*.

3\. Open `https://192.168.100.12:8006`.

4\. Select \*\*Datacenter → Cluster → Join Cluster\*\*.

5\. Paste the join information.

6\. Enter \*\*pve1's root password\*\*.

7\. If a local \*\*Link 0\*\* address is requested, select

&#x20;  `192.168.100.12`.

8\. Click \*\*Join\*\* and wait for completion.



The joining node receives a certificate signed by the cluster

certificate authority. Refresh the browser and log in again

if the session disconnects.



\## Runtime Verification



Run on both nodes through \*\*Node → Shell\*\*:



```bash

pvecm status

```



The outputs from both nodes confirm:



| Field | pve1 | pve2 |

|---|---|---|

| Cluster name | `homelab` | `homelab` |

| Config Version | `2` | `2` |

| Transport | `knet` | `knet` |

| Secure auth | `on` | `on` |

| Nodes | `2` | `2` |

| Node ID | `0x00000001` | `0x00000002` |

| Quorate | `Yes` | `Yes` |

| Expected votes | `2` | `2` |

| Highest expected | `2` | `2` |

| Total votes | `2` | `2` |

| Quorum | `2` | `2` |

| Flags | `Quorate` | `Quorate` |



Both nodes report the same membership:



| Node ID | Votes | Address |

|---|---|---|

| `0x00000001` | `1` | `192.168.100.11` |

| `0x00000002` | `1` | `192.168.100.12` |



The `(local)` marker identifies the node where the command

is executed.



\## Understanding the Current Quorum



Quorum determines whether enough votes are available to make

cluster configuration changes safely.



Each node contributes one vote. The cluster currently has

two votes available and requires two.



If either node becomes unavailable, the remaining node loses

quorum and the cluster configuration filesystem becomes

read-only.



Loss of quorum alone does not automatically stop ordinary

running guests. High availability introduces additional

behavior and is not configured by this milestone.



A Raspberry Pi quorum witness is planned for the two-node

setup. It would provide an external vote without becoming

a Proxmox virtualization host.



\## Result



\- The `homelab` cluster contains both operational Fujitsu nodes.

\- Both nodes report `Quorate: Yes`.

\- Both nodes agree on cluster membership.

\- Either node's web interface can be used for cluster administration.



Creating the cluster does not automatically configure shared

storage, guest disk replication, Ceph, backups, or high availability.



The different local LVM thin-pool capacities recorded in the

installation guide remain separate local storage capacities.



\## Remaining Tasks



\- Verify and document persistent time synchronization on both nodes.

\- Evaluate and configure a quorum witness for the two-node cluster.

\- Join the replacement Fujitsu to the cluster and reassess quorum.



\## Reference



\[Official Proxmox cluster documentation](https://github.com/proxmox/pve-docs/blob/master/pvecm.adoc)

