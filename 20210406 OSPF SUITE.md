---
attachments: [Clipboard_2021-04-06-09-30-35.png]
title: 20210406 OSPF SUITE
created: '2021-04-06T07:23:00.218Z'
modified: '2021-04-14T10:05:34.197Z'
---

# 20210406 OSPF SUITE

![](@attachment/Clipboard_2021-04-06-09-30-35.png)

propagation route par defaut
et apprentissage summary


### MLS 1

***configuration***

      show ip int brief              ( attention a gns dans le method il n'y a pas de cable reel)



      MLS1#conf t
      MLS1(config)#router ospf 1
      MLS1(config-router)#router-id 0.0.0.1
      MLS1(config-router)#passive-interface default        ( reactiver les interfaces manuellement)
      MLS1(config-router)#no passive-interface eth1/2      ( reactive l'interface eth 1/2)
      MLS1(config-router)#no passive-interface eth1/1      ( reactive l'interface eth 1/1)
      MLS1(config-router)#network 120.0.255.12 0.0.0.3 area 0   (wildcard mask)
      MLS1(config-router)#network 120.0.255.16 0.0.0.3 area 0
      MLS1(config-router)#network 120.0.0.0 0.0.1.255 area 10  ( network des vlan )


#### ***verification*** 

***MLS1#sh ip protocol***


      Routing Protocol is "ospf 1"
        Outgoing update filter list for all interfaces is not set
        Incoming update filter list for all interfaces is not set
        Router ID 0.0.0.1
        It is an area border router
        Number of areas in this router is 2. 2 normal 0 stub 0 nssa
        Maximum path: 4
        Routing for Networks:
          120.0.0.0 0.0.1.255 area 10
          120.0.255.12 0.0.0.3 area 0
          120.0.255.16 0.0.0.3 area 0
        Passive Interface(s):
          Vlan1
          Vlan10
          Vlan20
        Routing Information Sources:
          Gateway         Distance      Last Update
        Distance: (default is 110)



***MLS1#sh ip ospf interface brief***

      Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
      Et1/2        1     0               120.0.255.18/30    10    DR    0/0
      Et1/1        1     0               120.0.255.14/30    10    DR    0/0
      Vl20         1     10              120.0.1.1/24       1     DR    0/0
      Vl10         1     10              120.0.0.1/24       1     DR    0/0


### MLS 2

***MLS2#sh ip int brief***

    Interface              IP-Address      OK? Method Status                Protocol
    Ethernet0/0            120.0.255.13    YES NVRAM  up                    up
    Ethernet0/1            120.0.255.9     YES NVRAM  up                    up
    Ethernet0/2            unassigned      YES unset  up                    up
    Ethernet0/3            unassigned      YES unset  up                    up
    Ethernet1/0            120.0.255.2     YES NVRAM  up                    up

#### ***Configuration***

    MLS2#conf t

    Enter configuration commands, one per line.  End with CNTL/Z.
    MLS2(config)#router ospf 1
    MLS2(config-router)#router-id 0.0.0.2
    MLS2(config-router)#passive-interface default
    MLS2(config-router)#no passive-interface e0/0
    MLS2(config-router)#no passive-interface e0/1
    MLS2(config-router)#no passive-interface e1/0


***configuration des interfaces methode 2***

    MLS2(config)#int range e0/0 - 1, e1/0
    MLS2(config-if-range)#ip ospf 1 area 0


***verification***

    MLS2#sh ip protocols
    Routing Protocol is "ospf 1"
      Outgoing update filter list for all interfaces is not set
      Incoming update filter list for all interfaces is not set
      Router ID 0.0.0.2
      Number of areas in this router is 1. 1 normal 0 stub 0 nssa
      Maximum path: 4
      Routing for Networks:
      Routing on Interfaces Configured Explicitly (Area 0):
        Ethernet0/0
        Ethernet0/1
        Ethernet1/0
      Passive Interface(s):
        Vlan1
      Routing Information Sources:
        Gateway         Distance      Last Update
        0.0.0.1              110      00:00:28
      Distance: (default is 110)




    MLS2#sh ip ospf int brief

    Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
    Et0/0        1     0               120.0.255.13/30    10    BDR   1/1
    Et0/1        1     0               120.0.255.9/30     10    DR    0/0
    Et1/0        1     0               120.0.255.2/30     10    DR    0/0



    MLS2#sh ip ospf neighbour

    Neighbor ID     Pri   State           Dead Time   Address         Interface
    0.0.0.1           1   FULL/DR         00:00:35    120.0.255.14    Ethernet0/0



    MLS2#sh ip route

                120.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
      O IA     120.0.0.0/24 [110/11] via 120.0.255.14, 00:02:42, Ethernet0/0
      O IA     120.0.1.0/24 [110/11] via 120.0.255.14, 00:02:42, Ethernet0/0
      C        120.0.255.0/30 is directly connected, Ethernet1/0
      L        120.0.255.2/32 is directly connected, Ethernet1/0
      C        120.0.255.8/30 is directly connected, Ethernet0/1
      L        120.0.255.9/32 is directly connected, Ethernet0/1
      C        120.0.255.12/30 is directly connected, Ethernet0/0
      L        120.0.255.13/32 is directly connected, Ethernet0/0
      O        120.0.255.16/30 [110/20] via 120.0.255.14, 00:02:42, Ethernet0/0


    MLS2#sh ip ospf database

            OSPF Router with ID (0.0.0.2) (Process ID 1)

                Router Link States (Area 0)

      Link ID         ADV Router      Age         Seq#       Checksum Link count
      0.0.0.1         0.0.0.1         207         0x80000004 0x003443 2
      0.0.0.2         0.0.0.2         206         0x80000002 0x00757C 3

                      Net Link States (Area 0)

      Link ID         ADV Router      Age         Seq#       Checksum
      120.0.255.14    0.0.0.1         207         0x80000001 0x0003AF

                      Summary Net Link States (Area 0)

      Link ID         ADV Router      Age         Seq#       Checksum
      120.0.0.0       0.0.0.1         761         0x80000001 0x006F53
      120.0.1.0       0.0.0.1         761         0x80000001 0x00645D



### MLS 3


***Config***

    MLS3#sh ip int brief
    Interface              IP-Address      OK? Method Status                Protocol
    Ethernet0/0            120.0.255.17    YES NVRAM  up                    up
    Ethernet0/1            120.0.255.10    YES NVRAM  up                    up
    Ethernet0/2            unassigned      YES unset  up                    up
    Ethernet0/3            unassigned      YES unset  up                    up
    Ethernet1/0            120.0.255.6     YES NVRAM  up                    up
    Ethernet1/1            unassigned      YES unset  up                    up
    Ethernet1/2            unassigned      YES unset  up                    up
    Ethernet1/3            unassigned      YES unset  up                    up
    Ethernet2/0            unassigned      YES unset  up                    up
    Ethernet2/1            unassigned      YES unset  up                    up
    Ethernet2/2            unassigned      YES unset  up                    up
    Ethernet2/3            unassigned      YES unset  up                    up
    Ethernet3/0            unassigned      YES unset  up                    up
    Ethernet3/1            unassigned      YES unset  up                    up
    Ethernet3/2            unassigned      YES unset  up                    up
    Ethernet3/3            unassigned      YES unset  up                    up
    Vlan1                  unassigned      YES unset  administratively down down


    MLS3(config)#ip routing
    MLS3(config)#router ospf 1
    MLS3(config-router)#router-id 0.0.0.3
    MLS3(config-router)#pass
    MLS3(config-router)#passive-interface default
    MLS3(config-router)#no passive-interface e0/0
    MLS3(config-router)#no passive-interface e0/1
    MLS3(config-router)#no passive-interface e1/0
    MLS3(config-router)#network 120.0.255.4 0.0.0.3 area 0                   >>>>> deuxieme methodes
    MLS3(config-router)#network 120.0.255.8 0.0.0.3 area 0
    MLS3(config-router)#network 120.0.255.16 0.0.0.3 area 0


***verification usage***

    MLS3#sh ip protocol

    *** IP Routing is NSF aware ***

    Routing Protocol is "ospf 1"
      Outgoing update filter list for all interfaces is not set
      Incoming update filter list for all interfaces is not set
      Router ID 0.0.0.3
      Number of areas in this router is 1. 1 normal 0 stub 0 nssa
      Maximum path: 4
      Routing for Networks:
        120.0.255.4 0.0.0.3 area 0
        120.0.255.8 0.0.0.3 area 0
        120.0.255.16 0.0.0.3 area 0
      Passive Interface(s):
        Vlan1
      Routing Information Sources:
        Gateway         Distance      Last Update
        0.0.0.1              110      00:00:46
        0.0.0.2              110      00:00:56
      Distance: (default is 110)


      MLS3#sh ip ospf int brief
      Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
      Et0/0        1     0               120.0.255.17/30    10    BDR   1/1
      Et0/1        1     0               120.0.255.10/30    10    BDR   1/1
      Et1/0        1     0               120.0.255.6/30     10    DR    0/0


***verification adjacence***

      MLS3#sh ip ospf neig

      Neighbor ID     Pri   State           Dead Time   Address         Interface
      0.0.0.1           1   FULL/DR         00:00:36    120.0.255.18    Ethernet0/0
      0.0.0.2           1   FULL/DR         00:00:39    120.0.255.9     Ethernet0/1


***verification des routes***


      MLS3#sh ip route

            120.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
      O IA     120.0.0.0/24 [110/11] via 120.0.255.18, 00:02:26, Ethernet0/0
      O IA     120.0.1.0/24 [110/11] via 120.0.255.18, 00:02:26, Ethernet0/0
      O        120.0.255.0/30 [110/20] via 120.0.255.9, 00:02:36, Ethernet0/1
      C        120.0.255.4/30 is directly connected, Ethernet1/0
      L        120.0.255.6/32 is directly connected, Ethernet1/0
      C        120.0.255.8/30 is directly connected, Ethernet0/1
      L        120.0.255.10/32 is directly connected, Ethernet0/1
      O        120.0.255.12/30 [110/20] via 120.0.255.18, 00:02:26, Ethernet0/0
                              [110/20] via 120.0.255.9, 00:02:36, Ethernet0/1
      C        120.0.255.16/30 is directly connected, Ethernet0/0
      L        120.0.255.17/32 is directly connected, Ethernet0/0

### R1

***Configuration***

R1#sh ip int brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            120.0.255.1     YES NVRAM  up                    up
Serial0/0                  80.0.0.2        YES NVRAM  up                    up
FastEthernet0/1            120.0.255.5     YES NVRAM  up                    up
Serial0/1                  unassigned      YES NVRAM  administratively down down
Serial0/2                  unassigned      YES NVRAM  administratively down down
Serial0/3                  unassigned      YES NVRAM  administratively down down
Serial0/4                  unassigned      YES NVRAM  administratively down down
Serial0/5                  unassigned      YES NVRAM  administratively down down
FastEthernet1/0            unassigned      YES NVRAM  administratively down down


NE PAS INTEGRER LE SERIAL


***configuration ospf***

R1(config)#router ospf 1
R1(config-router)#router-id 1.0.0.1
R1(config-router)#pass
R1(config-router)#passive-interface default
R1(config-router)#no passive-interface fa0/0
R1(config-router)#no passive-interface fa0/1
R1(config-router)#exit
R1(config)#int fa0/0
R1(config-if)#ip ospf 1 area 0
R1(config-if)#int fa0/1
R1(config-if)#ip ospf 1 area 0
 


--------------------------------------------------------------------------------- 

*** configuration route par defaut sur R1***

R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#ip route 0.0.0.0 0.0.0.0 s0/0


R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!

### ***integration d'une route dans ospf***


    R1#conf t
    R1(config)#router ospf 1
    R1(config-router)#default-information originate     (prends une information d'une autre source et l'integre)


    R1#sh ip ospf database

                OSPF Router with ID (1.0.0.1) (Process ID 1)

                    Router Link States (Area 0)

    Link ID         ADV Router      Age         Seq#       Checksum Link count
    0.0.0.1         0.0.0.1         1799        0x80000006 0x00895D 2
    0.0.0.2         0.0.0.2         814         0x80000005 0x00CA21 3
    0.0.0.3         0.0.0.3         927         0x80000006 0x005483 3
    1.0.0.1         1.0.0.1         85          0x80000007 0x00E82C 2

                    Net Link States (Area 0)

    Link ID         ADV Router      Age         Seq#       Checksum
    120.0.255.2     0.0.0.2         814         0x80000002 0x007A41
    120.0.255.6     0.0.0.3         927         0x80000002 0x00565F
    120.0.255.9     0.0.0.2         1577        0x80000002 0x00456E
    120.0.255.14    0.0.0.1         19          0x80000003 0x00FEB1
    120.0.255.18    0.0.0.1         1799        0x80000002 0x00E6C5

                    Summary Net Link States (Area 0)

    Link ID         ADV Router      Age         Seq#       Checksum
    120.0.0.0       0.0.0.1         1025        0x80000003 0x006B55
    120.0.1.0       0.0.0.1         1027        0x80000003 0x00605F

                    Type-5 AS External Link States

    Link ID         ADV Router      Age         Seq#       Checksum Tag
    0.0.0.0         1.0.0.1         88          0x80000001 0x002C84 1                      >>>> LSA TYPE 5


    verification sur MLS2

    MLS2#sh ip route

      Gateway of last resort is 120.0.255.1 to network 0.0.0.0

      O*E2  0.0.0.0/0 [110/1] via 120.0.255.1, 00:02:50, Ethernet1/0          >>>> aura tjrs une metric 1 car externe de type 2
            120.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
      O IA     120.0.0.0/24 [110/11] via 120.0.255.14, 01:09:40, Ethernet0/0
      O IA     120.0.1.0/24 [110/11] via 120.0.255.14, 01:09:40, Ethernet0/0
      C        120.0.255.0/30 is directly connected, Ethernet1/0
      L        120.0.255.2/32 is directly connected, Ethernet1/0
      O        120.0.255.4/30 [110/20] via 120.0.255.10, 01:01:46, Ethernet0/1
                              [110/20] via 120.0.255.1, 00:47:50, Ethernet1/0
      C        120.0.255.8/30 is directly connected, Ethernet0/1
      L        120.0.255.9/32 is directly connected, Ethernet0/1
      C        120.0.255.12/30 is directly connected, Ethernet0/0
      L        120.0.255.13/32 is directly connected, Ethernet0/0
      O        120.0.255.16/30 [110/20] via 120.0.255.14, 01:09:40, Ethernet0/0
                              [110/20] via 120.0.255.10, 01:01:36, Ethernet0/1

    Type de lsa 0 = lsa 1 O IA = lsa 2  O E2 = lsa 5


------------------------------------------------------------------------------------------------

Liste des differents reseau qu'ospf a appris ( pas necessairement dans la table de routage )


* + > = dans la table de routage

MLS2#sh ip ospf rib

            OSPF Router with ID (0.0.0.2) (Process ID 1)


                Base Topology (MTID 0)

OSPF local RIB
Codes: * - Best, > - Installed in global RIB

*>  120.0.0.0/24, Inter, cost 11, area 0
      via 120.0.255.14, Ethernet0/0
*>  120.0.1.0/24, Inter, cost 11, area 0
      via 120.0.255.14, Ethernet0/0
*   120.0.255.0/30, Intra, cost 10, area 0, Connected
      via 120.0.255.2, Ethernet1/0
*>  120.0.255.4/30, Intra, cost 20, area 0
      via 120.0.255.1, Ethernet1/0
      via 120.0.255.10, Ethernet0/1
*   120.0.255.8/30, Intra, cost 10, area 0, Connected
      via 120.0.255.9, Ethernet0/1
*   120.0.255.12/30, Intra, cost 10, area 0, Connected
      via 120.0.255.13, Ethernet0/0
*>  120.0.255.16/30, Intra, cost 20, area 0
      via 120.0.255.10, Ethernet0/1
      via 120.0.255.14, Ethernet0/0
*>  0.0.0.0/0, Ext2, cost 1, tag 1
      via 120.0.255.1, Ethernet1/0


-------------------------------------------------------------------------------------------------


### ***Summary***    SUR ABR

      MLS1#conf t
      MLS1(config)#router ospf 1
      MLS1(config-router)#area 10 range 120.0.0.0 255.255.254.0      >>>>>> information pour summaryser 


      MLS1#sh ip ospf database                                   plus qu'une ligne dans le summary
                      Summary Net Link States (Area 0)

      Link ID         ADV Router      Age         Seq#       Checksum
      120.0.0.0       0.0.0.1         51          0x80000005 0x00625D


***MLS1#sh ip ospf database sum 120.0.0.0***

                  OSPF Router with ID (0.0.0.1) (Process ID 1)

                      Summary Net Link States (Area 0)

        LS age: 113
        Options: (No TOS-capability, DC, Upward)
        LS Type: Summary Links(Network)
        Link State ID: 120.0.0.0 (summary Network Number)
        Advertising Router: 0.0.0.1
        LS Seq Number: 80000005
        Checksum: 0x625D
        Length: 28
        Network Mask: /23
              MTID: 0         Metric: 1




      MLS2# sh ip ospf rib

      OSPF local RIB
      Codes: * - Best, > - Installed in global RIB

      *>  120.0.0.0/23, Inter, cost 11, area 0
            via 120.0.255.14, Ethernet0/0
      *   120.0.255.0/30, Intra, cost 10, area 0, Connected
            via 120.0.255.2, Ethernet1/0
      *>  120.0.255.4/30, Intra, cost 20, area 0
            via 120.0.255.1, Ethernet1/0
            via 120.0.255.10, Ethernet0/1
      *   120.0.255.8/30, Intra, cost 10, area 0, Connected
            via 120.0.255.9, Ethernet0/1
      *   120.0.255.12/30, Intra, cost 10, area 0, Connected
            via 120.0.255.13, Ethernet0/0
      *>  120.0.255.16/30, Intra, cost 20, area 0
            via 120.0.255.10, Ethernet0/1
            via 120.0.255.14, Ethernet0/0
      *>  0.0.0.0/0, Ext2, cost 1, tag 1
            via 120.0.255.1, Ethernet1/0



        MLS2#sh ip route


        O*E2  0.0.0.0/0 [110/1] via 120.0.255.1, 00:35:11, Ethernet1/0
              120.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
        O IA     120.0.0.0/23 [110/11] via 120.0.255.14, 00:03:52, Ethernet0/0



--------------------------------------------------------------------------------------------------------



### ***Changer la bande passante d'une interface***


MLS1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
MLS1(config)#int e1/1
MLS1(config-if)#bandwidth 100000


MLS2#conf t
MLS2(config)#int e0/0
MLS2(config-if)#bandwidth 100000

----------------------------------------------------------------------------------------------------

### ***Changer le cout par defaut ospf***


R1#conf t
R1(config)#router ospf 1
R1(config-router)#auto-cost reference-bandwidth 1000000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.




### ROUTE PAR DEFAUT CFG ET VERIFICATION


      R1(config)#router ospf 1
      R1(config-router)#default-information originate


      sh ip ospf database external 0.0.0.0

