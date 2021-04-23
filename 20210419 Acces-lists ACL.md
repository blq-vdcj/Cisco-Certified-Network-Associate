---
title: 20210419 Acces-lists ACL
created: '2021-04-19T07:14:20.644Z'
modified: '2021-04-19T10:18:11.826Z'
---

# 20210419  ***Acces-lists*** ACL

Acces control list

liste de regles definissable et ordonée , pour controller l'acces a quelque chose
 
> controle d'acces :  
                     - filtrer des paquets
                     - sécuriser les lignes vty
                     - Controler l'acces au NAT

liste de règles ***ordonnée*** 
une regle peux avoir deux actions  : Permit
                                     Deny

- Premiere regles qui match est appliqué



      Acl Standard
         -ip source

      Acl Etendues
         - ip source
         - ip destination
         - protocol ( udp / tcp etc)
         - ports source/destination    


***une acces list par direction ( Entrée/Sortie ) et par interface***


quand des regles se chauveche , les regles les plus precises sont a mettre en PREMIER


      Exemple :

      Autoriser ping de 10.0.0.0/24 vers 10.0.1.0/24
      Sauf 10.0.0.100
      1 Permit icmp 10.0.0.0 0.0.0.255 10.0.1.0 0.0.0.255
      2 deny   icmp 10.0.0.100 0.0.0.0 10.0.1.0 0.0.0.255

      NE MARCHE PAS CAR PAS DANS LE BONNE ORDRE

dernier commande tjrs presente dans les acces list  ( si aucun acl match = )
***Deny ip any any***



ACCES LISTE STANDAR plus proche de la finalité ( ex : Server web , juste avant)
ACCESS LISTE ETENDUES plus proche de la source ( par exemple machine et premier routeur)

--------------------------------------------------------------------------------------------------------------------------

Exercices Labo

    P3R2(config)#int fa0/0
    P3R2(config-if)#ip add 192.168.0.32 255.255.255.0
    P3R2(config)#int Lo0
    P3R2(config-if)#ip add 32.32.32.32 255.255.255.255


    P3R2(config)#router ospf 1
    P3R2(config-router)#router-id 32.32.32.32
    P3R2(config-router)#pass def
    P3R2(config-router)#no pass fa0/0
    P3R2(config-router)#net 192.168.0.0 0.0.0.255 a 0
    P3R2(config-router)#net 32.32.32.32 0.0.0.0 a 0


Configuration SSH : admin passwd pasderoutepasdepaquet


***NON MODIFIABLE***
    
    ACL numérotées (ancien model)    
    numero défini si standar ou etendue
    0 a 99 = STD
    de 100 a 199 = ETENDUE

***MODIFIABLE***    
    
    ACL nommées    (nouveau modele)
    Spécifie le type 


----------------------------------------------------------------------------------------------

## ***CONFIGURATION ACCESS-LIST*** 



***1ere methode***

      P3R2#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      P3R2(config)#access-list ?
        <1-99>            IP standard access list
        <100-199>         IP extended access list
        <1100-1199>       Extended 48-bit MAC address access list
        <1300-1999>       IP standard access list (expanded range)
        <200-299>         Protocol type-code access list
        <2000-2699>       IP extended access list (expanded range)
        <2700-2799>       MPLS access list
        <700-799>         48-bit MAC address access list
        compiled          Enable IP access-list compilation
        dynamic-extended  Extend the dynamic ACL absolute timer
        rate-limit        Simple rate-limit specific access list


      P3R2(config)#access-list 1 ?
        deny    Specify packets to reject
        permit  Specify packets to forward
        remark  Access list entry comment


autoriser tout sauf 192.168.0.0/24


      P3R2(config)#access-list 1 deny 192.168.0.0 0.0.0.255           ( rejetter 192.168.0.0/24)
      P3R2(config)#access-list 1 permit any


## Verifier access-list

      P3R2#show access-list 1
      Standard IP access list 1
            10 deny   192.168.0.0, wildcard bits 0.0.0.255          |   ( 4 matches ) <<<<< compteur de tentatives
            20 permit any

## configurer sur les lignes virtuels

      P3R2#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      P3R2(config)#line vty 0 4
      P3R2(config-line)#access-class 1 in

      P3R2#show access-list 1
      Standard IP access list 1
          10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
          20 permit any

      p3R22(config)# ip ssh source-interface Lo0           >>>>>> forcer a utilisé l'interface loopback pr le ssh

      P3R2#show access-list 1
      Standard IP access list 1
        10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
        20 permit any (2 matches)


***2e methode***


      P3R2(config)#ip access-list ?
        extended    Extended Access List
        log-update  Control access list log updates
        logging     Control access list logging
        resequence  Resequence Access List
        standard    Standard Access List

      P3R2(config)#ip access-list standard ACCES-VTY

      P3R2(config-std-nacl)#?
        Standard Access List configuration commands:
          <1-2147483647>  Sequence Number
          default         Set a command to its defaults
          deny            Specify packets to reject
          exit            Exit from access-list configuration mode
          no              Negate a command or set its defaults
          permit          Specify packets to forward
          remark          Access list entry comment


        P3R2(config-std-nacl)#deny 192.168.0.0 0.0.0.255
        P3R2(config-std-nacl)#permit any

***Verification***

      P3R2#sh access-list
          Standard IP access list 1
              10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
              20 permit any (2 matches)
          Standard IP access list ACCES-VTY                            >>>>> Bonne pratique MAJUSCULE
              10 deny   192.168.0.0, wildcard bits 0.0.0.255
              20 permit any

on peut edité les ACL

        P3R2(config)#ip access-list standard ACCES-VTY
        P3R2(config-std-nacl)#15 deny 23.23.23.23


        Standard IP access list 1
            10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
            20 permit any (2 matches)
        Standard IP access list ACCES-VTY
            15 deny   23.23.23.23                                       >>>>> AJOUT EN EDITION
            10 deny   192.168.0.0, wildcard bits 0.0.0.255
            20 permit any


            P3R2#conf t
            P3R2(config)#ip access-list standard ACCES-VTY
            P3R2(config-std-nacl)#no 20
            P3R2(config-std-nacl)#^Z
            P3R2#sh
            P3R2#sh access-list
            Standard IP access list 1
                10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
                20 permit any (2 matches)
            Standard IP access list ACCES-VTY
                15 deny   23.23.23.23
                10 deny   192.168.0.0, wildcard bits 0.0.0.255
            P3R2#conf t
            P3R2(config)#ip access-list standard ACCES-VTY
            P3R2(config-std-nacl)#permit any
            P3R2#conf t
            P3R2#sh access-list
            Standard IP access list 1
                10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
                20 permit any (2 matches)
            Standard IP access list ACCES-VTY
                15 deny   23.23.23.23
                10 deny   192.168.0.0, wildcard bits 0.0.0.255
                25 permit any


***RENUMEROTER LES REGLES***

            P3R2(config)#ip access-list resequence ACCESS-VTY 20 20
            P3R2#sh access-list
              Standard IP access list 1
                  10 deny   192.168.0.0, wildcard bits 0.0.0.255 (1 match)
                  20 permit any (2 matches)
              Standard IP access list ACCES-VTY
                  20 deny   23.23.23.23
                  40 deny   192.168.0.0, wildcard bits 0.0.0.255
                  60 permit any



***Changer les access list*** 

            P3R2(config)#line vty 0 4
            P3R2(config-line)#no access-class 1 in
            P3R2(config-line)#access-class ACCES-VTY in


----------------------------------------------------------------------------------------------------------------------

## ***Access-list etendu***

Exercices :

Autorisé ping depuis 192.168.0.0/24
autorisé SSH depuis tt sauf 192.168.0.0/24


      P3R2#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      P3R2(config)#access-list 100 permit icmp 192.168.0.0 0.0.0.255 any
      P3R2(config)#access-list 100 deny tcp 192.168.0.0 0.0.0.255 any eq 22
      P3R2(config)#access-list 100 permit tcp any any eq 22

***verification***

      P3R2#sh access-list 100
      Extended IP access list 100
          10 permit icmp 192.168.0.0 0.0.0.255 any
          20 deny tcp 192.168.0.0 0.0.0.255 any eq 22
          30 permit tcp any any eq 22



      P3R2(config)#int fa0/0
      P3R2(config-if)#
      P3R2(config-if)#
      P3R2(config-if)#ip access-group 100 in


            Apr 19 08:57:13.667: %OSPF-5-ADJCHG: Process 1, Nbr 1.1.1.1 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            *Apr 19 08:57:14.183: %OSPF-5-ADJCHG: Process 1, Nbr 23.23.23.23 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            *Apr 19 08:57:14.419: %OSPF-5-ADJCHG: Process 1, Nbr 22.22.22.22 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            P3R2(config-if)#
            *Apr 19 08:57:15.039: %OSPF-5-ADJCHG: Process 1, Nbr 14.14.14.14 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            P3R2(config-if)#
            *Apr 19 08:57:16.943: %OSPF-5-ADJCHG: Process 1, Nbr 24.24.24.24 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            *Apr 19 08:57:17.807: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            P3R2(config-if)#
            *Apr 19 08:57:18.559: %OSPF-5-ADJCHG: Process 1, Nbr 21.21.21.21 on FastEthernet0/0 from 2WAY to DOWN, Neighbor Down: Dead timer expired
            *Apr 19 08:57:19.359: %OSPF-5-ADJCHG: Process 1, Nbr 34.34.34.34 on FastEthernet0/0 from FULL to DOWN, Neighbor Down: Dead timer expired
            *Apr 19 08:57:19.555: %OSPF-5-ADJCHG: Process 1, Nbr 33.33.33.33 on FastEthernet0/0 from FULL to DOWN, Neighbor Down: Dead timer expired
            P3R2(config-if)#
            *Apr 19 08:57:20.491: %OSPF-5-ADJCHG: Process 1, Nbr 31.31.31.31 on FastEthernet0/0 from EXSTART to DOWN, Neighbor Down: Dead timer expired
            
***PAS de communication OSPF***

        P3R2#sh ip int fa0/0
        FastEthernet0/0 is up, line protocol is up
          Internet address is 192.168.0.32/24
          Broadcast address is 255.255.255.255
          Address determined by setup command
          MTU is 1500 bytes
          Helper address is not set
          Directed broadcast forwarding is disabled
          Multicast reserved groups joined: 224.0.0.5 224.0.0.6
          Outgoing access list is not set
          Inbound  access list is 100                               >>>>> access list appliqué

***Correction recuperation ospf***

          P3R2(config)#no access-list 100                           >>>> supprime l'acces list

***recreation access list numeroté***

          P3R2(config)#ip access-list extended FA00_IN
          P3R2(config-ext-nacl)#permit icmp 192.168.0.0 0.0.0.255 any
          P3R2(config-ext-nacl)#deny tcp 192.168.0.0 0.0.0.255 any eq 22
          P3R2(config-ext-nacl)#permit tcp any any eq 22

          P3R2(config)#int fa0/0                                    >>>> ecrase l'ancienne commande
          P3R2(config-if)#ip access-group fa00_IN in

***Resolution message ospf***


          P3R2(config)#ip access-list extended FA00_IN
          P3R2(config-ext-nacl)#permit ospf any any

***verifiier ping***

          P3R2(config-ext-nacl)#5 permit icmp any any echo-reply


***REINITIALISER COMPTEUR MATCHES***
          
          P3R2#clear access-list counters FA00_IN




