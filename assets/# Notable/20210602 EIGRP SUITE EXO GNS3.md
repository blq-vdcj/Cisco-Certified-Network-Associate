---
title: 20210602 EIGRP SUITE EXO GNS3
created: '2021-06-02T08:21:59.913Z'
modified: '2021-06-02T11:48:06.019Z'
---

# 20210602 EIGRP SUITE EXO GNS3


### ***summariser des routes*** (exercices gns eigrp) ( peut privilegié un reseau plutot qu'un autre si pas fait sur les deux interfaces)

!!!!! Configurer l'interface qui vas vers le routeur qui necessite le summary!!!!!!!!


    R4(config)#int fa0/0
    R4(config-if)#ip summary-address eigrp 350 10.0.0.0 255.255.252.0

ex : sh ip route
    
    D       10.0.0.0/22 is a summary, 00:00:32, Null0

    R4(config)#int fa0/1
    R4(config-if)#ip summary-address eigrp 350 10.0.0.0 255.255.252.0


### ***verification sur R1***


    R1#sh ip route
    Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route

    Gateway of last resort is 0.0.0.0 to network 0.0.0.0

        118.0.0.0/30 is subnetted, 1 subnets
    C       118.218.118.216 is directly connected, Serial0/2
        172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
    C       172.16.0.20/30 is directly connected, Serial0/1
    C       172.16.0.16/30 is directly connected, Serial0/0
    D       172.16.0.8/29 [90/857600] via 172.16.0.22, 00:26:33, Serial0/1
                          [90/1177600] via 172.16.0.18, 00:26:33, Serial0/0
    D       172.16.0.0/29 [90/857600] via 172.16.0.22, 00:26:33, Serial0/1
                          [90/1177600] via 172.16.0.18, 00:26:34, Serial0/0
        10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
    D       10.0.0.0/22 [90/985600] via 172.16.0.22, 00:02:32, Serial0/1
                        [90/1305600] via 172.16.0.18, 00:02:32, Serial0/0

### summary suite r3 r2


    R3#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    R3(config)#int s0/0
    R3(config-if)#ip summary-address eigrp 350 10.0.0.0 255.255.248.0
    R3(config-if)#
    *Mar  1 00:28:31.643: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.17 (Serial0/0) is resync: summary configured
    R3(config-if)#ip summary-address eigrp 350 172.16.0.0 255.255.255.240



    R2#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    R2(config)#int s0/0
    R2(config-if)#ip summary-address eigrp 350 10.0.0.0 255.255.248.0
     Neighbor 172.16.0.21 (Serial0/0) is resync: summary configured
    R2(config-if)#ip summary-address eigrp 350 172.16.0.0 255.255.255.240


### ***Verification sur R1*** ( table de routage la plus optimisé car planification bien preparer)

    R1#sh ip route
    Gateway of last resort is 0.0.0.0 to network 0.0.0.0

        118.0.0.0/30 is subnetted, 1 subnets
    C       118.218.118.216 is directly connected, Serial0/2
        172.16.0.0/16 is variably subnetted, 3 subnets, 2 masks
    C       172.16.0.20/30 is directly connected, Serial0/1
    C       172.16.0.16/30 is directly connected, Serial0/0
    D       172.16.0.0/28 [90/857600] via 172.16.0.22, 00:00:47, Serial0/1
                          [90/1177600] via 172.16.0.18, 00:00:47, Serial0/0
        10.0.0.0/21 is subnetted, 1 subnets
    D       10.0.0.0 [90/985600] via 172.16.0.22, 00:01:02, Serial0/1
                    [90/1305600] via 172.16.0.18, 00:01:02, Serial0/0
    S*   0.0.0.0/0 is directly connected, Serial0/2


### ***changer les variables***

      R1#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      R1(config)#router ei
      R1(config)#router eigrp 350
      R1(config-router)#metric wei
      R1(config-router)#metric weights 0
      % Incomplete command.

      R1(config-router)#metric weights 0 ?
        <0-255>  K1

      R1(config-router)#metric weights 0 0 0 1 0 0
      R1(config-router)#
      *Mar  1 00:50:19.795: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.18 (Serial0/0) is down: metric changed
      *Mar  1 00:50:19.795: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.22 (Serial0/1) is down: metric changed
      *Mar  1 00:50:20.739: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.18 (Serial0/0) is down: K-value mismatch
      *Mar  1 00:50:25.043: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.18 (Serial0/0) is down: K-value mismatch
      R1(config-router)#
      *Mar  1 00:50:27.235: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.22 (Serial0/1) is down: K-value mismatch
      R1(config-router)#
      *Mar  1 00:50:29.615: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 350: Neighbor 172.16.0.18 (Serial0/0) is down: K-value mismatch


### ***ajuster les intervalles entre les hellos***

      R1#conf t
      R1(config)#int s0/0
      R1(config-if)#ip hello
      R1(config-if)#ip hello-interval eigrp 350 ?
        <1-65535>  Seconds between hello transmissions
      R1(config-if)#ip hello-interval eigrp 350 1

***Ajuster la charge de la bande passante communiquer***
      
      R1(config-if)#ip bandwidth-percent eigrp 350 ?
      <1-999999>  Maximum bandwidth percentage that EIGRP may use
      R1(config-if)#ip bandwidth-percent eigrp 350

***verification***

      R1#sh ip eigrp interfaces detail s0/0
      IP-EIGRP interfaces for process 350

                              Xmit Queue   Mean   Pacing Time   Multicast    Pending
      Interface        Peers  Un/Reliable  SRTT   Un/Reliable   Flow Timer   Routes
      Se0/0              1        0/0        28       0/12         138           0
        Hello interval is 1 sec
        Next xmit serial <none>
        Un/reliable mcasts: 0/0  Un/reliable ucasts: 26/26
        Mcast exceptions: 0  CR packets: 0  ACKs suppressed: 1
        Retransmissions sent: 0  Out-of-sequence rcvd: 1
        Authentication mode is not set
        Use unicast


