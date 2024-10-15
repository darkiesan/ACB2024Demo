# dc2-leaf3

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
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
- [Interfaces](#interfaces)
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
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.0.0.11/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.0.0.11/24
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| dc2-leafpair2 | Vlan4094 | 10.10.0.3 | Port-Channel531 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id dc2-leafpair2
   local-interface Vlan4094
   peer-address 10.10.0.3
   peer-link Port-Channel531
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
| 10 | VLAN_10 | - |
| 100 | VLAN_100 | - |
| 101 | VLAN_101 | - |
| 200 | VLAN_200 | - |
| 201 | VLAN_201 | - |
| 3000 | MLAG_iBGP_VRF_A | LEAF_PEER_L3 |
| 3001 | MLAG_iBGP_VRF_B | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name VLAN_10
!
vlan 100
   name VLAN_100
!
vlan 101
   name VLAN_101
!
vlan 200
   name VLAN_200
!
vlan 201
   name VLAN_201
!
vlan 3000
   name MLAG_iBGP_VRF_A
   trunk group LEAF_PEER_L3
!
vlan 3001
   name MLAG_iBGP_VRF_B
   trunk group LEAF_PEER_L3
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet53/1 | MLAG_PEER_dc2-leaf4_Ethernet53/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 531 |
| Ethernet54/1 | MLAG_PEER_dc2-leaf4_Ethernet54/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 531 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49/1 | P2P_LINK_TO_DC2-SPINE1_Ethernet3/1 | routed | - | 10.7.0.5/31 | default | 9100 | False | - | - |
| Ethernet50/1 | P2P_LINK_TO_DC2-SPINE2_Ethernet3/1 | routed | - | 10.7.0.7/31 | default | 9100 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet49/1
   description P2P_LINK_TO_DC2-SPINE1_Ethernet3/1
   no shutdown
   mtu 9100
   no switchport
   ip address 10.7.0.5/31
!
interface Ethernet50/1
   description P2P_LINK_TO_DC2-SPINE2_Ethernet3/1
   no shutdown
   mtu 9100
   no switchport
   ip address 10.7.0.7/31
!
interface Ethernet53/1
   description MLAG_PEER_dc2-leaf4_Ethernet53/1
   no shutdown
   channel-group 531 mode active
!
interface Ethernet54/1
   description MLAG_PEER_dc2-leaf4_Ethernet54/1
   no shutdown
   channel-group 531 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel531 | MLAG_PEER_dc2-leaf4_Po531 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel531
   description MLAG_PEER_dc2-leaf4_Po531
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.6.0.4/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.7.0.4/32 |
| Loopback100 | VRF_A_VTEP_DIAGNOSTICS | VRF_A | 10.1.255.4/32 |
| Loopback200 | VRF_B_VTEP_DIAGNOSTICS | VRF_B | 10.2.255.4/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback100 | VRF_A_VTEP_DIAGNOSTICS | VRF_A | - |
| Loopback200 | VRF_B_VTEP_DIAGNOSTICS | VRF_B | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.6.0.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.7.0.4/32
!
interface Loopback100
   description VRF_A_VTEP_DIAGNOSTICS
   no shutdown
   vrf VRF_A
   ip address 10.1.255.4/32
!
interface Loopback200
   description VRF_B_VTEP_DIAGNOSTICS
   no shutdown
   vrf VRF_B
   ip address 10.2.255.4/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan100 | VLAN_100 | VRF_A | - | False |
| Vlan101 | VLAN_101 | VRF_A | - | False |
| Vlan200 | VLAN_200 | VRF_B | - | False |
| Vlan201 | VLAN_201 | VRF_B | - | False |
| Vlan3000 | MLAG_PEER_L3_iBGP: vrf VRF_A | VRF_A | 9100 | False |
| Vlan3001 | MLAG_PEER_L3_iBGP: vrf VRF_B | VRF_B | 9100 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 9100 | False |
| Vlan4094 | MLAG_PEER | default | 9100 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan100 |  VRF_A  |  -  |  10.1.100.1/24  |  -  |  -  |  -  |  -  |
| Vlan101 |  VRF_A  |  -  |  10.1.101.1/24  |  -  |  -  |  -  |  -  |
| Vlan200 |  VRF_B  |  -  |  10.2.200.1/24  |  -  |  -  |  -  |  -  |
| Vlan201 |  VRF_B  |  -  |  10.2.201.1/24  |  -  |  -  |  -  |  -  |
| Vlan3000 |  VRF_A  |  10.9.0.2/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3001 |  VRF_B  |  10.9.0.2/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.9.0.2/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.10.0.2/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan100
   description VLAN_100
   no shutdown
   vrf VRF_A
   ip address virtual 10.1.100.1/24
!
interface Vlan101
   description VLAN_101
   no shutdown
   vrf VRF_A
   ip address virtual 10.1.101.1/24
!
interface Vlan200
   description VLAN_200
   no shutdown
   vrf VRF_B
   ip address virtual 10.2.200.1/24
!
interface Vlan201
   description VLAN_201
   no shutdown
   vrf VRF_B
   ip address virtual 10.2.201.1/24
!
interface Vlan3000
   description MLAG_PEER_L3_iBGP: vrf VRF_A
   no shutdown
   mtu 9100
   vrf VRF_A
   ip address 10.9.0.2/31
!
interface Vlan3001
   description MLAG_PEER_L3_iBGP: vrf VRF_B
   no shutdown
   mtu 9100
   vrf VRF_B
   ip address 10.9.0.2/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 9100
   ip address 10.9.0.2/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9100
   no autostate
   ip address 10.10.0.2/31
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
| 100 | 10100 | - | - |
| 101 | 10101 | - | - |
| 200 | 10200 | - | - |
| 201 | 10201 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| VRF_A | 1 | - |
| VRF_B | 2 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc2-leaf3_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 100 vni 10100
   vxlan vlan 101 vni 10101
   vxlan vlan 200 vni 10200
   vxlan vlan 201 vni 10201
   vxlan vrf VRF_A vni 1
   vxlan vrf VRF_B vni 2
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
| MGMT | False |
| VRF_A | True |
| VRF_B | True |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf VRF_A
ip routing vrf VRF_B
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |
| VRF_A | false |
| VRF_B | false |

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
| 65102 | 10.6.0.4 |

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
| Remote AS | 65102 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.6.0.0 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.6.0.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.7.0.4 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.7.0.6 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.9.0.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.9.0.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF_A | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.9.0.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF_B | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 10.6.0.4:10010 | 65000:10010 | - | - | learned |
| 100 | 10.6.0.4:10100 | 65000:10100 | - | - | learned |
| 101 | 10.6.0.4:10101 | 65000:10101 | - | - | learned |
| 200 | 10.6.0.4:10200 | 65000:10200 | - | - | learned |
| 201 | 10.6.0.4:10201 | 65000:10201 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| VRF_A | 10.6.0.4:1 | connected |
| VRF_B | 10.6.0.4:2 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 10.6.0.4
   maximum-paths 2 ecmp 2
   update wait-install
   no bgp default ipv4-unicast
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
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65102
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc2-leaf4
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.6.0.0 peer group EVPN-OVERLAY-PEERS
   neighbor 10.6.0.0 remote-as 65100
   neighbor 10.6.0.0 description dc2-spine1
   neighbor 10.6.0.0 route-map RM-EVPN-FILTER-AS65100 out
   neighbor 10.6.0.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.6.0.1 remote-as 65100
   neighbor 10.6.0.1 description dc2-spine2
   neighbor 10.6.0.1 route-map RM-EVPN-FILTER-AS65100 out
   neighbor 10.7.0.4 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.7.0.4 remote-as 65100
   neighbor 10.7.0.4 description dc2-spine1_Ethernet3/1
   neighbor 10.7.0.6 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.7.0.6 remote-as 65100
   neighbor 10.7.0.6 description dc2-spine2_Ethernet3/1
   neighbor 10.9.0.3 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.9.0.3 description dc2-leaf4
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 10.6.0.4:10010
      route-target both 65000:10010
      redistribute learned
   !
   vlan 100
      rd 10.6.0.4:10100
      route-target both 65000:10100
      redistribute learned
   !
   vlan 101
      rd 10.6.0.4:10101
      route-target both 65000:10101
      redistribute learned
   !
   vlan 200
      rd 10.6.0.4:10200
      route-target both 65000:10200
      redistribute learned
   !
   vlan 201
      rd 10.6.0.4:10201
      route-target both 65000:10201
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
   vrf VRF_A
      rd 10.6.0.4:1
      route-target import evpn 65000:1
      route-target export evpn 65000:1
      router-id 10.6.0.4
      update wait-install
      neighbor 10.9.0.3 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf VRF_B
      rd 10.6.0.4:2
      route-target import evpn 65000:2
      route-target export evpn 65000:2
      router-id 10.6.0.4
      update wait-install
      neighbor 10.9.0.3 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
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
| 10 | permit 10.6.0.0/24 eq 32 |
| 20 | permit 10.7.0.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.6.0.0/24 eq 32
   seq 20 permit 10.7.0.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-EVPN-FILTER-AS65100

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | as 65100 | - | - | - |
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
route-map RM-EVPN-FILTER-AS65100 deny 10
   match as 65100
!
route-map RM-EVPN-FILTER-AS65100 permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| VRF_A | enabled |
| VRF_B | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance VRF_A
!
vrf instance VRF_B
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| VRF_A | 10.1.255.4 |
| VRF_B | 10.2.255.4 |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf VRF_A address 10.1.255.4
ip address virtual source-nat vrf VRF_B address 10.2.255.4
```
