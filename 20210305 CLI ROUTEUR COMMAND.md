---
title: 20210305 CLI ROUTEUR COMMAND
created: '2021-03-06T10:18:31.411Z'
modified: '2021-03-09T10:05:37.776Z'
---

# ***20210305*** CLI ROUTEUR COMMAND

## ***exercices de début de matinée***

##### ***REMISE A ZERO***
    erase nvram:
    reload ( pas sauvegarder la modif puisqu'on la vide)

##### ***CONFIGURATION DE BASE(configuration terminal)***
    (config)# hostname <....>
    (config)# enable secret <....> [user exec a priviligié]
    (config)# service password-encryption
    (config)# line console 0
    (config-line)# exec-timeout <0> <0> ( desactiver timeout session)
    (config-line)# logging synchronous (desactive la separation de la ligne de commande)
    (config-line)# password <....> (mdps = admin)
    (config-line)# login

***********************************************************************************************************************************

##### ***SAUVEGUARDE***
    exemple : P3R2#:copy running-config startup-config (attention au erreurs!!!!)
    Password: = admin
    P3R2>enable
    Password: cisco
    P3R2#

###### ***VERIFICATION DE LA CONFIG :***

    exemple : P3R2# show running-config
    afficher la configuration actuelle

***********************************************************************************************************************************

###### ***COMMANDE INFORMATIVE :***

    show version ==== information générique de la version du routeur
    show flash: afficher les fichiers dans la flash

***********************************************************************************************************************************

    show ip interface brief (interface du routeur en bref)


    P3R2#show ip interface brief
    Interface       IP-Address OK? Method Status                Protocol         
    FastEthernet0/0 unassigned YES unset  administratively down down             
    FastEthernet0/1 unassigned YES unset  administratively down down             
    Serial0/1/0     unassigned YES unset  administratively down down            
    Serial0/1/1     unassigned YES unset  administratively down down            
    SSLVPN-VIF0     unassigned NO  unset                   up   up
    P3R2#
                                          L1                    l2

| Status | Protocol |
| ------ | -------- | 
| down   |   down   |  >>>>>>>>>>>>>>>> pas de cable ou pas de courant
| up     |   down   |  >>>>>>>>>>>>>>>> mauvais protocol [serial],clock(mauvaise freq)[serial],auth [serial & ethernet]
| up     |    up    |  >>>>>>>>>>>>>>>> okay!


##### ***SHOW INTERFACES*** (informations des interfaces)
    show interfaces fa0/0
    FastEthernet0/0 is administratively down, line protocol is down
    Hardware is Gt96k FE, address is 001e.7a4c.a88e (bia 001e.7a4c.a88e) >>>>>> mac adress
    MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec, >>>>>> mtu, bandwitch, delay (valeur abstraite)
    reliability 255/255, txload 1/255, rxload 1/255 >>>>>> compteur de fiabilité, charge emission, charge reception plus valeur elevé plus charge elevé
    Encapsulation ARPA, loopback not set >>>>>> protocol ethernet ancien nom ( arpa )
    Keepalive set (10 sec) >>>>>>
    Auto-duplex, Auto Speed, 100BaseTX/FX >>>>>>
    ARP type: ARPA, ARP Timeout 04:00:00
    Last input never, output never, output hang never
    Last clearing of "show interface" counters never
    Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
    Queueing strategy: fifo
    Output queue: 0/40 (size/max)
    5 minute input rate 0 bits/sec, 0 packets/sec
    5 minute output rate 0 bits/sec, 0 packets/sec
    0 packets input, 0 bytes >>>>>> compteur de statistiques
    Received 0 broadcasts, 0 runts, 0 giants, 0 throttles >>>>>> runts(trame anormalement petite doit rester a 0),giants(anormalement grande),throttles(emission postposé)
    symptom problems hardware
    0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored >>>>>>
    0 watchdog >>>>>>
    0 input packets with dribble condition detected >>>>>>
    7 packets output, 420 bytes, 0 underruns >>>>>>
    0 output errors, 0 collisions, 0 interface resets >>>>>>
    0 unknown protocol drops >>>>>> n'envoie pas de trame parceque ne sait pas ce que c'est
    0 babbles, 0 late collision, 0 deferred >>>>>> colision tardive en dehors des 64 premier bytes (peu signifier un problem d'half duplex/fullduplex)
    0 lost carrier, 0 no carrier >>>>>>
    0 output buffer failures, 0 output buffers swapped out >>>>>>

***********************************************************************************************************************************

##### ***CONFIGURATION INTERFACE***

    P3R2#conf t
    Enter configuration commands, one per line. End with CNTL/Z.
    P3R2(config)#interface f0/0 ou fastethernet0/0 ou fastethernet 0/0

    P3R2(config-if)#duplex ?
    auto Enable AUTO duplex configuration
    full Force full duplex operation
    half Force half-duplex operation
    P3R2(config-if)#duplex full

    P3R2(config-if)#speed ?
    10 Force 10 Mbps operation
    100 Force 100 Mbps operation
    auto Enable AUTO speed configuration
    P3R2(config-if)#speed 100

***ANNULE SPEED***
    p3R2(config-if)#no speed

***DESCRIPTION***
    P3R2(config-if)#description (permet de donner une description,permet de reconstruire plus rapide //documentation)

    P3R2(config-if)#ip address ?
    A.B.C.D IP address
    dhcp IP Address negotiated via DHCP
    pool IP Address autoconfigured from a local DHCP pool

**10.0.0.xy /24 xy = P3R2 xy = 32 CONVENTION ETABLI PAR STEVE !**

    P3R2(config-if)#ip address 10.0.0.32 255.255.255.0?
    secondary Make this IP address a secondary address

    P3R2(config-if)#ip address 10.0.0.32 255.255.255.0
    verification de la description et adresse tappé (bonne adresse et bon masque)

    P3R2#show ip int brief
    Interface IP-Address OK? Method Status Protocol
    FastEthernet0/0 10.0.0.32 YES manual administratively down down
    FastEthernet0/1 unassigned YES unset administratively down down
    Serial0/1/0 unassigned YES unset administratively down down
    Serial0/1/1 unassigned YES unset administratively down down
    SSLVPN-VIF0 unassigned NO unset up up

**METHODE 1**

    P3R2#show int fa0/0

    FastEthernet0/0 is administratively down, line protocol is down
    Hardware is Gt96k FE, address is 001e.7a4c.a88e (bia 001e.7a4c.a88e)
    Description: Connexion LAN
    Internet address is 10.0.0.32/24
    MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
    reliability 255/255, txload 1/255, rxload 1/255
    Encapsulation ARPA, loopback not set
    Keepalive set (10 sec)
    Full-duplex, Auto Speed, 100BaseTX/FX
    ARP type: ARPA, ARP Timeout 04:00:00
    Last input never, output never, output hang never
    Last clearing of "show interface" counters never
    Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
    Queueing strategy: fifo
    Output queue: 0/40 (size/max)
    5 minute input rate 0 bits/sec, 0 packets/sec
    5 minute output rate 0 bits/sec, 0 packets/sec
    0 packets input, 0 bytes
    Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
    0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
    0 watchdog
    0 input packets with dribble condition detected
    7 packets output, 420 bytes, 0 underruns
    0 output errors, 0 collisions, 0 interface resets
    0 unknown protocol drops
    0 babbles, 0 late collision, 0 deferred
    0 lost carrier, 0 no carrier
    0 output buffer failures, 0 output buffers swapped out

**METHODE 2**

    P3R2#show ip interface fa0/0 ( plus concentrer sur le niveau 3 = show ip interface brief mais en complet )

    FastEthernet0/0 is administratively down, line protocol is down
    Internet address is 10.0.0.32/24
    Broadcast address is 255.255.255.255
    Address determined by setup command
    MTU is 1500 bytes
    Helper address is not set
    Directed broadcast forwarding is disabled
    Outgoing access list is not set
    Inbound access list is not set
    Proxy ARP is enabled
    Local Proxy ARP is disabled
    Security level is default
    Split horizon is enabled
    ICMP redirects are always sent
    ICMP unreachables are always sent
    ICMP mask replies are never sent
    IP fast switching is enabled
    IP fast switching on the same interface is disabled
    IP Flow switching is disabled
    IP CEF switching is enabled
    IP CEF switching turbo vector
    IP multicast fast switching is enabled
    IP multicast distributed fast switching is disabled
    IP route-cache flags are Fast, CEF
    Router Discovery is disabled
    IP output packet accounting is disabled
    IP access violation accounting is disabled
    TCP/IP header compression is disabled
    RTP/IP header compression is disabled
    Policy routing is disabled
    Network address translation is disabled
    BGP Policy Mapping is disabled
    Input features: MCI Check
    WCCP Redirect outbound is disabled
    WCCP Redirect inbound is disabled
    WCCP Redirect exclude is disabled

  **METHODE 3**  

    P3R2#show protocols

    Global values:
    Internet Protocol routing is enabled
    FastEthernet0/0 is administratively down, line protocol is down
    Internet address is 10.0.0.32/24
    FastEthernet0/1 is administratively down, line protocol is down
    Serial0/1/0 is administratively down, line protocol is down
    Serial0/1/1 is administratively down, line protocol is down
    SSLVPN-VIF0 is up, line protocol is up
    Interface is unnumbered. Using address of SSLVPN-VIF0 (0.0.0.0)

***********************************************************************************************************************************

##### ***CREATION DE ROUTE***

    P3R2#conf t
    Enter configuration commands, one per line. End with CNTL/Z.
    P3R2(config)#ip route 14.14.14.14 255.255.255.255 10.0.0.14
    destination masque next-hop

    P3R2#sh ip route 14.14.14.14
    Routing entry for 14.14.14.14/32
    Known via "static", distance 1, metric 0
    Routing Descriptor Blocks:

    10.0.0.14 ( il connais la route )
    Route metric is 0, traffic share count is 1

    ajouté une adresse ip dans la table de routage : 
    ex P3R2(config)#ip route 10.14.0.0 255.255.254.0 172.16.0.1
                   *ip route adresse reseau|masque reseau|next hop*

            
***********************************************************************************************************************************


##### **INTERPRETATION COMMANDE PING**

    P3R2#ping 10.0.0.11

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 10.0.0.11, timeout is 2 seconds:
    ..... 
    Success rate is 0 percent (0/5)

**ce qui peut etre a la place des point**
**! = ok exemple:!!!!!**
**. = faill**
**h (hote) ou u(reseau) = pas de route pas de paquet ( pas joignable )**

***********************************************************************************************************************************

##### ***ALLUMER INTERFACE***
    P3R2#conf t
    Enter configuration commands, one per line. End with CNTL/Z.
    P3R2(config)#int fa0/0
    P3R2(config-if)#no shutdown

    *viré duplex_missmatch* ( half et full pas de negociation)
    P3R2(config)#int fa0/0
    P3R2(config-if)#no speed
    P3R2(config-if)#no duplex
    P3R2(config-if)#
    *Mar 5 10:01:01.235: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up ( quand changement de vitesse interface redemarre )
  
##### ***VOIR TABLE ARP***
  
    P3R2#show arp
    Protocol Address Age (min) Hardware Addr Type Interface
    Internet 10.0.0.11 2 0015.fad7.f400 ARPA FastEthernet0/0
    Internet 10.0.0.21 1 0025.845d.bc20 ARPA FastEthernet0/0
    Internet 10.0.0.32 - 001e.7a4c.a88e ARPA FastEthernet0/0
    Internet 10.0.0.33 3 001e.130b.ffe6 ARPA FastEthernet0/0

##### ***PURGE TABLE ARP***

** 1 METHODE**
  
    P3R2#clear arp-cache
    P3R2#show arp
    Protocol Address Age (min) Hardware Addr Type Interface
    Internet 10.0.0.11 0 0015.fad7.f400 ARPA FastEthernet0/0
    Internet 10.0.0.21 0 0025.845d.bc20 ARPA FastEthernet0/0
    Internet 10.0.0.32 - 001e.7a4c.a88e ARPA FastEthernet0/0
    Internet 10.0.0.33 0 001e.130b.ffe6 ARPA FastEthernet0/0
    age = temps avant expiration

  **2 METHODE**

    P3R2#clear ip arp 10.0.0.11
    P3R2#show arp
    Protocol Address Age (min) Hardware Addr Type Interface
    Internet 10.0.0.11 0 0015.fad7.f400 ARPA FastEthernet0/0
    Internet 10.0.0.21 1 0025.845d.bc20 ARPA FastEthernet0/0
    Internet 10.0.0.32 - 001e.7a4c.a88e ARPA FastEthernet0/0
    Internet 10.0.0.33 0 001e.130b.ffe6 ARPA FastEthernet0/0
***********************************************************************************************************************************

##### ***SHOW IP TABLE***

    P3R2#show ip route
    Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
    D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
    N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
    E1 - OSPF external type 1, E2 - OSPF external type 2
    i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
    ia - IS-IS inter area, * - candidate default, U - per-user static route
    o - ODR, P - periodic downloaded static route

    Gateway of last resort is not set
    content_copy

    10.0.0.0/24 is subnetted, 1 subnets

    C 10.0.0.0 is directly connected, FastEthernet0/0
    vlsm = variable lenght subnet mask a retenir

***********************************************************************************************************************************

##### ***CREER UNE INTERFACE LOOPBAK***
    P3R2#conf t
    Enter configuration commands, one per line. End with CNTL/Z.
    P3R2(config)#interface
    P3R2(config)#interface lo
    P3R2(config)#interface loopback ?
    <0-2147483647> Loopback interface number

    P3R2(config)#interface loopback 0
    P3R2(config-if)#
    *Mar 5 10:15:19.799: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up (toujours actif par defaut , car pas de materiel dont l'interface depends)
    P3R2(config-if)#ip address 32.32.32.32 255.255.255.255
    P3R2(config-if)#^Z
    P3R2#show ip interface brief
    Interface IP-Address OK? Method Status Protocol
    FastEthernet0/0 10.0.0.32 YES manual up up
    FastEthernet0/1 unassigned YES unset administratively down down
    Serial0/1/0 unassigned YES unset administratively down down
    Serial0/1/1 unassigned YES unset administratively down down
    SSLVPN-VIF0 unassigned NO unset up up
    Loopback0 32.32.32.32 YES manual up up
    P3R2#show ip route
    Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
    D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
    N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
    E1 - OSPF external type 1, E2 - OSPF external type 2
    i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
    ia - IS-IS inter area, * - candidate default, U - per-user static route
    o - ODR, P - periodic downloaded static route

    Gateway of last resort is not set
    content_copy

    32.0.0.0/32 is subnetted, 1 subnets

    C 32.32.32.32 is directly connected, Loopback0
    10.0.0.0/24 is subnetted, 1 subnets
    C 10.0.0.0 is directly connected, FastEthernet0/0

    P3R2#ping 14.14.14.14

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 14.14.14.14, timeout is 2 seconds:
    .....
    Success rate is 0 percent (0/5)
    P3R2#show ip route
    Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
    D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
    N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
    E1 - OSPF external type 1, E2 - OSPF external type 2
    i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
    ia - IS-IS inter area, * - candidate default, U - per-user static route
    o - ODR, P - periodic downloaded static route

    Gateway of last resort is not set
    content_copy

    32.0.0.0/32 is subnetted, 1 subnets

    C 32.32.32.32 is directly connected, Loopback0
    10.0.0.0/24 is subnetted, 1 subnets
    C 10.0.0.0 is directly connected, FastEthernet0/0

**TROUBLESHOOTING**

    P3R2#show ip route 14.14.14.14
    % Network not in table

***********************************************************************************************************************************






