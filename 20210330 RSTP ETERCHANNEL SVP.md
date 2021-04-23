---
attachments: [Clipboard_2021-03-30-09-34-55.png, Clipboard_2021-03-30-09-37-39.png, Clipboard_2021-03-30-09-42-09.png, Clipboard_2021-03-30-10-07-35.png, Clipboard_2021-03-30-11-01-09.png]
title: 20210330 RSTP ETERCHANNEL SVP
created: '2021-03-30T07:12:46.039Z'
modified: '2021-03-30T10:12:44.627Z'
---

# 20210330 RSTP ETERCHANNEL SVP


## RSTP

    RSTP = rapid spanning-tree
    topologie convergé = topologie en etat stable 
    IEEE 802.1w protocol standar
    
    Cisco > Pvrstp+ 
            PER VLAN RAPID SPANNING-TREE +
    equilibrer la charge vlan par vlan

    priorité par increment de 4096 comme STP

    BPDU encapsulé dans une trame ethernet standar
    
    Commun a spanning-tree
    - Meme logique éléction root bridge ( bid etc)
    - Meme logique Détermination des Root Port ( rp dp)
    - Meme logique determination des port block ( BLK )
    
    Differences a Spanning-tree
    - RSTP définit un port alternate (port qui est potentielment chemin d'acces au root bridge de secour) port blk qui peut devenir FWD
    - RSTP permet le remplacement immédiat des DP (remplacement port defaillant) 
    - Timer sont plus court que stp ( 2s a 6 sec contre 30 sec stp normal)
    
-----------------------------------------------------------------------------------

    Roles des port:
    RP root port ( idem stp )  meilleur chemin vers le root
    DP designated port ( Idem stp )  Port quelconque qui peut forwarder du traffic
    Disabled ( Status down )
    Alternate ( Chemin alternatif vers le root )
    Backup port ( remplacent des Dp secondaires ) tres peu probable d'en rencontrer un jour
    Edge = port d'acces configurer avec le portfast

    Etapes :
    Point to point  >>>> Full-duplex
    Shared          >>>> Half-duplex



  ![](@attachment/Clipboard_2021-03-30-09-34-55.png)
  ![](@attachment/Clipboard_2021-03-30-09-37-39.png) 
  on ne fait plus ca a l'heure actuelle
  ![](@attachment/Clipboard_2021-03-30-09-42-09.png)
  interface full duplex edge p2p Edge


 ----------------------------------------------------------------------------------


Transitions des ports      ( plus de phases d'attente des bpdu )


    - Discarding  = blk ou shut 
    - Learning   = negotiation avec les voisin
    - Forwarding 
 


 ----------------------------------------------------------------------------------
 Difference entre les stp

    Stp (802.10)   1Cst (commun spanning-tree) = une arborescende spanning-tree pour l'ensemble des vlans
    Pvstp+         1cst par vlan 
    RSTP (802.1w)  1cst pr tt les vlans 
    Pvrstp         1cst par vlan 
    Mst(p)(802.1s) 1 ou + cst par groupe de vlan



    (Config)#spanning-tree mode <PVST,RAPID-PVST,MST>
    rapid spanning tree pour fonctionner demande a tt les switch de la topologie d etre en rstp


-------------------------------------------------------------------------------------

## Etherchannel

![](@attachment/Clipboard_2021-03-30-10-07-35.png)

    Int dans port channel

    - Même vitesses
    - Même duplex
    - Même mode ( acces ou trunk )
    - Même Vlan NATIF par defaut le vlan1 mais peut etre changer
    - Même Vlan qui transitent


    Portchannel peut etre lvl 2 / lvl 3  

    Eterchannel Statique  :  
    Eterchannel Dynamique :LACP 802.3ad link agregation control protocol ( le plus employé a l'heure actuelle )
                           PAgP proprietaire cisco Port agregation protocol


    Eterchannel = 8 Interfaces Maximum Actives


### ***Equilibrage de charges sur les liaisons***


![](@attachment/Clipboard_2021-03-30-11-01-09.png)

### ***Regles choix pr l'equilibrage***

      - Src-MAc ( source Mac)
      - DST-MAC ( destination Mac)
      - Src-dest-Mac ( source destination mac)    (XOR)
      - Src-ip ( source ip)
      - Dst-ip ( destination ip )
      - Src-dst-ip ( source destination ip )
      - Src-port  (source port)
      - Dst-port  (destination port)
      - SRC-DST-PORT (source destination port)

----------------------------------------------------------------------------------------------------

## ***Exercices Eterchannel***             travailler en range interface

    2960-7#sh cdp neigh
    Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                      S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone,
                      D - Remote, C - CVTA, M - Two-port Mac Relay

    Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
    2960-8           Fas 0/2           140              S I   WS-C2960- Fas 0/2
    2960-8           Fas 0/1           131              S I   WS-C2960- Fas 0/1


    2960-7#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    2960-7(config)#int vlan 1
    2960-7(config-if)#ip add 192.168.0.1 255.255.255.0
    2960-7(config-if)#no sh
    2960-7(config-if)#

#### ***1ere methode*** Static

    2960-7#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    2960-7(config)#int range fa0/1 - 2
    2960-7(config-if-range)#channel-group ?            uniquement groupe local
      <1-6>  Channel group number
    2960-7(config-if-range)#channel-group 

    2960-7(config-if-range)#channel-group 1 mode ?
      active     Enable LACP unconditionally
      auto       Enable PAgP only if a PAgP device is detected
      desirable  Enable PAgP unconditionally
      on         Enable Etherchannel only
      passive    Enable LACP only if a LACP device is detected


    2960-7(config-if-range)#channel-group 1 mode on
    Creating a port-channel interface Port-channel 1

    2960-7(config-if-range)#
    *Mar  1 00:17:36.763: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
    2960-7(config-if-range)#
    *Mar  1 00:17:38.751: %LINK-3-UPDOWN: Interface Port-channel1, changed state to up
    *Mar  1 00:17:39.757: %LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel1, changed state to up

***Commande principal verificiation***

    2960-7#sh etherchannel summary
    Flags:  D - down        P - bundled in port-channel
            I - stand-alone s - suspended
            H - Hot-standby (LACP only)
            R - Layer3      S - Layer2
            U - in use      f - failed to allocate aggregator

            M - not in use, minimum links not met
            u - unsuitable for bundling
            w - waiting to be aggregated
            d - default port


    Number of channel-groups in use: 1
    Number of aggregators:           1

    Group  Port-channel  Protocol    Ports
    ------+-------------+-----------+-----------------------------------------------
    1      Po1(SU)          -        Fa0/1(P)    Fa0/2(P)
    
    1 groupe | (su = switch in use) | pas de protocol | interface qui font partie de l'etherchannel


    2960-7#sh span      (plusieur lien entre deux equipement agreger les : Layer2)

      VLAN0001
        Spanning tree enabled protocol ieee
        Root ID    Priority    32769
                  Address     001d.712a.c180
                  Cost        12
                  Port        64 (Port-channel1)
                  Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

        Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                  Address     e840.4017.5f00
                  Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
                  Aging Time  300 sec

      Interface           Role Sts Cost      Prio.Nbr Type
      ------------------- ---- --- --------- -------- --------------------------------
      Po1                 Root FWD 12        128.64   P2p

*** creation trunk***

      2960-7(config)#int po1
      2960-7(config-if)#sw mode trunk


      2960-7#sh int trunk
      Port        Mode             Encapsulation  Status        Native vlan
      Po1         on               802.1q         trunking      1

      Port        Vlans allowed on trunk
      Po1         1-4094

      Port        Vlans allowed and active in management domain
      Po1         1

      Port        Vlans in spanning tree forwarding state and not pruned
      Po1         1

      2960-7#sh int po1 switchport
        Name: Po1
        Switchport: Enabled
        Administrative Mode: trunk
        Operational Mode: trunk
        Administrative Trunking Encapsulation: dot1q
        Operational Trunking Encapsulation: dot1q
        Negotiation of Trunking: On
        Access Mode VLAN: 1 (default)
        Trunking Native Mode VLAN: 1 (default)
        Administrative Native VLAN tagging: enabled
        Voice VLAN: none
        Administrative private-vlan host-association: none
        Administrative private-vlan mapping: none
        Administrative private-vlan trunk native VLAN: none
        Administrative private-vlan trunk Native VLAN tagging: enabled
        Administrative private-vlan trunk encapsulation: dot1q
        Administrative private-vlan trunk normal VLANs: none
        Administrative private-vlan trunk associations: none
        Administrative private-vlan trunk mappings: none
        Operational private-vlan: none
        Trunking VLANs Enabled: ALL
        Pruning VLANs Enabled: 2-1001

        Protected: false  >>>> peut etre activer sur switch , port protected ne peuvent pas communiquer entre eux (exemple : personne public qui viens se connecter sur le reseau pas de comm entre eux)
        Unknown unicast blocked: disabled
        Unknown multicast blocked: disabled
        Appliance trust: none


#### deconfigurer etherchannel

    2960-7#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      2960-7(config)#default interface range fa0/1 - 2


    2960-7#sh ether sum
      Flags:  D - down        P - bundled in port-channel
              I - stand-alone s - suspended
              H - Hot-standby (LACP only)
              R - Layer3      S - Layer2
              U - in use      f - failed to allocate aggregator

              M - not in use, minimum links not met
              u - unsuitable for bundling
              w - waiting to be aggregated
              d - default port


      Number of channel-groups in use: 1
      Number of aggregators:           1

      Group  Port-channel  Protocol    Ports
      ------+-------------+-----------+-----------------------------------------------
      1      Po1(SD)          -


      2960-7#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      
      2960-7(config)#no int po1


#### *** 2 eme methode ***     PAGP

      2960-7#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      2960-7(config)#int range fa0/1 - 2
      2960-7(config-if-range)#channel-protocol pagp
      2960-7(config-if-range)#channel-group 1 mode ?
        active     Enable LACP unconditionally
        auto       Enable PAgP only if a PAgP device is detected
        desirable  Enable PAgP unconditionally
        on         Enable Etherchannel only
        passive    Enable LACP only if a LACP device is detected

      2960-7(config-if-range)#channel-group 1 mode desirable
      Creating a port-channel interface Port-channel 1

    2960-7#sh ether summary
      Flags:  D - down        P - bundled in port-channel
              I - stand-alone s - suspended
              H - Hot-standby (LACP only)
              R - Layer3      S - Layer2
              U - in use      f - failed to allocate aggregator

              M - not in use, minimum links not met
              u - unsuitable for bundling
              w - waiting to be aggregated
              d - default port


      Number of channel-groups in use: 1
      Number of aggregators:           1

      Group  Port-channel  Protocol    Ports
      ------+-------------+-----------+-----------------------------------------------
      1      Po1(SU)         PAgP      Fa0/1(P)    Fa0/2(P)                      <<<<< protocol PAgP

#### ***3eme methode***   LACP


    2960-7#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    2960-7(config)#int range fa0/1 - 2
    2960-7(config-if-range)#channel-protocol lacp
    2960-7(config-if-range)#channel-group 1 mode active
    Creating a port-channel interface Port-channel 1


    sh ethe sum
    Flags:  D - down        P - bundled in port-channel
            I - stand-alone s - suspended
            H - Hot-standby (LACP only)
            R - Layer3      S - Layer2
            U - in use      f - failed to allocate aggregator

            M - not in use, minimum links not met
            u - unsuitable for bundling
            w - waiting to be aggregated
            d - default port


    Number of channel-groups in use: 1
    Number of aggregators:           1

    Group  Port-channel  Protocol    Ports
    ------+-------------+-----------+-----------------------------------------------
    1      Po1(SU)         LACP      Fa0/1(P)    Fa0/2(P)



#### ***Definition equilibrage de charge***


    2960-7#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    2960-7(config)#port-chann
    2960-7(config)#port-channel load-balance
    2960-7(config)#port-channel load-balance ?
      dst-ip       Dst IP Addr
      dst-mac      Dst Mac Addr
      src-dst-ip   Src XOR Dst IP Addr
      src-dst-mac  Src XOR Dst Mac Addr
      src-ip       Src IP Addr
      src-mac      Src Mac Addr
    2960-7(config)#port-channel load-balance dst-mac


##### ***verifier load-balancing***

    2960-7#sh ethe load-balance
    EtherChannel Load-Balancing Configuration:
            src-mac

    EtherChannel Load-Balancing Addresses Used Per-Protocol:
    Non-IP: Source MAC address
      IPv4: Source MAC address
      IPv6: Source MAC address


      2960-7#test etherchannel load-balance interface po1 mac 0200.0000.0001 0200.1111.1111
      Would select Fa0/2 of Po1





