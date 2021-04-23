# 20210318  routage inter VLAN switch layer 3

##### ***activation routage sur switch layer3***

    MLS1: conf t
    MLS1(config)#ip routing                (commande pour activer le layer 3 en ipv4)
    MLS1(config)#ipv6 unicast              (commande pour activer le layer 3 en ipv6)


##### ***CREATION INTERFACE VLAN***

    MLS1(config)#int vlan 100
    MLS1(config-if)#ip address 172.16.100.1 255.255.255.0
    MLS1(config-if)#int vlan 101
    MLS1(config-if)#
    %LINK-5-CHANGED: Interface Vlan101, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan101, changed state to up
    ip address 172.16.101.1 255.255.255.0

##### ***verification***

    MLS1#sh ip route
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
          i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
          * - candidate default, U - per-user static route, o - ODR
          P - periodic downloaded static route

    Gateway of last resort is not set

        172.16.0.0/24 is subnetted, 2 subnets
    C       172.16.100.0 is directly connected, Vlan100
    C       172.16.101.0 is directly connected, Vlan101


##### ***interconnexion entre layer 3***

    MLS1#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    MLS1(config)#int g0/2
    MLS1(config-if)#no switchport
    MLS1(config-if)#
    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

    MLS1(config-if)#ip add 172.16.0.1 255.255.255.252


##### ***verficiation***

    sh ip int brief
    Interface              IP-Address      OK? Method Status                Protocol 
    FastEthernet0/1        unassigned      YES unset  administratively down down 
    FastEthernet0/2        unassigned      YES unset  administratively down down 
    FastEthernet0/3        unassigned      YES unset  administratively down down 
    FastEthernet0/4        unassigned      YES unset  administratively down down 
    FastEthernet0/5        unassigned      YES unset  administratively down down 
    FastEthernet0/6        unassigned      YES unset  administratively down down 
    FastEthernet0/7        unassigned      YES unset  administratively down down 
    FastEthernet0/8        unassigned      YES unset  administratively down down 
    FastEthernet0/9        unassigned      YES unset  administratively down down 
    FastEthernet0/10       unassigned      YES unset  administratively down down 
    FastEthernet0/11       unassigned      YES unset  administratively down down 
    FastEthernet0/12       unassigned      YES unset  administratively down down 
    FastEthernet0/13       unassigned      YES unset  administratively down down 
    FastEthernet0/14       unassigned      YES unset  administratively down down 
    FastEthernet0/15       unassigned      YES unset  administratively down down 
    FastEthernet0/16       unassigned      YES unset  administratively down down 
    FastEthernet0/17       unassigned      YES unset  administratively down down 
    FastEthernet0/18       unassigned      YES unset  administratively down down 
    FastEthernet0/19       unassigned      YES unset  administratively down down 
    FastEthernet0/20       unassigned      YES unset  administratively down down 
    FastEthernet0/21       unassigned      YES unset  administratively down down 
    FastEthernet0/22       unassigned      YES unset  administratively down down 
    FastEthernet0/23       unassigned      YES unset  administratively down down 
    FastEthernet0/24       unassigned      YES unset  administratively down down 
    GigabitEthernet0/1     unassigned      YES unset  up                    up 
    GigabitEthernet0/2     172.16.0.1      YES manual up                    up 
    Vlan1                  unassigned      YES unset  administratively down down 
    Vlan100                172.16.100.1    YES manual up                    up 
    Vlan101                172.16.101.1    YES manual up                    up


    MLS1#sh ip route
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
          i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
          * - candidate default, U - per-user static route, o - ODR
          P - periodic downloaded static route

    Gateway of last resort is not set

        172.16.0.0/16 is variably subnetted, 3 subnets, 2 masks
    C       172.16.0.0/30 is directly connected, GigabitEthernet0/2
    C       172.16.100.0/24 is directly connected, Vlan100
    C       172.16.101.0/24 is directly connected, Vlan101


##### ***creation de route***

    MLS2(config)#ip route 172.16.100.0 255.255.254.0 172.16.0.1
    MLS1(config)#ip route 172.16.102.0 255.255.254.0 172.16.0.2



