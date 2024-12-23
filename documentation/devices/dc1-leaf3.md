# dc1-leaf3

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management SSH](#management-ssh)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Logging](#logging)
  - [SNMP](#snmp)
  - [SFlow](#sflow)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Errdisable](#errdisable)
  - [Errdisable Summary](#errdisable-summary)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | mgmt | 10.0.0.5/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | mgmt | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   vrf mgmt
   ip address 10.0.0.5/24
```

### DNS Domain

DNS domain: poc.local

#### DNS Domain Device Configuration

```eos
dns domain poc.local
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 10.0.0.247 | mgmt | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf mgmt 10.0.0.247
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **Europe/Stockholm**.

#### Clock Device Configuration

```eos
!
clock timezone Europe/Stockholm
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| ntp.poc.local | mgmt | True | - | True | 4 | 3 | 9 | Management1 | - |

#### NTP Device Configuration

```eos
!
ntp server vrf mgmt ntp.poc.local prefer iburst version 4 minpoll 3 maxpoll 9 local-interface Management1
```

### Management SSH

#### SSH Timeout and Management

| Idle Timeout | SSH Management |
| ------------ | -------------- |
| 30 | Enabled |

#### Max number of SSH sessions limit and per-host limit

| Connection Limit | Max from a single Host |
| ---------------- | ---------------------- |
| - | - |

#### Ciphers and Algorithms

| Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
|---------|----------------------|----------------|---------------------------|
| default | default | default | default |

#### VRFs

| VRF | Status |
| --- | ------ |
| mgmt | Enabled |

#### Management SSH Device Configuration

```eos
!
management ssh
   idle-timeout 30
   no shutdown
   !
   vrf mgmt
      no shutdown
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| mgmt | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf mgmt
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |
| df | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
username df privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 10.0.0.253:9910,10.0.0.252:9910,10.0.0.251:9910 | mgmt | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | - | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.0.0.253:9910,10.0.0.252:9910,10.0.0.251:9910 -cvauth=token,/tmp/token -cvvrf=mgmt -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs=
   no shutdown
```

### Logging

#### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |

| Format Type | Setting |
| ----------- | ------- |
| Hostname | fqdn |
| Sequence-numbers | false |
| RFC5424 | False |

| VRF | Source Interface |
| --- | ---------------- |

| VRF | Hosts | Ports | Protocol |
| --- | ----- | ----- | -------- |
| mgmt | 10.0.0.247 | Default | UDP |

#### Logging Servers and Features Device Configuration

```eos
!
logging vrf mgmt host 10.0.0.247
logging format hostname fqdn
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | - | All | Disabled |

#### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |
| Management1 | mgmt |

#### SNMP VRF Status

| VRF | Status |
| --- | ------ |
| mgmt | Enabled |

#### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| <removed> | ro | - | - | - |
| <removed> | rw | - | - | - |

#### SNMP Device Configuration

```eos
!
snmp-server vrf mgmt local-interface Management1
snmp-server community <removed> ro
snmp-server community <removed> rw
snmp-server vrf mgmt
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | Loopback0 | - | - |

sFlow Sample Rate: 1000

sFlow is enabled.

#### SFlow Device Configuration

```eos
!
sflow sample 1000
sflow destination 127.0.0.1
sflow source-interface Loopback0
sflow run
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| dc1-leafpair2 | Vlan4094 | 10.0.5.3 | Port-Channel551 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id dc1-leafpair2
   local-interface Vlan4094
   peer-address 10.0.5.3
   peer-link Port-Channel551
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 3700 | 3900 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 3700 3900
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 10 | ISP1 | - |
| 20 | ISP2 | - |
| 100 | InbandMgmt | - |
| 200 | Linknet Kubernetes FW | - |
| 201 | Kubernetes1 | - |
| 202 | Kubernetes2 | - |
| 300 | Linknet Proxmox FW | - |
| 301 | Proxmox1 | - |
| 302 | Proxmox2 | - |
| 3000 | MLAG_L3_VRF_InbandMgmt | MLAG |
| 3001 | MLAG_L3_VRF_Kubernetes | MLAG |
| 3002 | MLAG_L3_VRF_Proxmox | MLAG |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name ISP1
!
vlan 20
   name ISP2
!
vlan 100
   name InbandMgmt
!
vlan 200
   name Linknet Kubernetes FW
!
vlan 201
   name Kubernetes1
!
vlan 202
   name Kubernetes2
!
vlan 300
   name Linknet Proxmox FW
!
vlan 301
   name Proxmox1
!
vlan 302
   name Proxmox2
!
vlan 3000
   name MLAG_L3_VRF_InbandMgmt
   trunk group MLAG
!
vlan 3001
   name MLAG_L3_VRF_Kubernetes
   trunk group MLAG
!
vlan 3002
   name MLAG_L3_VRF_Proxmox
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## MAC Address Table

### MAC Address Table Summary

- MAC address table entry maximum age: 901 seconds

### MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 901
```

## Interfaces

### Interface Defaults

#### Interface Defaults Summary

- Default Ethernet Interface Shutdown: True

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   ethernet
      shutdown
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet55/1 | MLAG_dc1-leaf4_Ethernet55/1 | *trunk | *- | *- | *MLAG | 551 |
| Ethernet56/1 | MLAG_dc1-leaf4_Ethernet56/1 | *trunk | *- | *- | *MLAG | 551 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49/1 | P2P_dc1-spine1_Ethernet3/1 | - | 10.0.2.5/31 | default | 9100 | False | - | - |
| Ethernet50/1 | P2P_dc1-spine2_Ethernet3/1 | - | 10.0.2.7/31 | default | 9100 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet49/1
   description P2P_dc1-spine1_Ethernet3/1
   no shutdown
   mtu 9100
   no switchport
   ip address 10.0.2.5/31
!
interface Ethernet50/1
   description P2P_dc1-spine2_Ethernet3/1
   no shutdown
   mtu 9100
   no switchport
   ip address 10.0.2.7/31
!
interface Ethernet55/1
   description MLAG_dc1-leaf4_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_dc1-leaf4_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel551 | MLAG_dc1-leaf4_Port-Channel551 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel551
   description MLAG_dc1-leaf4_Port-Channel551
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.0.1.4/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.0.3.4/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.0.1.4/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.0.3.4/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan100 | InbandMgmt | InbandMgmt | 9000 | False |
| Vlan200 | Linknet Kubernetes FW | Kubernetes | 9000 | False |
| Vlan201 | Kubernetes1 | Kubernetes | 9000 | False |
| Vlan202 | Kubernetes2 | Kubernetes | 9000 | False |
| Vlan300 | Linknet Proxmox FW | Proxmox | 9000 | False |
| Vlan301 | Proxmox1 | Proxmox | 9000 | False |
| Vlan302 | Proxmox2 | Proxmox | 9000 | False |
| Vlan3000 | MLAG_L3_VRF_InbandMgmt | InbandMgmt | 9100 | False |
| Vlan3001 | MLAG_L3_VRF_Kubernetes | Kubernetes | 9100 | False |
| Vlan3002 | MLAG_L3_VRF_Proxmox | Proxmox | 9100 | False |
| Vlan4093 | MLAG_L3 | default | 9100 | False |
| Vlan4094 | MLAG | default | 9100 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan100 |  InbandMgmt  |  10.10.0.5/24  |  -  |  -  |  -  |  -  |
| Vlan200 |  Kubernetes  |  -  |  10.20.0.1/24  |  -  |  -  |  -  |
| Vlan201 |  Kubernetes  |  -  |  10.21.0.1/24  |  -  |  -  |  -  |
| Vlan202 |  Kubernetes  |  -  |  10.22.0.1/24  |  -  |  -  |  -  |
| Vlan300 |  Proxmox  |  -  |  10.30.0.1/24  |  -  |  -  |  -  |
| Vlan301 |  Proxmox  |  -  |  10.31.0.1/24  |  -  |  -  |  -  |
| Vlan302 |  Proxmox  |  -  |  10.32.0.1/24  |  -  |  -  |  -  |
| Vlan3000 |  InbandMgmt  |  10.0.4.2/31  |  -  |  -  |  -  |  -  |
| Vlan3001 |  Kubernetes  |  10.0.4.2/31  |  -  |  -  |  -  |  -  |
| Vlan3002 |  Proxmox  |  10.0.4.2/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.0.4.2/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.0.5.2/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan100
   description InbandMgmt
   no shutdown
   mtu 9000
   vrf InbandMgmt
   ip address 10.10.0.5/24
!
interface Vlan200
   description Linknet Kubernetes FW
   no shutdown
   mtu 9000
   vrf Kubernetes
   ip address virtual 10.20.0.1/24
!
interface Vlan201
   description Kubernetes1
   no shutdown
   mtu 9000
   vrf Kubernetes
   ip helper-address 10.20.0.250
   ip address virtual 10.21.0.1/24
!
interface Vlan202
   description Kubernetes2
   no shutdown
   mtu 9000
   vrf Kubernetes
   ip helper-address 10.20.0.250
   ip address virtual 10.22.0.1/24
!
interface Vlan300
   description Linknet Proxmox FW
   no shutdown
   mtu 9000
   vrf Proxmox
   ip address virtual 10.30.0.1/24
!
interface Vlan301
   description Proxmox1
   no shutdown
   mtu 9000
   vrf Proxmox
   ip helper-address 10.30.0.250
   ip address virtual 10.31.0.1/24
!
interface Vlan302
   description Proxmox2
   no shutdown
   mtu 9000
   vrf Proxmox
   ip helper-address 10.30.0.250
   ip address virtual 10.32.0.1/24
!
interface Vlan3000
   description MLAG_L3_VRF_InbandMgmt
   no shutdown
   mtu 9100
   vrf InbandMgmt
   ip address 10.0.4.2/31
!
interface Vlan3001
   description MLAG_L3_VRF_Kubernetes
   no shutdown
   mtu 9100
   vrf Kubernetes
   ip address 10.0.4.2/31
!
interface Vlan3002
   description MLAG_L3_VRF_Proxmox
   no shutdown
   mtu 9100
   vrf Proxmox
   ip address 10.0.4.2/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 9100
   ip address 10.0.4.2/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 9100
   no autostate
   ip address 10.0.5.2/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 10 | 10010 | - | - |
| 20 | 10020 | - | - |
| 100 | 10100 | - | - |
| 200 | 10200 | - | - |
| 201 | 10201 | - | - |
| 202 | 10202 | - | - |
| 300 | 10300 | - | - |
| 301 | 10301 | - | - |
| 302 | 10302 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| InbandMgmt | 1 | - |
| Kubernetes | 2 | - |
| Proxmox | 3 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc1-leaf3_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 20 vni 10020
   vxlan vlan 100 vni 10100
   vxlan vlan 200 vni 10200
   vxlan vlan 201 vni 10201
   vxlan vlan 202 vni 10202
   vxlan vlan 300 vni 10300
   vxlan vlan 301 vni 10301
   vxlan vlan 302 vni 10302
   vxlan vrf InbandMgmt vni 1
   vxlan vrf Kubernetes vni 2
   vxlan vrf Proxmox vni 3
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:11:22:33:44:55

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:11:22:33:44:55
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| InbandMgmt | True |
| Kubernetes | True |
| mgmt | True |
| Proxmox | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf InbandMgmt
ip routing vrf Kubernetes
ip routing vrf mgmt
ip routing vrf Proxmox
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| InbandMgmt | false |
| Kubernetes | false |
| mgmt | false |
| Proxmox | false |

### ARP

Global ARP timeout: 900

#### ARP Device Configuration

```eos
!
arp aging timeout default 900
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65002 | 10.0.1.4 |

| BGP Tuning |
| ---------- |
| distance bgp 150 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| update wait-install |
| no bgp default ipv4-unicast |
| maximum-paths 2 ecmp 2 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 4 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65002 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.0.1.2 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.3 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.2.4 | 65000 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.0.2.6 | 65000 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.0.4.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.0.4.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | InbandMgmt | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.0.4.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Kubernetes | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.0.4.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Proxmox | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 10.0.1.4:10010 | 65000:10010 | - | - | learned |
| 20 | 10.0.1.4:10020 | 65000:10020 | - | - | learned |
| 100 | 10.0.1.4:10100 | 65000:10100 | - | - | learned |
| 200 | 10.0.1.4:10200 | 65000:10200 | - | - | learned |
| 201 | 10.0.1.4:10201 | 65000:10201 | - | - | learned |
| 202 | 10.0.1.4:10202 | 65000:10202 | - | - | learned |
| 300 | 10.0.1.4:10300 | 65000:10300 | - | - | learned |
| 301 | 10.0.1.4:10301 | 65000:10301 | - | - | learned |
| 302 | 10.0.1.4:10302 | 65000:10302 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| InbandMgmt | 10.0.1.4:1 | connected |
| Kubernetes | 10.0.1.4:2 | connected |
| Proxmox | 10.0.1.4:3 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65002
   router-id 10.0.1.4
   update wait-install
   no bgp default ipv4-unicast
   maximum-paths 2 ecmp 2
   distance bgp 150 200 200
   graceful-restart restart-time 300
   graceful-restart
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 4
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65002
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc1-leaf4
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.0.1.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.2 remote-as 65001
   neighbor 10.0.1.2 description dc1-leaf1_Loopback0
   neighbor 10.0.1.2 route-map RM-EVPN-FILTER-AS65001 out
   neighbor 10.0.1.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.3 remote-as 65001
   neighbor 10.0.1.3 description dc1-leaf2_Loopback0
   neighbor 10.0.1.3 route-map RM-EVPN-FILTER-AS65001 out
   neighbor 10.0.2.4 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.2.4 remote-as 65000
   neighbor 10.0.2.4 description dc1-spine1_Ethernet3/1
   neighbor 10.0.2.6 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.2.6 remote-as 65000
   neighbor 10.0.2.6 description dc1-spine2_Ethernet3/1
   neighbor 10.0.4.3 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.0.4.3 description dc1-leaf4_Vlan4093
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 10.0.1.4:10010
      route-target both 65000:10010
      redistribute learned
   !
   vlan 20
      rd 10.0.1.4:10020
      route-target both 65000:10020
      redistribute learned
   !
   vlan 100
      rd 10.0.1.4:10100
      route-target both 65000:10100
      redistribute learned
   !
   vlan 200
      rd 10.0.1.4:10200
      route-target both 65000:10200
      redistribute learned
   !
   vlan 201
      rd 10.0.1.4:10201
      route-target both 65000:10201
      redistribute learned
   !
   vlan 202
      rd 10.0.1.4:10202
      route-target both 65000:10202
      redistribute learned
   !
   vlan 300
      rd 10.0.1.4:10300
      route-target both 65000:10300
      redistribute learned
   !
   vlan 301
      rd 10.0.1.4:10301
      route-target both 65000:10301
      redistribute learned
   !
   vlan 302
      rd 10.0.1.4:10302
      route-target both 65000:10302
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf InbandMgmt
      rd 10.0.1.4:1
      route-target import evpn 65000:1
      route-target export evpn 65000:1
      router-id 10.0.1.4
      update wait-install
      neighbor 10.0.4.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.0.4.3 description dc1-leaf4_Vlan3000
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf Kubernetes
      rd 10.0.1.4:2
      route-target import evpn 65000:2
      route-target export evpn 65000:2
      router-id 10.0.1.4
      update wait-install
      neighbor 10.0.4.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.0.4.3 description dc1-leaf4_Vlan3001
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf Proxmox
      rd 10.0.1.4:3
      route-target import evpn 65000:3
      route-target export evpn 65000:3
      router-id 10.0.1.4
      update wait-install
      neighbor 10.0.4.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.0.4.3 description dc1-leaf4_Vlan3002
      redistribute connected route-map RM-CONN-2-BGP-VRFS
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 5000 | 5000 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 5000 min-rx 5000 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.0.1.0/24 eq 32 |
| 20 | permit 10.0.3.0/24 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.0.4.2/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.0.1.0/24 eq 32
   seq 20 permit 10.0.3.0/24 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.0.4.2/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-EVPN-FILTER-AS65001

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | as 65001 | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-EVPN-FILTER-AS65001 deny 10
   match as 65001
!
route-map RM-EVPN-FILTER-AS65001 permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| InbandMgmt | enabled |
| Kubernetes | enabled |
| mgmt | enabled |
| Proxmox | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance InbandMgmt
!
vrf instance Kubernetes
!
vrf instance mgmt
!
vrf instance Proxmox
```

## Errdisable

### Errdisable Summary

|  Detect Cause | Enabled |
| ------------- | ------- |
| acl | True |
| link-change | True |
| tapagg | True |
| xcvr-misconfigured | True |
| xcvr-overheat | True |
| xcvr-power-unsupported | True |

|  Detect Cause | Enabled | Interval |
| ------------- | ------- | -------- |
| bpduguard | True | 30 |
| hitless-reload-down | True | 30 |
| lacp-rate-limit | True | 30 |
| link-flap | True | 30 |
| no-internal-vlan | True | 30 |
| portchannelguard | True | 30 |
| portsec | True | 30 |
| speed-misconfigured | True | 30 |
| tapagg | True | 30 |
| uplink-failure-detection | True | 30 |
| xcvr-misconfigured | True | 30 |
| xcvr-overheat | True | 30 |
| xcvr-power-unsupported | True | 30 |
| xcvr-unsupported | True | 30 |

```eos
!
errdisable detect cause acl
errdisable detect cause link-change
errdisable detect cause tapagg
errdisable detect cause xcvr-misconfigured
errdisable detect cause xcvr-overheat
errdisable detect cause xcvr-power-unsupported
errdisable recovery cause bpduguard
errdisable recovery cause hitless-reload-down
errdisable recovery cause lacp-rate-limit
errdisable recovery cause link-flap
errdisable recovery cause no-internal-vlan
errdisable recovery cause portchannelguard
errdisable recovery cause portsec
errdisable recovery cause speed-misconfigured
errdisable recovery cause tapagg
errdisable recovery cause uplink-failure-detection
errdisable recovery cause xcvr-misconfigured
errdisable recovery cause xcvr-overheat
errdisable recovery cause xcvr-power-unsupported
errdisable recovery cause xcvr-unsupported
errdisable recovery interval 30
```
