---
title: 20210623 SNMP Chapter 12 (Simple Network Management Protocol)
created: '2021-06-23T07:13:57.260Z'
modified: '2021-06-23T09:55:46.349Z'
---

# 20210623  SNMP Chapter 12 (Simple Network Management Protocol)

date des années 1980

Standard
Protocol Applicatif
UDP
Version de base 2c

```````
Agent SNMP [Routeur]                                            |              |
Agent SNMP [SWITCH]                                             | MANAGER SNMP |
Agent SNMP [Server]                                             |              |

relation entre routeur et manager   [exemple] definir une information a un momement donnée
Routeur = snmp Get  
Manager = Snmp set

relation entre switch et manager    [exemple] quand equipement veux générer des informations
Switch = Trap snmp  (bette message udp envoyé car tres ancien)
Manager = inform    (le manager envoy un ack applicatif car udp)
`````````

les informations envoyé sont sous forme
MIB = Management Info Base
Oid = 1.3.6.1.4.1.9.2.2 
````
Variable possible = - string
                    - int
                    - counter
                    - gauge
                    - ...
````

il existe un dictionnaire d'oid qui existe (un peu comme les server dns ou alias )
ex : cpuidpourcentage permet de ne pas ecire ex "1.5.4.6.8.31.5.1.6

--------------------------------------------



snmp v1         password en clear text ( community ) Read Only ou Read Write      
snmp v2c 

snmp v3         Auth , crypt , intégrité


snmp peut etre utilisé avec des suite logiciel

MRTG
PRTG







Exercices dans Gns3 21062021 SNMP


Accepter les requetes snmp

```````
R1(config)#snmp-server community READ ro
```````

verifier community

R1#show snmp community

Community name: ILMI
Community Index: cisco0
Community SecurityName: ILMI
storage-type: read-only  active


Community name: READ
Community Index: cisco1
Community SecurityName: READ
storage-type: nonvolatile        active



sur le manager snmp 

interroger 
1.3

```````

Exemple shutdown de la serial sur R1 

root@SNMP-1:/# snmpset -v2c -c CONFIG 192.168.0.1 1.3.6.1.2.1.2.2.1.7.3 i 1


R1(config)#
*Mar  1 01:48:19.079: %SYS-5-CONFIG_I: Configured from 192.168.0.2 by snmp
R1(config)#
*Mar  1 01:48:21.067: %LINK-3-UPDOWN: Interface Serial0/0, changed state to up

`````````

TRAP

R1(config)#snmp-server enable traps
% Cannot enable both sham-link state-change interface traps.
% New sham link interface trap not enabled.
R1(config)#snmp-server host 192.168.0.2 traps TRAP


