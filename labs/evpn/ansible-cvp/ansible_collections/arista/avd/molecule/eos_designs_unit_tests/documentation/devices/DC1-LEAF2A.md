# DC1-LEAF2A
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Name Servers](#name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [System Boot Settings](#system-boot-settings)
  - [Boot Secret Summary](#boot-secret-summary)
  - [System Boot Configuration](#system-boot-configuration)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SNMP](#snmp)
- [Hardware TCAM Profile](#hardware-tcam-profile)
  - [Hardware TCAM configuration](#hardware-tcam-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
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
  - [Static Routes](#static-routes)
  - [IPv6 Static Routes](#ipv6-static-routes)
  - [Router OSPF](#router-ospf)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [Platform](#platform)
  - [Platform Summary](#platform-summary)
  - [Platform Configuration](#platform-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.200.106/24 | 192.168.200.5 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.200.106/24
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 192.168.200.5 | MGMT |
| 8.8.8.8 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 192.168.200.5
```

## NTP

### NTP Summary

#### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.200.5 | MGMT | True | - | - | - | - | - | - | - |

### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT 192.168.200.5 prefer
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | False |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no default-services
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| admin | 15 | network-admin |
| cvpadmin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username cvpadmin privilege 15 role network-admin secret sha512 $6$rZKcbIZ7iWGAWTUM$TCgDn1KcavS0s.OV8lacMTUkxTByfzcGlFlYUWroxYuU7M/9bIodhRO7nXGzMweUxvbk8mJmQl8Bh44cRktUj.
username cvpadmin ssh-key ssh-rsa AAAAB3NzaC1yc2EAA82spi2mkxp4FgaLi4CjWkpnL1A/MD7WhrSNgqXToF7QCb9Lidagy9IHafQxfu7LwkFdyQIMu8XNwDZIycuf29wHbDdz1N+YNVK8zwyNAbMOeKMqblsEm2YIorgjzQX1m9+/rJeFBKz77PSgeMp/Rc3txFVuSmFmeTy3aMkU= cvpadmin@hostmachine.local
```

# System Boot Settings

## Boot Secret Summary

- The sha512 hashed Aboot password is configured

## System Boot Configuration

```eos
!
boot secret sha512 a153de6290ff1409257ade45f
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.200.11:9910 | MGMT | key,telarista | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.200.11:9910 -cvauth=key,telarista -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| example@example.com | EOS_DESIGNS_UNIT_TESTS rackC DC1-LEAF2A | All | Disabled |

### SNMP Device Configuration

```eos
!
snmp-server contact example@example.com
snmp-server location EOS_DESIGNS_UNIT_TESTS rackC DC1-LEAF2A
```

# Hardware TCAM Profile

TCAM profile __`vxlan-routing`__ is active

## Hardware TCAM configuration

```eos
!
hardware tcam
   system profile vxlan-routing
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

STP Root Super: **True**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

## Spanning Tree Device Configuration

```eos
!
spanning-tree root super
spanning-tree mode mstp
spanning-tree mst 0 priority 4096
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 110 | Tenant_A_OP_Zone_1 | - |
| 111 | Tenant_A_OP_Zone_2 | - |
| 112 | Tenant_A_OP_Zone_3 | - |
| 120 | Tenant_A_WEB_Zone_1 | - |
| 121 | Tenant_A_WEBZone_2 | - |
| 130 | Tenant_A_APP_Zone_1 | - |
| 131 | Tenant_A_APP_Zone_2 | - |
| 140 | Tenant_A_DB_BZone_1 | - |
| 141 | Tenant_A_DB_Zone_2 | - |
| 160 | Tenant_A_VMOTION | - |
| 161 | Tenant_A_NFS | - |
| 210 | Tenant_B_OP_Zone_1 | - |
| 211 | Tenant_B_OP_Zone_2 | - |
| 310 | Tenant_C_OP_Zone_1 | - |
| 311 | Tenant_C_OP_Zone_2 | - |
| 410 | Tenant_D_v6_OP_Zone_1 | - |
| 411 | Tenant_D_v6_OP_Zone_2 | - |
| 412 | Tenant_D_v6_OP_Zone_1 | - |
| 450 | Tenant_D_v6_WAN_Zone_1 | - |
| 451 | Tenant_D_v6_WAN_Zone_2 | - |
| 452 | Tenant_D_v6_WAN_Zone_3 | - |

## VLANs Device Configuration

```eos
!
vlan 110
   name Tenant_A_OP_Zone_1
!
vlan 111
   name Tenant_A_OP_Zone_2
!
vlan 112
   name Tenant_A_OP_Zone_3
!
vlan 120
   name Tenant_A_WEB_Zone_1
!
vlan 121
   name Tenant_A_WEBZone_2
!
vlan 130
   name Tenant_A_APP_Zone_1
!
vlan 131
   name Tenant_A_APP_Zone_2
!
vlan 140
   name Tenant_A_DB_BZone_1
!
vlan 141
   name Tenant_A_DB_Zone_2
!
vlan 160
   name Tenant_A_VMOTION
!
vlan 161
   name Tenant_A_NFS
!
vlan 210
   name Tenant_B_OP_Zone_1
!
vlan 211
   name Tenant_B_OP_Zone_2
!
vlan 310
   name Tenant_C_OP_Zone_1
!
vlan 311
   name Tenant_C_OP_Zone_2
!
vlan 410
   name Tenant_D_v6_OP_Zone_1
!
vlan 411
   name Tenant_D_v6_OP_Zone_2
!
vlan 412
   name Tenant_D_v6_OP_Zone_1
!
vlan 450
   name Tenant_D_v6_WAN_Zone_1
!
vlan 451
   name Tenant_D_v6_WAN_Zone_2
!
vlan 452
   name Tenant_D_v6_WAN_Zone_3
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet7 | DC1-L2LEAF1A_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 7 |
| Ethernet8 | DC1-L2LEAF1B_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 7 |
| Ethernet9 | DC1-L2LEAF3A_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 9 |
| Ethernet10 | server01_MLAG_Eth2 | *trunk | *210-211 | *- | *- | 10 |
| Ethernet11 | server01_MTU_PROFILE_MLAG_Eth4 | *access | *110 | *- | *- | 11 |
| Ethernet12 | server01_MTU_ADAPTOR_MLAG_Eth6 | *access | *- | *- | *- | 12 |
| Ethernet13 | DC1-L2LEAF4A_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 13 |
| Ethernet14/1 | DC1-L2LEAF5A_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 141 |
| Ethernet15/1 | DC1-L2LEAF5B_Ethernet1 | *trunk | *110-112,120-121,130-131,160-161 | *- | *- | 141 |
| Ethernet20 | FIREWALL01_E0 | *trunk | *110-111,210-211 | *- | *- | 20 |
| Ethernet21 |  ROUTER01_Eth0 | access | 110 | - | - | - |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet22 | - | routed | - | 10.0.0.1/30 | Tenant_OSPF | - | false | - | - |
| Ethernet23 | - | routed | - | 10.0.0.13/30 | Tenant_OSPF | - | false | - | - |
| Ethernet49/1 | P2P_LINK_TO_DC1-SPINE1_Ethernet3/1 | routed | - | 172.31.254.33/31 | default | 1500 | false | - | - |
| Ethernet50/1 | P2P_LINK_TO_DC1-SPINE2_Ethernet1/3/1 | routed | - | 172.31.254.35/31 | default | 1500 | false | - | - |
| Ethernet51/1 | P2P_LINK_TO_DC1-SPINE2_Ethernet1/4/1 | routed | - | 172.31.254.37/31 | default | 1500 | false | - | - |
| Ethernet52/1 | P2P_LINK_TO_DC1-SPINE1_Ethernet4/1 | routed | - | 172.31.254.39/31 | default | 1500 | false | - | - |
| Ethernet53/1 | P2P_LINK_TO_DC1-SPINE3_Ethernet1/3/1 | routed | - | 172.31.254.41/31 | default | 1500 | false | - | - |
| Ethernet54/1 | P2P_LINK_TO_DC1-SPINE4_Ethernet3/1 | routed | - | 172.31.254.43/31 | default | 1500 | false | - | - |
| Ethernet55/1 | P2P_LINK_TO_DC1-SPINE4_Ethernet4/1 | routed | - | 172.31.254.45/31 | default | 1500 | false | - | - |
| Ethernet56/1 | P2P_LINK_TO_DC1-SPINE3_Ethernet1/4/1 | routed | - | 172.31.254.47/31 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet7
   description DC1-L2LEAF1A_Ethernet1
   no shutdown
   channel-group 7 mode active
!
interface Ethernet8
   description DC1-L2LEAF1B_Ethernet1
   no shutdown
   channel-group 7 mode active
!
interface Ethernet9
   description DC1-L2LEAF3A_Ethernet1
   no shutdown
   channel-group 9 mode active
!
interface Ethernet10
   description server01_MLAG_Eth2
   no shutdown
   channel-group 10 mode active
!
interface Ethernet11
   description server01_MTU_PROFILE_MLAG_Eth4
   no shutdown
   channel-group 11 mode active
!
interface Ethernet12
   description server01_MTU_ADAPTOR_MLAG_Eth6
   no shutdown
   channel-group 12 mode active
!
interface Ethernet13
   description DC1-L2LEAF4A_Ethernet1
   no shutdown
   channel-group 13 mode active
!
interface Ethernet14/1
   description DC1-L2LEAF5A_Ethernet1
   no shutdown
   channel-group 141 mode active
!
interface Ethernet15/1
   description DC1-L2LEAF5B_Ethernet1
   no shutdown
   channel-group 141 mode active
!
interface Ethernet20
   description FIREWALL01_E0
   no shutdown
   channel-group 20 mode active
!
interface Ethernet21
   description ROUTER01_Eth0
   no shutdown
   switchport access vlan 110
   switchport mode access
   switchport
!
interface Ethernet22
   no shutdown
   no switchport
   vrf Tenant_OSPF
   ip address 10.0.0.1/30
   ip ospf network point-to-point
   ip ospf area 0
!
interface Ethernet23
   no shutdown
   no switchport
   vrf Tenant_OSPF
   ip address 10.0.0.13/30
   ip ospf network point-to-point
   ip ospf area 0
!
interface Ethernet49/1
   description P2P_LINK_TO_DC1-SPINE1_Ethernet3/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.33/31
!
interface Ethernet50/1
   description P2P_LINK_TO_DC1-SPINE2_Ethernet1/3/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.35/31
!
interface Ethernet51/1
   description P2P_LINK_TO_DC1-SPINE2_Ethernet1/4/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.37/31
!
interface Ethernet52/1
   description P2P_LINK_TO_DC1-SPINE1_Ethernet4/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.39/31
!
interface Ethernet53/1
   description P2P_LINK_TO_DC1-SPINE3_Ethernet1/3/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.41/31
!
interface Ethernet54/1
   description P2P_LINK_TO_DC1-SPINE4_Ethernet3/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.43/31
!
interface Ethernet55/1
   description P2P_LINK_TO_DC1-SPINE4_Ethernet4/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.45/31
!
interface Ethernet56/1
   description P2P_LINK_TO_DC1-SPINE3_Ethernet1/4/1
   no shutdown
   mtu 1500
   speed forced 100gfull
   no switchport
   ip address 172.31.254.47/31
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel7 | DC1_L2LEAF1_Po1 | switched | trunk | 110-112,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:0808:0707:0606 |
| Port-Channel9 | DC1-L2LEAF3A_Po1 | switched | trunk | 110-112,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:0606:0707:0808 |
| Port-Channel10 | server01_MLAG_PortChanne1 | switched | trunk | 210-211 | - | - | - | - | - | - |
| Port-Channel11 | server01_MTU_PROFILE_MLAG_PortChanne1 | switched | access | 110 | - | - | - | - | - | - |
| Port-Channel12 | server01_MTU_ADAPTOR_MLAG_PortChanne1 | switched | access | - | - | - | - | - | - | - |
| Port-Channel13 | DC1-L2LEAF4A_Po1 | switched | trunk | 110-112,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:a36b:7013:457b |
| Port-Channel20 | FIREWALL01_PortChanne1 | switched | trunk | 110-111,210-211 | - | - | - | - | - | - |
| Port-Channel141 | DC1_L2LEAF5_Po1 | switched | trunk | 110-112,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:fa91:ce62:ce95 |

#### EVPN Multihoming

##### EVPN Multihoming Summary

| Interface | Ethernet Segment Identifier | Multihoming Redundancy Mode | Route Target |
| --------- | --------------------------- | --------------------------- | ------------ |
| Port-Channel7 | 0000:0000:0808:0707:0606 | all-active | 08:08:07:07:06:06 |
| Port-Channel9 | 0000:0000:0606:0707:0808 | all-active | 06:06:07:07:08:08 |
| Port-Channel13 | 0000:0000:a36b:7013:457b | all-active | a3:6b:70:13:45:7b |
| Port-Channel141 | 0000:0000:fa91:ce62:ce95 | all-active | fa:91:ce:62:ce:95 |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel7
   description DC1_L2LEAF1_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-112,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:0808:0707:0606
      route-target import 08:08:07:07:06:06
   lacp system-id 0808.0707.0606
!
interface Port-Channel9
   description DC1-L2LEAF3A_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-112,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:0606:0707:0808
      route-target import 06:06:07:07:08:08
   lacp system-id 0606.0707.0808
!
interface Port-Channel10
   description server01_MLAG_PortChanne1
   no shutdown
   switchport
   switchport trunk allowed vlan 210-211
   switchport mode trunk
   spanning-tree bpduguard disable
   spanning-tree bpdufilter disable
!
interface Port-Channel11
   description server01_MTU_PROFILE_MLAG_PortChanne1
   no shutdown
   mtu 1600
   switchport
   switchport access vlan 110
   spanning-tree bpduguard enable
   spanning-tree bpdufilter enable
!
interface Port-Channel12
   description server01_MTU_ADAPTOR_MLAG_PortChanne1
   no shutdown
   mtu 1601
   switchport
   spanning-tree bpduguard enable
   spanning-tree bpdufilter enable
!
interface Port-Channel13
   description DC1-L2LEAF4A_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-112,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:a36b:7013:457b
      route-target import a3:6b:70:13:45:7b
   lacp system-id a36b.7013.457b
!
interface Port-Channel20
   description FIREWALL01_PortChanne1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-111,210-211
   switchport mode trunk
!
interface Port-Channel141
   description DC1_L2LEAF5_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-112,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:fa91:ce62:ce95
      route-target import fa:91:ce:62:ce:95
   lacp system-id fa91.ce62.ce95
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 192.168.255.10/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 192.168.254.10/32 |
| Loopback100 | Tenant_A_OP_Zone_VTEP_DIAGNOSTICS | Tenant_A_OP_Zone | 10.255.1.10/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback100 | Tenant_A_OP_Zone_VTEP_DIAGNOSTICS | Tenant_A_OP_Zone | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 192.168.255.10/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 192.168.254.10/32
!
interface Loopback100
   description Tenant_A_OP_Zone_VTEP_DIAGNOSTICS
   no shutdown
   vrf Tenant_A_OP_Zone
   ip address 10.255.1.10/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan110 | Tenant_A_OP_Zone_1 | Tenant_A_OP_Zone | - | false |
| Vlan111 | Tenant_A_OP_Zone_2 | Tenant_A_OP_Zone | - | false |
| Vlan112 | Tenant_A_OP_Zone_3 | Tenant_A_OP_Zone | 1560 | false |
| Vlan120 | Tenant_A_WEB_Zone_1 | Tenant_A_WEB_Zone | - | false |
| Vlan121 | Tenant_A_WEBZone_2 | Tenant_A_WEB_Zone | 1560 | true |
| Vlan130 | Tenant_A_APP_Zone_1 | Tenant_A_APP_Zone | - | false |
| Vlan131 | Tenant_A_APP_Zone_2 | Tenant_A_APP_Zone | - | false |
| Vlan140 | Tenant_A_DB_BZone_1 | Tenant_A_DB_Zone | - | false |
| Vlan141 | Tenant_A_DB_Zone_2 | Tenant_A_DB_Zone | - | false |
| Vlan210 | Tenant_B_OP_Zone_1 | Tenant_B_OP_Zone | - | false |
| Vlan211 | Tenant_B_OP_Zone_2 | Tenant_B_OP_Zone | - | false |
| Vlan310 | Tenant_C_OP_Zone_1 | Tenant_C_OP_Zone | - | false |
| Vlan311 | Tenant_C_OP_Zone_2 | Tenant_C_OP_Zone | - | false |
| Vlan410 | Tenant_D_v6_OP_Zone_1 | Tenant_D_OP_Zone | - | false |
| Vlan411 | Tenant_D_v6_OP_Zone_2 | Tenant_D_OP_Zone | - | false |
| Vlan412 | Tenant_D_v6_OP_Zone_1 | Tenant_D_OP_Zone | 1560 | false |
| Vlan450 | Tenant_D_v6_WAN_Zone_1 | Tenant_D_WAN_Zone | - | false |
| Vlan451 | Tenant_D_v6_WAN_Zone_2 | Tenant_D_WAN_Zone | 1560 | false |
| Vlan452 | Tenant_D_v6_WAN_Zone_3 | Tenant_D_WAN_Zone | 1560 | false |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan110 |  Tenant_A_OP_Zone  |  -  |  10.1.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan111 |  Tenant_A_OP_Zone  |  -  |  10.1.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan112 |  Tenant_A_OP_Zone  |  -  |  -  |  -  |  -  |  -  |  -  |
| Vlan120 |  Tenant_A_WEB_Zone  |  -  |  10.1.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan121 |  Tenant_A_WEB_Zone  |  -  |  10.1.10.254/24  |  -  |  -  |  -  |  -  |
| Vlan130 |  Tenant_A_APP_Zone  |  -  |  10.1.30.1/24  |  -  |  -  |  -  |  -  |
| Vlan131 |  Tenant_A_APP_Zone  |  -  |  10.1.31.1/24  |  -  |  -  |  -  |  -  |
| Vlan140 |  Tenant_A_DB_Zone  |  -  |  10.1.40.1/24  |  -  |  -  |  -  |  -  |
| Vlan141 |  Tenant_A_DB_Zone  |  -  |  10.1.41.1/24  |  -  |  -  |  -  |  -  |
| Vlan210 |  Tenant_B_OP_Zone  |  -  |  10.2.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan211 |  Tenant_B_OP_Zone  |  -  |  10.2.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan310 |  Tenant_C_OP_Zone  |  -  |  10.3.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan311 |  Tenant_C_OP_Zone  |  -  |  10.3.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan410 |  Tenant_D_OP_Zone  |  -  |  10.3.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan411 |  Tenant_D_OP_Zone  |  10.3.11.2/24  |  -  |  10.3.11.1/24  |  -  |  -  |  -  |
| Vlan412 |  Tenant_D_OP_Zone  |  -  |  10.4.12.254/24  |  -  |  -  |  -  |  -  |
| Vlan450 |  Tenant_D_WAN_Zone  |  -  |  -  |  -  |  -  |  -  |  -  |
| Vlan451 |  Tenant_D_WAN_Zone  |  -  |  -  |  -  |  -  |  -  |  -  |
| Vlan452 |  Tenant_D_WAN_Zone  |  -  |  10.4.12.254/24  |  -  |  -  |  -  |  -  |

#### IPv6

| Interface | VRF | IPv6 Address | IPv6 Virtual Address | Virtual Router Address | VRRP | ND RA Disabled | Managed Config Flag | IPv6 ACL In | IPv6 ACL Out |
| --------- | --- | ------------ | -------------------- | ---------------------- | ---- | -------------- | ------------------- | ----------- | ------------ |
| Vlan410 | Tenant_D_OP_Zone | - | 2001:db8:310::1/64 | - | - | - | - | - | - |
| Vlan411 | Tenant_D_OP_Zone | 2001:db8:311::2/64 | - | 2001:db8:311::1/64 | - | - | - | - | - |
| Vlan412 | Tenant_D_OP_Zone | - | 2001:db8:412::1/64 | - | - | - | - | - | - |
| Vlan450 | Tenant_D_WAN_Zone | - | 2001:db8:355::1/64 | - | - | - | - | - | - |
| Vlan451 | Tenant_D_WAN_Zone | - | 2001:db8:451::1/64 | - | - | - | - | - | - |
| Vlan452 | Tenant_D_WAN_Zone | - | 2001:db8:412::1/64 | - | - | - | - | - | - |

### VLAN Interfaces Device Configuration

```eos
!
interface Vlan110
   description Tenant_A_OP_Zone_1
   no shutdown
   vrf Tenant_A_OP_Zone
   ip address virtual 10.1.10.1/24
!
interface Vlan111
   description Tenant_A_OP_Zone_2
   no shutdown
   vrf Tenant_A_OP_Zone
   ip helper-address 1.1.1.1 vrf MGMT source-interface lo100
   ip address virtual 10.1.11.1/24
!
interface Vlan112
   description Tenant_A_OP_Zone_3
   no shutdown
   mtu 1560
   vrf Tenant_A_OP_Zone
   ip helper-address 2.2.2.2 vrf MGMT source-interface lo101
!
interface Vlan120
   description Tenant_A_WEB_Zone_1
   no shutdown
   vrf Tenant_A_WEB_Zone
   ip helper-address 1.1.1.1 vrf TEST source-interface lo100
   ip address virtual 10.1.20.1/24
   ip address virtual 10.2.20.1/24 secondary
   ip address virtual 10.2.21.1/24 secondary
!
interface Vlan121
   description Tenant_A_WEBZone_2
   shutdown
   mtu 1560
   vrf Tenant_A_WEB_Zone
   ip address virtual 10.1.10.254/24
!
interface Vlan130
   description Tenant_A_APP_Zone_1
   no shutdown
   vrf Tenant_A_APP_Zone
   ip address virtual 10.1.30.1/24
!
interface Vlan131
   description Tenant_A_APP_Zone_2
   no shutdown
   vrf Tenant_A_APP_Zone
   ip address virtual 10.1.31.1/24
!
interface Vlan140
   description Tenant_A_DB_BZone_1
   no shutdown
   vrf Tenant_A_DB_Zone
   ip address virtual 10.1.40.1/24
!
interface Vlan141
   description Tenant_A_DB_Zone_2
   no shutdown
   vrf Tenant_A_DB_Zone
   ip address virtual 10.1.41.1/24
!
interface Vlan210
   description Tenant_B_OP_Zone_1
   no shutdown
   vrf Tenant_B_OP_Zone
   ip address virtual 10.2.10.1/24
!
interface Vlan211
   description Tenant_B_OP_Zone_2
   no shutdown
   vrf Tenant_B_OP_Zone
   ip address virtual 10.2.11.1/24
!
interface Vlan310
   description Tenant_C_OP_Zone_1
   no shutdown
   vrf Tenant_C_OP_Zone
   ip address virtual 10.3.10.1/24
!
interface Vlan311
   description Tenant_C_OP_Zone_2
   no shutdown
   vrf Tenant_C_OP_Zone
   ip address virtual 10.3.11.1/24
!
interface Vlan410
   description Tenant_D_v6_OP_Zone_1
   no shutdown
   vrf Tenant_D_OP_Zone
   ipv6 address virtual 2001:db8:310::1/64
   ip address virtual 10.3.10.1/24
!
interface Vlan411
   description Tenant_D_v6_OP_Zone_2
   no shutdown
   vrf Tenant_D_OP_Zone
   ip address 10.3.11.2/24
   ipv6 address 2001:db8:311::2/64
   ipv6 virtual-router address 2001:db8:311::1/64
   ip virtual-router address 10.3.11.1/24
!
interface Vlan412
   description Tenant_D_v6_OP_Zone_1
   no shutdown
   mtu 1560
   vrf Tenant_D_OP_Zone
   ipv6 address virtual 2001:db8:412::1/64
   ip address virtual 10.4.12.254/24
!
interface Vlan450
   description Tenant_D_v6_WAN_Zone_1
   no shutdown
   vrf Tenant_D_WAN_Zone
   ipv6 address virtual 2001:db8:355::1/64
!
interface Vlan451
   description Tenant_D_v6_WAN_Zone_2
   no shutdown
   mtu 1560
   vrf Tenant_D_WAN_Zone
   ipv6 address virtual 2001:db8:451::1/64
!
interface Vlan452
   description Tenant_D_v6_WAN_Zone_3
   no shutdown
   mtu 1560
   vrf Tenant_D_WAN_Zone
   ipv6 address virtual 2001:db8:412::1/64
   ip address virtual 10.4.12.254/24
```

## VXLAN Interface

### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |

#### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 110 | 10110 | - | - |
| 111 | 50111 | - | - |
| 112 | 10112 | - | - |
| 120 | 10120 | - | - |
| 121 | 10121 | - | - |
| 130 | 10130 | - | - |
| 131 | 10131 | - | - |
| 140 | 10140 | - | - |
| 141 | 10141 | - | - |
| 160 | 10160 | - | - |
| 161 | 10161 | - | - |
| 210 | 20210 | - | - |
| 211 | 20211 | - | - |
| 310 | 30310 | - | - |
| 311 | 30311 | - | - |
| 410 | 40410 | - | - |
| 411 | 40411 | - | - |
| 412 | 40412 | - | - |
| 450 | 40450 | - | - |
| 451 | 40451 | - | - |
| 452 | 40452 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| Tenant_A_APP_Zone | 12 | - |
| Tenant_A_DB_Zone | 13 | - |
| Tenant_A_OP_Zone | 10 | - |
| Tenant_A_WEB_Zone | 11 | - |
| Tenant_B_OP_Zone | 20 | - |
| Tenant_C_OP_Zone | 30 | - |
| Tenant_D_OP_Zone | 40 | - |
| Tenant_D_WAN_Zone | 41 | - |
| Tenant_OSPF | 30 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description DC1-LEAF2A_VTEP
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 111 vni 50111
   vxlan vlan 112 vni 10112
   vxlan vlan 120 vni 10120
   vxlan vlan 121 vni 10121
   vxlan vlan 130 vni 10130
   vxlan vlan 131 vni 10131
   vxlan vlan 140 vni 10140
   vxlan vlan 141 vni 10141
   vxlan vlan 160 vni 10160
   vxlan vlan 161 vni 10161
   vxlan vlan 210 vni 20210
   vxlan vlan 211 vni 20211
   vxlan vlan 310 vni 30310
   vxlan vlan 311 vni 30311
   vxlan vlan 410 vni 40410
   vxlan vlan 411 vni 40411
   vxlan vlan 412 vni 40412
   vxlan vlan 450 vni 40450
   vxlan vlan 451 vni 40451
   vxlan vlan 452 vni 40452
   vxlan vrf Tenant_A_APP_Zone vni 12
   vxlan vrf Tenant_A_DB_Zone vni 13
   vxlan vrf Tenant_A_OP_Zone vni 10
   vxlan vrf Tenant_A_WEB_Zone vni 11
   vxlan vrf Tenant_B_OP_Zone vni 20
   vxlan vrf Tenant_C_OP_Zone vni 30
   vxlan vrf Tenant_D_OP_Zone vni 40
   vxlan vrf Tenant_D_WAN_Zone vni 41
   vxlan vrf Tenant_OSPF vni 30
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:dc:00:00:00:0a

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:dc:00:00:00:0a
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| MGMT | false |
| Tenant_A_APP_Zone | true |
| Tenant_A_DB_Zone | true |
| Tenant_A_OP_Zone | true |
| Tenant_A_WEB_Zone | true |
| Tenant_B_OP_Zone | true |
| Tenant_C_OP_Zone | true |
| Tenant_D_OP_Zone | true |
| Tenant_D_WAN_Zone | true |
| Tenant_OSPF | true |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf Tenant_A_APP_Zone
ip routing vrf Tenant_A_DB_Zone
ip routing vrf Tenant_A_OP_Zone
ip routing vrf Tenant_A_WEB_Zone
ip routing vrf Tenant_B_OP_Zone
ip routing vrf Tenant_C_OP_Zone
ip routing vrf Tenant_D_OP_Zone
ip routing vrf Tenant_D_WAN_Zone
ip routing vrf Tenant_OSPF
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| MGMT | false |
| Tenant_A_APP_Zone | false |
| Tenant_A_DB_Zone | false |
| Tenant_A_OP_Zone | false |
| Tenant_A_WEB_Zone | false |
| Tenant_B_OP_Zone | false |
| Tenant_C_OP_Zone | false |
| Tenant_D_OP_Zone | true |
| Tenant_D_WAN_Zone | true |
| Tenant_OSPF | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 192.168.200.5 | - | 1 | - | - | - |
| Tenant_D_OP_Zone | 0.0.0.0/0 | 10.3.11.4 | - | 1 | - | - | - |
| Tenant_D_OP_Zone | 10.3.11.0/24 | - | Vlan411 | 1 | - | VARP | - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.200.5
ip route vrf Tenant_D_OP_Zone 0.0.0.0/0 10.3.11.4
ip route vrf Tenant_D_OP_Zone 10.3.11.0/24 Vlan411 name VARP
```

## IPv6 Static Routes

### IPv6 Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| Tenant_D_OP_Zone | ::/0 | 2001:db8:311::4 | - | 1 | - | IPv6-test-2 | - |
| Tenant_D_OP_Zone | 2001:db8:311::/64 | - | Vlan411 | 1 | - | VARPv6 | - |

### Static Routes Device Configuration

```eos
!
ipv6 route vrf Tenant_D_OP_Zone ::/0 2001:db8:311::4 name IPv6-test-2
ipv6 route vrf Tenant_D_OP_Zone 2001:db8:311::/64 Vlan411 name VARPv6
```

## Router OSPF

### Router OSPF Summary

| Process ID | Router ID | Default Passive Interface | No Passive Interface | BFD | Max LSA | Default Information Originate | Log Adjacency Changes Detail | Auto Cost Reference Bandwidth | Maximum Paths | MPLS LDP Sync Default | Distribute List In |
| ---------- | --------- | ------------------------- | -------------------- | --- | ------- | ----------------------------- | ---------------------------- | ----------------------------- | ------------- | --------------------- | ------------------ |
| 30 | 192.168.255.10 | enabled | Ethernet22 <br> Ethernet23 <br> | disabled | default | disabled | disabled | - | - | - | - |

### Router OSPF Router Redistribution

| Process ID | Source Protocol | Route Map |
| ---------- | --------------- | --------- |
| 30 | bgp | - |

### OSPF Interfaces

| Interface | Area | Cost | Point To Point |
| -------- | -------- | -------- | -------- |
| Ethernet22 | 0 | - | True |
| Ethernet23 | 0 | - | True |

### Router OSPF Device Configuration

```eos
!
router ospf 30 vrf Tenant_OSPF
   router-id 192.168.255.10
   passive-interface default
   no passive-interface Ethernet22
   no passive-interface Ethernet23
   redistribute bgp
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102|  192.168.255.10 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- |
| 172.31.254.32 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.34 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.36 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.38 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.40 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.42 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.44 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 172.31.254.46 | 65001 | default | - | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - | - |
| 192.168.255.1 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 192.168.255.2 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 192.168.255.3 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 192.168.255.4 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

#### EVPN Host Flapping Settings

| State | Window | Threshold | Expiry Timeout |
| ----- | ------ | --------- | -------------- |
| Enabled | 180 Seconds | 5 | 10 Seconds |

### Router BGP VLAN Aware Bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| Tenant_A_APP_Zone | 65001:12 | 100000:12 | - | - | learned | 130-131 |
| Tenant_A_DB_Zone | 65001:13 | 100000:13 | - | - | learned | 140-141 |
| Tenant_A_NFS | 65001:20161 | 100000:20161 | - | - | learned | 161 |
| Tenant_A_OP_Zone | 65001:9 | 100000:9 | - | - | learned | 110-112 |
| Tenant_A_VMOTION | 65001:20160 | 100000:20160 | - | - | learned | 160 |
| Tenant_A_WEB_Zone | 65001:11 | 100000:11 | - | - | learned | 120-121 |
| Tenant_B_OP_Zone | 65001:20 | 100000:20 | - | - | learned | 210-211 |
| Tenant_C_OP_Zone | 65001:30 | 100000:30 | - | - | learned | 310-311 |
| Tenant_D_OP_Zone | 65001:40 | 100000:40 | - | - | learned | 410-412 |
| Tenant_D_WAN_Zone | 65001:41 | 100000:41 | - | - | learned | 450-452 |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Tenant_A_APP_Zone | 65001:12 | connected |
| Tenant_A_DB_Zone | 65001:13 | connected |
| Tenant_A_OP_Zone | 65001:9 | connected |
| Tenant_A_WEB_Zone | 65001:11 | connected |
| Tenant_B_OP_Zone | 65001:20 | connected |
| Tenant_C_OP_Zone | 65001:30 | connected |
| Tenant_D_OP_Zone | 65001:40 | connected<br>static |
| Tenant_D_WAN_Zone | 65001:41 | connected |
| Tenant_OSPF | 65001:30 | connected<br>ospf |

### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 192.168.255.10
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor UNDERLAY-PEERS peer group
   neighbor UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor UNDERLAY-PEERS send-community
   neighbor UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.31.254.32 peer group UNDERLAY-PEERS
   neighbor 172.31.254.32 remote-as 65001
   neighbor 172.31.254.32 description DC1-SPINE1_Ethernet3/1
   neighbor 172.31.254.34 peer group UNDERLAY-PEERS
   neighbor 172.31.254.34 remote-as 65001
   neighbor 172.31.254.34 description DC1-SPINE2_Ethernet1/3/1
   neighbor 172.31.254.36 peer group UNDERLAY-PEERS
   neighbor 172.31.254.36 remote-as 65001
   neighbor 172.31.254.36 description DC1-SPINE2_Ethernet1/4/1
   neighbor 172.31.254.38 peer group UNDERLAY-PEERS
   neighbor 172.31.254.38 remote-as 65001
   neighbor 172.31.254.38 description DC1-SPINE1_Ethernet4/1
   neighbor 172.31.254.40 peer group UNDERLAY-PEERS
   neighbor 172.31.254.40 remote-as 65001
   neighbor 172.31.254.40 description DC1-SPINE3_Ethernet1/3/1
   neighbor 172.31.254.42 peer group UNDERLAY-PEERS
   neighbor 172.31.254.42 remote-as 65001
   neighbor 172.31.254.42 description DC1-SPINE4_Ethernet3/1
   neighbor 172.31.254.44 peer group UNDERLAY-PEERS
   neighbor 172.31.254.44 remote-as 65001
   neighbor 172.31.254.44 description DC1-SPINE4_Ethernet4/1
   neighbor 172.31.254.46 peer group UNDERLAY-PEERS
   neighbor 172.31.254.46 remote-as 65001
   neighbor 172.31.254.46 description DC1-SPINE3_Ethernet1/4/1
   neighbor 192.168.255.1 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.1 remote-as 65001
   neighbor 192.168.255.1 description DC1-SPINE1
   neighbor 192.168.255.2 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.2 remote-as 65001
   neighbor 192.168.255.2 description DC1-SPINE2
   neighbor 192.168.255.3 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.3 remote-as 65001
   neighbor 192.168.255.3 description DC1-SPINE3
   neighbor 192.168.255.4 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.4 remote-as 65001
   neighbor 192.168.255.4 description DC1-SPINE4
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle Tenant_A_APP_Zone
      rd 65001:12
      route-target both 100000:12
      redistribute learned
      vlan 130-131
   !
   vlan-aware-bundle Tenant_A_DB_Zone
      rd 65001:13
      route-target both 100000:13
      redistribute learned
      vlan 140-141
   !
   vlan-aware-bundle Tenant_A_NFS
      rd 65001:20161
      route-target both 100000:20161
      redistribute learned
      vlan 161
   !
   vlan-aware-bundle Tenant_A_OP_Zone
      rd 65001:9
      route-target both 100000:9
      redistribute learned
      vlan 110-112
   !
   vlan-aware-bundle Tenant_A_VMOTION
      rd 65001:20160
      route-target both 100000:20160
      redistribute learned
      vlan 160
   !
   vlan-aware-bundle Tenant_A_WEB_Zone
      rd 65001:11
      route-target both 100000:11
      redistribute learned
      vlan 120-121
   !
   vlan-aware-bundle Tenant_B_OP_Zone
      rd 65001:20
      route-target both 100000:20
      redistribute learned
      vlan 210-211
   !
   vlan-aware-bundle Tenant_C_OP_Zone
      rd 65001:30
      route-target both 100000:30
      redistribute learned
      vlan 310-311
   !
   vlan-aware-bundle Tenant_D_OP_Zone
      rd 65001:40
      route-target both 100000:40
      redistribute learned
      vlan 410-412
   !
   vlan-aware-bundle Tenant_D_WAN_Zone
      rd 65001:41
      route-target both 100000:41
      redistribute learned
      vlan 450-452
   !
   address-family evpn
      host-flap detection window 180 threshold 5 expiry timeout 10 seconds
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor UNDERLAY-PEERS activate
   !
   vrf Tenant_A_APP_Zone
      rd 65001:12
      route-target import evpn 100000:12
      route-target export evpn 100000:12
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_A_DB_Zone
      rd 65001:13
      route-target import evpn 100000:13
      route-target export evpn 100000:13
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_A_OP_Zone
      rd 65001:9
      route-target import evpn 100000:9
      route-target export evpn 100000:9
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_A_WEB_Zone
      rd 65001:11
      route-target import evpn 100000:11
      route-target export evpn 100000:11
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_B_OP_Zone
      rd 65001:20
      route-target import evpn 100000:20
      route-target export evpn 100000:20
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_C_OP_Zone
      rd 65001:30
      route-target import evpn 100000:30
      route-target export evpn 100000:30
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_D_OP_Zone
      rd 65001:40
      route-target import evpn 100000:40
      route-target export evpn 100000:40
      router-id 192.168.255.10
      redistribute connected
      redistribute static
   !
   vrf Tenant_D_WAN_Zone
      rd 65001:41
      route-target import evpn 100000:41
      route-target export evpn 100000:41
      router-id 192.168.255.10
      redistribute connected
   !
   vrf Tenant_OSPF
      rd 65001:30
      route-target import evpn 100000:30
      route-target export evpn 100000:30
      router-id 192.168.255.10
      redistribute connected
      redistribute ospf
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Vlan Summary

| Vlan | IGMP Snooping | Fast Leave | Max Groups | Proxy |
| ---- | ------------- | ---------- | ---------- | ----- |
| 120 | False | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
!
no ip igmp snooping vlan 120
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.168.255.0/24 eq 32 |
| 20 | permit 192.168.254.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.168.255.0/24 eq 32
   seq 20 permit 192.168.254.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| Tenant_A_APP_Zone | enabled |
| Tenant_A_DB_Zone | enabled |
| Tenant_A_OP_Zone | enabled |
| Tenant_A_WEB_Zone | enabled |
| Tenant_B_OP_Zone | enabled |
| Tenant_C_OP_Zone | enabled |
| Tenant_D_OP_Zone | enabled |
| Tenant_D_WAN_Zone | enabled |
| Tenant_OSPF | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance Tenant_A_APP_Zone
!
vrf instance Tenant_A_DB_Zone
!
vrf instance Tenant_A_OP_Zone
   description Tenant_A_OP_Zone
!
vrf instance Tenant_A_WEB_Zone
!
vrf instance Tenant_B_OP_Zone
!
vrf instance Tenant_C_OP_Zone
!
vrf instance Tenant_D_OP_Zone
!
vrf instance Tenant_D_WAN_Zone
!
vrf instance Tenant_OSPF
```

# Virtual Source NAT

## Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| Tenant_A_OP_Zone | 10.255.1.10 |

## Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf Tenant_A_OP_Zone address 10.255.1.10
```

# Platform

## Platform Summary

### Platform Sand Summary

| Settings | Value |
| -------- | ----- |
| Hardware Only Lag | True |

## Platform Configuration

```eos
!
platform sand lag hardware-only
```

# Quality Of Service
