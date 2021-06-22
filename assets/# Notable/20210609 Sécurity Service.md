---
attachments: [Clipboard_2021-06-09-09-27-02.png, Clipboard_2021-06-09-09-46-24.png]
title: 20210609 Sécurity Service
created: '2021-06-09T07:09:13.181Z'
modified: '2021-06-09T07:47:53.544Z'
---

# 20210609 Sécurity Service


Fw >>> fw
       concentrateur vpn
       nat
       ...

## **1 Concept ------ Firewall**

```````
- Fw traditionelles    (cisco = ASA appplicatif sécurity appliance et ASAv asa virtuelle)
    - Routage
    - Filtrage de traffic
    - Suivis/tracking de flux
    - Inspesction applicative (filtrage bcp plus précis) HTTP
    - Inspection Statefull (information sur la durée de communication, compter nombre de connection sur une durée de temps)


- Fw N-G ( next ou new génération )
    - Même que traditionnel mais avec plus de possibilités
    - Application Visibility (le firewall vas plus loin dans l'inspection applicative)
    - Protection Anti Malware
    - Url filtering
    - NG-IPS intrusion prevention system


```````

## **schema traditionnel firewall**

dmz zone moins sécurisé
dmz peut etre un vlan 

----
**Zone de sécurité**

```````
[Privé]---------------[FW]-------------[WAN publique]
                      |
                      |
                    [DMZ]
```````
pas de refus d'addresse mais d'echange de zone

![](@attachment/Clipboard_2021-06-09-09-27-02.png)

----------------------------

![](@attachment/Clipboard_2021-06-09-09-46-24.png)

```````
IDS = INTRUSION DETECTION SYSTEM 

wireshark ressemble a un ids
un ids n'a accun application possible sur le traffic il ecoute la duplication du traffic

IPS INTRUSION PREVENTION SYSTEM

Basé sur le principe de signature (base de connaissance des choses anormales)
Prends des mesures en donnant des instruction au autre equipements

```````
