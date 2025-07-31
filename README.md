<img width="786" height="259" alt="image" src="https://github.com/user-attachments/assets/56150547-0631-47bb-a164-ee6e24ed7dd8" />

## Cisco 8000 emulator
- Download <br>
https://software.cisco.com/download/beta/1028492456

- How to setup <br>
please see "cml-8102.README" or "CML documentation" in the above link

## Lab
* Please shut/no shut Bundle-Ether 1 on CE1 if CE1's mac address cannot be learnt by PEs.

### Bundle
```
RP/0/RP0/CPU0:PE1#show bundle bundle-Ether 1
Thu Jul 31 14:08:53.571 UTC

Bundle-Ether1
  Status:                                    Up
  Local links <active/standby/configured>:   1 / 0 / 1
  Local bandwidth <effective/available>:     100000000 (100000000) kbps
  MAC address (source):                      78c6.3c1b.8106 (Chassis pool)
  Inter-chassis link:                        No
  Minimum active links / bandwidth:          1 / 1 kbps
  Maximum active links:                      64
  Wait while timer:                          2000 ms
  Load balancing:                            
    Link order signaling:                    Not configured
    Hash type:                               Default
    Locality threshold:                      None
  LACP:                                      Not operational
    Flap suppression timer:                  Off
    Cisco extensions:                        Disabled
    Non-revertive:                           Disabled
  mLACP:                                     Not configured
  IPv4 BFD:                                  Not configured
  IPv6 BFD:                                  Not configured

  Port                  Device           State        Port ID         B/W, kbps
  --------------------  ---------------  -----------  --------------  ----------
  Hu0/0/0/1             Local            Active       0x8000, 0x0000   100000000
      Link is Active
RP/0/RP0/CPU0:PE1#
```

### OFPS
```
RP/0/RP0/CPU0:PE1#show ospf neighbor
Thu Jul 31 14:09:32.957 UTC

* Indicates MADJ interface
# Indicates Neighbor awaiting BFD session up

Neighbors for OSPF 100

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3         1     FULL/BDR        00:00:38    100.101.103.103 HundredGigE0/0/0/0
    Neighbor is up for 01:13:42
2.2.2.2         1     FULL/BDR        00:00:32    100.101.102.102 HundredGigE0/0/0/2
    Neighbor is up for 01:21:28

Total neighbor count: 2
RP/0/RP0/CPU0:PE1#
```

### BGP
```
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show bgp neighbor brief
Thu Jul 31 14:10:07.917 UTC

Neighbor         Spk    AS  Description                         Up/Down  NBRState
2.2.2.2           0   100                                      00:58:13 Established 
3.3.3.3           0   100                                      01:12:15 Established 
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show bgp l2vpn evpn summary
Thu Jul 31 14:10:16.021 UTC
BGP router identifier 1.1.1.1, local AS number 100
BGP generic scan interval 60 secs
Non-stop routing is enabled
BGP table state: Active
Table ID: 0x0
BGP table nexthop route policy: 
BGP main routing table version 24
BGP NSR Initial initsync version 1 (Reached)
BGP NSR/ISSU Sync-Group versions 0/0
BGP scan interval 60 secs

BGP is operating in STANDALONE mode.


Process    RcvTblVer     bRIB/RIB     LabelVer    ImportVer    SendTblVer   StandbyVer
Speaker           24            24            24            24            24             0

Neighbor        Spk    AS MsgRcvd MsgSent       TblVer  InQ OutQ  Up/Down  St/PfxRcd
2.2.2.2           0   100      66      69           24    0    0 00:58:21          5
3.3.3.3           0   100      81      84           24    0    0 01:12:24          4

RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show bgp l2vpn evpn
Thu Jul 31 14:10:27.083 UTC
BGP router identifier 1.1.1.1, local AS number 100
BGP generic scan interval 60 secs
Non-stop routing is enabled
BGP table state: Active
Table ID: 0x0
BGP table nexthop route policy: 
BGP main routing table version 24
BGP NSR Initial initsync version 1 (Reached)
BGP NSR/ISSU Sync-Group versions 0/0
BGP scan interval 60 secs

Status codes: s suppressed, d damped, h history, * valid, > best
              i - internal, r RIB-failure, S stale, N Nexthop-discard
Origin codes: i - IGP, e - EGP, ? - incomplete
   Network            Next Hop            Metric LocPrf Weight Path
Route Distinguisher: 1.1.1.1:0 (default for vrf ES:GLOBAL)
Route Distinguisher Version: 17
*> [1][1.1.1.1:1][0011.1111.1111.1111.1111][4294967295]/184
                      0.0.0.0                                0 i
*> [4][0011.1111.1111.1111.1111][32][1.1.1.1]/128
                      0.0.0.0                                0 i
*>i[4][0011.1111.1111.1111.1111][32][2.2.2.2]/128
                      2.2.2.2                       100      0 i
Route Distinguisher: 1.1.1.1:100 (default for vrf 100)
Route Distinguisher Version: 24
*> [1][0011.1111.1111.1111.1111][0]/120
                      0.0.0.0                                0 i
* i                   2.2.2.2                       100      0 i
*>i[1][0011.1111.1111.1111.1111][4294967295]/120
                      2.2.2.2                       100      0 i
*>i[1][0022.2222.2222.2222.2222][0]/120
                      3.3.3.3                       100      0 i
*>i[1][0022.2222.2222.2222.2222][4294967295]/120
                      3.3.3.3                       100      0 i
*>i[2][0][48][78c2.f071.9d06][0]/104
                      3.3.3.3                       100      0 i
*> [2][0][48][78da.700f.ed06][0]/104
                      0.0.0.0                                0 i
* i                   2.2.2.2                       100      0 i
*> [3][0][32][1.1.1.1]/80
                      0.0.0.0                                0 i
*>i[3][0][32][2.2.2.2]/80
                      2.2.2.2                       100      0 i
*>i[3][0][32][3.3.3.3]/80
                      3.3.3.3                       100      0 i
Route Distinguisher: 2.2.2.2:0
Route Distinguisher Version: 14
*>i[4][0011.1111.1111.1111.1111][32][2.2.2.2]/128
                      2.2.2.2                       100      0 i
Route Distinguisher: 2.2.2.2:1
Route Distinguisher Version: 20
*>i[1][2.2.2.2:1][0011.1111.1111.1111.1111][4294967295]/184
                      2.2.2.2                       100      0 i
Route Distinguisher: 2.2.2.2:100
Route Distinguisher Version: 22
*>i[1][0011.1111.1111.1111.1111][0]/120
                      2.2.2.2                       100      0 i
*>i[2][0][48][78da.700f.ed06][0]/104
                      2.2.2.2                       100      0 i
*>i[3][0][32][2.2.2.2]/80
                      2.2.2.2                       100      0 i
Route Distinguisher: 3.3.3.3:1
Route Distinguisher Version: 10
*>i[1][3.3.3.3:1][0022.2222.2222.2222.2222][4294967295]/184
                      3.3.3.3                       100      0 i
Route Distinguisher: 3.3.3.3:100
Route Distinguisher Version: 12
*>i[1][0022.2222.2222.2222.2222][0]/120
                      3.3.3.3                       100      0 i
*>i[2][0][48][78c2.f071.9d06][0]/104
                      3.3.3.3                       100      0 i
*>i[3][0][32][3.3.3.3]/80
                      3.3.3.3                       100      0 i

Processed 21 prefixes, 23 paths
RP/0/RP0/CPU0:PE1# 
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show mpls forwarding 
Thu Jul 31 14:10:53.651 UTC
Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       
Label  Label       or ID              Interface                    Switched    
------ ----------- ------------------ ------------ --------------- ------------
16102  Pop         SR Pfx (idx 102)   Hu0/0/0/2    100.101.102.102 0           
16103  Pop         SR Pfx (idx 103)   Hu0/0/0/0    100.101.103.103 0           
24000  Pop         EVPN:100 U         BD=0 E       point2point     0           
24001  Pop         EVPN:100 M         BD=0 EIM     point2point     0           
24003  Pop         SR Adj (idx 0)     Hu0/0/0/2    100.101.102.102 0           
24004  Pop         SR Adj (idx 0)     Hu0/0/0/0    100.101.103.103 0           
RP/0/RP0/CPU0:PE1#
```

### EVPN
```
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show l2vpn bridge-domain
Thu Jul 31 14:11:43.557 UTC
Legend: pp = Partially Programmed.
Bridge group: 100, bridge-domain: 100, id: 0, state: up, ShgId: 0, MSTi: 0
  Aging: 300 s, MAC limit: 131072, Action: none, Notification: syslog
  Filter MAC addresses: 0
  ACs: 1 (1 up), VFIs: 0, PWs: 0 (0 up), PBBs: 0 (0 up), VNIs: 0 (0 up)
  List of EVPNs:
    EVPN, state: up
  List of ACs:
    BE1, state: up, Static MAC addresses: 0, MSTi: 5
  List of Access PWs:
  List of VFIs:
  List of Access VFIs:
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show l2vpn forwarding interface Bundle-Ether 1 detail location 0/RP0/CPU0
Thu Jul 31 14:11:55.559 UTC
Local interface: Bundle-Ether1, Xconnect id: 0xc0000001, Status: up
  Segment 1
    AC, Bundle-Ether1, Ethernet port mode, status: Bound
    Statistics:
      packets: received 0 (multicast 0, broadcast 0, unknown unicast 0, unicast 0), sent 24
      bytes: received 0 (multicast 0, broadcast 0, unknown unicast 0, unicast 0), sent 1806
      MAC move: 0
      packets dropped: PLU 0, tail 0
      bytes dropped: PLU 0, tail 0
  Segment 2
    Bridge id: 0, Split horizon group id: 0, status: Bound
    Storm control: disabled
    MAC learning: enabled
    Software MAC learning: enabled
    MAC port down flush: enabled
    Flooding:
      Broadcast & Multicast: enabled
      Unknown unicast: enabled
    MAC aging time: 300 s, Type: inactivity
    MAC limit: none
    MAC Secure: disabled, Logging: disabled, Accept-Shutdown: enabled
    DHCPv4 snooping: profile not known on this node, disabled
    Dynamic ARP Inspection: disabled, Logging: disabled
    IP Source Guard: disabled, Logging: disabled
    IGMP snooping profile: profile not known on this node 
    MLD snooping profile: profile not known on this node 
    Router guard disabled
    P2MP PW: disabled
    PD System Data: Learn key: 0

RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show evpn evi vpn-id 100 detail
Thu Jul 31 14:12:02.577 UTC

VPN-ID     Encap      Bridge Domain                Type               
---------- ---------- ---------------------------- -------------------
100        MPLS       100                          EVPN               
   Stitching: Regular
   Unicast Label  : 24000
   Multicast Label: 24001
   Reroute Label: 0
   Flow Label: N
   Dynamic Flow Label: No
   Control-Word: Enabled
   E-Tree: Root
   Forward-class: 0
   Advertise MACs: Yes
   Advertise BVI MACs: No
   Aliasing: Enabled
   UUF: Enabled
   Re-origination: Enabled
   Multicast:
     IGMP-Snooping Proxy: No
     MLD-Snooping Proxy : No
   BGP Implicit Import: Enabled
   VRF Name: 
   Preferred Nexthop Mode: Off
   BVI Coupled Mode: No
   BVI Subnet Withheld: ipv4 No, ipv6 No
   L3VRF Label Mode: Per-VRF
   RD Config: none
   RD Auto  : (auto) 1.1.1.1:100
   RT Auto  : 100:100
   Route Targets in Use           Type                 
   ------------------------------ ---------------------
   100:100                        Import               
   100:100                        Export               

RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show evpn ethernet-segment esi 0011.1111.1111.1111.1111 detail
Thu Jul 31 14:12:56.007 UTC
Legend:
  B   - No Forwarders EVPN-enabled,
  C   - MAC missing (Grouping ES-MAC vES),
  RT  - ES-Import Route Target missing,
  E   - ESI missing,
  H   - Interface handle missing,
  I   - Name (Interface or Virtual Access) missing,
  M   - Interface in Down state,
  O   - BGP End of Download missing,
  P   - Interface already Access Protected,
  Pf  - Interface forced single-homed,
  R   - BGP RID not received,
  S   - Interface in redundancy standby state,
  X   - ESI-extracted MAC Conflict
  SHG - No local split-horizon-group label allocated
  Hp  - Interface blocked on peering complete during HA event
  Rc  - Recovery timer running during peering sequence

Ethernet Segment Id      Interface                          Nexthops            
------------------------ ---------------------------------- --------------------
0011.1111.1111.1111.1111 BE1                                1.1.1.1
                                                            2.2.2.2
  ES to BGP Gates   : Ready
  ES to L2FIB Gates : Ready
  Main port         :
     Interface name : Bundle-Ether1
     Interface MAC  : 78c6.3c1b.8106
     IfHandle       : 0x7800001c
     State          : Up
     Redundancy     : Not Defined
  ESI ID            : 1
  ESI type          : 0
     Value          : 0011.1111.1111.1111.1111
  ES Import RT      : 1111.1111.1111 (from ESI)
  Topology          :
     Operational    : MH, All-active
     Configured     : All-active (AApF) (default)
  Service Carving   : Auto-selection
     Multicast      : Disabled
  Convergence       : 
  Peering Details   : 2 Nexthops
     1.1.1.1 [MOD:P:00:T]
     2.2.2.2 [MOD:P:00:T]
  Service Carving Synchronization:
     Mode           : NONE
     Peer Updates   :
                 1.1.1.1 [SCT: N/A]
                 2.2.2.2 [SCT: N/A]
  Service Carving Results:
     Forwarders     : 1
     Elected        : 1
     Not Elected    : 0
  EVPN-VPWS Service Carving Results:
     Primary        : 0
     Backup         : 0
     Non-DF         : 0
  MAC Flush msg     : STP-TCN
  Peering timer     : 3 sec [not running]
  Recovery timer    : 30 sec [not running]
  Carving timer     : 0 sec [not running]
  Revert timer      : 0 sec [not running]
  HRW Reset timer   : 5 sec [not running]
  Local SHG label   : 24002
     IPv6_Filtering_ID : 1:16
  Remote SHG labels : 1
              24004 : nexthop 2.2.2.2
  Access signal mode: Bundle OOS

RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#
RP/0/RP0/CPU0:PE1#show evpn evi mac
Thu Jul 31 14:13:11.418 UTC

VPN-ID     Encap      MAC address    IP address                               Nexthop                                 Label   
---------- ---------- -------------- ---------------------------------------- --------------------------------------- --------
100        MPLS       78c2.f071.9d06 ::                                       3.3.3.3                                 24002   
100        MPLS       78da.700f.ed06 ::                                       2.2.2.2                                 24002   
RP/0/RP0/CPU0:PE1#
```

### CE
```
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#show bundle bundle-Ether 1
Thu Jul 31 14:14:04.197 UTC

Bundle-Ether1
  Status:                                    Up
  Local links <active/standby/configured>:   2 / 0 / 2
  Local bandwidth <effective/available>:     200000000 (200000000) kbps
  MAC address (source):                      78da.700f.ed06 (Chassis pool)
  Inter-chassis link:                        No
  Minimum active links / bandwidth:          1 / 1 kbps
  Maximum active links:                      64
  Wait while timer:                          2000 ms
  Load balancing:                            
    Link order signaling:                    Not configured
    Hash type:                               Default
    Locality threshold:                      None
  LACP:                                      Not operational
    Flap suppression timer:                  Off
    Cisco extensions:                        Disabled
    Non-revertive:                           Disabled
  mLACP:                                     Not configured
  IPv4 BFD:                                  Not configured
  IPv6 BFD:                                  Not configured

  Port                  Device           State        Port ID         B/W, kbps
  --------------------  ---------------  -----------  --------------  ----------
  Hu0/0/0/0             Local            Active       0x8000, 0x0000   100000000
      Link is Active
  Hu0/0/0/1             Local            Active       0x8000, 0x0000   100000000
      Link is Active
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#show arp
Thu Jul 31 14:14:15.211 UTC

-------------------------------------------------------------------------------
0/RP0/CPU0
-------------------------------------------------------------------------------
Address         Age        Hardware Addr   State      Type  Interface
10.1.1.2        -          78da.700f.ed06  Interface  ARPA  Bundle-Ether1
10.1.1.3        00:27:03   78c2.f071.9d06  Dynamic    ARPA  Bundle-Ether1
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#ping 10.1.1.3
Thu Jul 31 14:14:18.736 UTC
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.3 timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/11/17 ms
RP/0/RP0/CPU0:CE1#
RP/0/RP0/CPU0:CE1#
```
