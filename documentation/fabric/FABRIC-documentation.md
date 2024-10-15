# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| DC1 | l3leaf | dc1-leaf1 | 10.0.0.3/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf2 | 10.0.0.4/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf3 | 10.0.0.5/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf4 | 10.0.0.6/24 | - | Provisioned | - |
| DC1 | spine | dc1-spine1 | 10.0.0.1/24 | - | Provisioned | - |
| DC1 | spine | dc1-spine2 | 10.0.0.2/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf1 | 10.0.0.9/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf2 | 10.0.0.10/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf3 | 10.0.0.11/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf4 | 10.0.0.12/24 | - | Provisioned | - |
| DC2 | spine | dc2-spine1 | 10.0.0.7/24 | - | Provisioned | - |
| DC2 | spine | dc2-spine2 | 10.0.0.8/24 | - | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | dc1-leaf1 | Ethernet49/1 | spine | dc1-spine1 | Ethernet1/1 |
| l3leaf | dc1-leaf1 | Ethernet50/1 | spine | dc1-spine2 | Ethernet1/1 |
| l3leaf | dc1-leaf1 | Ethernet55/1 | mlag_peer | dc1-leaf2 | Ethernet55/1 |
| l3leaf | dc1-leaf1 | Ethernet56/1 | mlag_peer | dc1-leaf2 | Ethernet56/1 |
| l3leaf | dc1-leaf2 | Ethernet49/1 | spine | dc1-spine1 | Ethernet2/1 |
| l3leaf | dc1-leaf2 | Ethernet50/1 | spine | dc1-spine2 | Ethernet2/1 |
| l3leaf | dc1-leaf3 | Ethernet49/1 | spine | dc1-spine1 | Ethernet3/1 |
| l3leaf | dc1-leaf3 | Ethernet50/1 | spine | dc1-spine2 | Ethernet3/1 |
| l3leaf | dc1-leaf3 | Ethernet55/1 | mlag_peer | dc1-leaf4 | Ethernet55/1 |
| l3leaf | dc1-leaf3 | Ethernet56/1 | mlag_peer | dc1-leaf4 | Ethernet56/1 |
| l3leaf | dc1-leaf4 | Ethernet49/1 | spine | dc1-spine1 | Ethernet4/1 |
| l3leaf | dc1-leaf4 | Ethernet50/1 | spine | dc1-spine2 | Ethernet4/1 |
| l3leaf | dc2-leaf1 | Ethernet49/1 | spine | dc2-spine1 | Ethernet1/1 |
| l3leaf | dc2-leaf1 | Ethernet50/1 | spine | dc2-spine2 | Ethernet1/1 |
| l3leaf | dc2-leaf1 | Ethernet55/1 | mlag_peer | dc2-leaf2 | Ethernet55/1 |
| l3leaf | dc2-leaf1 | Ethernet56/1 | mlag_peer | dc2-leaf2 | Ethernet56/1 |
| l3leaf | dc2-leaf2 | Ethernet49/1 | spine | dc2-spine1 | Ethernet2/1 |
| l3leaf | dc2-leaf2 | Ethernet50/1 | spine | dc2-spine2 | Ethernet2/1 |
| l3leaf | dc2-leaf3 | Ethernet49/1 | spine | dc2-spine1 | Ethernet3/1 |
| l3leaf | dc2-leaf3 | Ethernet50/1 | spine | dc2-spine2 | Ethernet3/1 |
| l3leaf | dc2-leaf3 | Ethernet55/1 | mlag_peer | dc2-leaf4 | Ethernet55/1 |
| l3leaf | dc2-leaf3 | Ethernet56/1 | mlag_peer | dc2-leaf4 | Ethernet56/1 |
| l3leaf | dc2-leaf4 | Ethernet49/1 | spine | dc2-spine1 | Ethernet4/1 |
| l3leaf | dc2-leaf4 | Ethernet50/1 | spine | dc2-spine2 | Ethernet4/1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.2.0.0/24 | 256 | 12 | 4.69 % |
| 10.7.0.0/24 | 256 | 16 | 6.25 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| dc1-leaf1 | Ethernet49/1 | 10.1.255.253/31 | dc1-spine1 | Ethernet1/1 | 10.1.255.252/31 |
| dc1-leaf1 | Ethernet50/1 | 10.1.255.255/31 | dc1-spine2 | Ethernet1/1 | 10.1.255.254/31 |
| dc1-leaf2 | Ethernet49/1 | 10.2.0.1/31 | dc1-spine1 | Ethernet2/1 | 10.2.0.0/31 |
| dc1-leaf2 | Ethernet50/1 | 10.2.0.3/31 | dc1-spine2 | Ethernet2/1 | 10.2.0.2/31 |
| dc1-leaf3 | Ethernet49/1 | 10.2.0.5/31 | dc1-spine1 | Ethernet3/1 | 10.2.0.4/31 |
| dc1-leaf3 | Ethernet50/1 | 10.2.0.7/31 | dc1-spine2 | Ethernet3/1 | 10.2.0.6/31 |
| dc1-leaf4 | Ethernet49/1 | 10.2.0.9/31 | dc1-spine1 | Ethernet4/1 | 10.2.0.8/31 |
| dc1-leaf4 | Ethernet50/1 | 10.2.0.11/31 | dc1-spine2 | Ethernet4/1 | 10.2.0.10/31 |
| dc2-leaf1 | Ethernet49/1 | 10.6.255.253/31 | dc2-spine1 | Ethernet1/1 | 10.6.255.252/31 |
| dc2-leaf1 | Ethernet50/1 | 10.6.255.255/31 | dc2-spine2 | Ethernet1/1 | 10.6.255.254/31 |
| dc2-leaf2 | Ethernet49/1 | 10.7.0.1/31 | dc2-spine1 | Ethernet2/1 | 10.7.0.0/31 |
| dc2-leaf2 | Ethernet50/1 | 10.7.0.3/31 | dc2-spine2 | Ethernet2/1 | 10.7.0.2/31 |
| dc2-leaf3 | Ethernet49/1 | 10.7.0.5/31 | dc2-spine1 | Ethernet3/1 | 10.7.0.4/31 |
| dc2-leaf3 | Ethernet50/1 | 10.7.0.7/31 | dc2-spine2 | Ethernet3/1 | 10.7.0.6/31 |
| dc2-leaf4 | Ethernet49/1 | 10.7.0.9/31 | dc2-spine1 | Ethernet4/1 | 10.7.0.8/31 |
| dc2-leaf4 | Ethernet50/1 | 10.7.0.11/31 | dc2-spine2 | Ethernet4/1 | 10.7.0.10/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.1.0.0/24 | 256 | 6 | 2.35 % |
| 10.6.0.0/24 | 256 | 6 | 2.35 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC1 | dc1-leaf1 | 10.1.0.2/32 |
| DC1 | dc1-leaf2 | 10.1.0.3/32 |
| DC1 | dc1-leaf3 | 10.1.0.4/32 |
| DC1 | dc1-leaf4 | 10.1.0.5/32 |
| DC1 | dc1-spine1 | 10.1.0.0/32 |
| DC1 | dc1-spine2 | 10.1.0.1/32 |
| DC2 | dc2-leaf1 | 10.6.0.2/32 |
| DC2 | dc2-leaf2 | 10.6.0.3/32 |
| DC2 | dc2-leaf3 | 10.6.0.4/32 |
| DC2 | dc2-leaf4 | 10.6.0.5/32 |
| DC2 | dc2-spine1 | 10.6.0.0/32 |
| DC2 | dc2-spine2 | 10.6.0.1/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.3.0.0/24 | 256 | 4 | 1.57 % |
| 10.7.0.0/24 | 256 | 16 | 6.25 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC1 | dc1-leaf1 | 10.3.0.2/32 |
| DC1 | dc1-leaf2 | 10.3.0.2/32 |
| DC1 | dc1-leaf3 | 10.3.0.4/32 |
| DC1 | dc1-leaf4 | 10.3.0.4/32 |
| DC2 | dc2-leaf1 | 10.7.0.2/32 |
| DC2 | dc2-leaf2 | 10.7.0.2/32 |
| DC2 | dc2-leaf3 | 10.7.0.4/32 |
| DC2 | dc2-leaf4 | 10.7.0.4/32 |
