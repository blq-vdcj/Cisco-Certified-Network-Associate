---
attachments: [Clipboard_2021-03-23-11-01-04.png, Clipboard_2021-03-23-11-06-29.png, Clipboard_2021-03-23-11-09-02.png, Clipboard_2021-03-23-11-12-49.png, Clipboard_2021-03-23-11-31-16.png, Clipboard_2021-03-23-11-33-57.png, Clipboard_2021-03-23-11-39-22.png, Clipboard_2021-03-23-11-59-23.png, Clipboard_2021-03-23-13-40-55.png, Clipboard_2021-03-23-13-47-10.png, Clipboard_2021-03-23-14-17-14.png, Clipboard_2021-03-23-14-50-37.png, Clipboard_2021-03-23-16-02-07.png, Clipboard_2021-03-23-16-29-51.png, Clipboard_2021-03-23-16-31-46.png]
title: 20210323 Protocol Layer 2 (essentiellement au niveau des switchs)
created: '2021-03-23T09:54:35.230Z'
modified: '2021-03-24T13:09:20.286Z'
---



# 20210323 Protocol Layer 2 (essentiellement au niveau des switchs)
# ***spanning-tree Protocol ieee 802.1 D***

## ***Broadcast storm***

![](@attachment/Clipboard_2021-03-23-11-01-04.png)

tables mac inconsistentes 

![](@attachment/Clipboard_2021-03-23-11-06-29.png)

## ***bpdu***
bridge protocol data unit

![](@attachment/Clipboard_2021-03-23-11-09-02.png)

![](@attachment/Clipboard_2021-03-23-11-12-49.png)

bid = priorité ( sur 16 bits [ 0 a 65535 ] par defaut 32768 ) - mac address switch ( 48 bits )

le plus petit bridge id (bid) est la valeur selectionné

1 ) election du root du bridge > + petit BID ( bridge id ) 
2 ) determiné les root ports ( RP ) forward
![](@attachment/Clipboard_2021-03-23-11-33-57.png)
    designated Ports  (DP) forward 
    port bloqués      (BLK) pas de forward   ( 1 2 3 criteres discriminant )
        1) grand route past cost (RPC)
        2) plus grand bridge id (BID)
        3) le plus grand porte id (Port id)   
![](@attachment/Clipboard_2021-03-23-11-39-22.png)


Etats des ports 

![](@attachment/Clipboard_2021-03-23-11-59-23.png)



Differente version de STp

Stp     >>> 802.1D
Pvstp   >>> Per vlan stp  ( en isl ) , priorité encode par pallier de 4096 + la valeur du vlan       exemple vlan 10 + prior a 4096 = 4106
PVSTP + >>> per vlan stp en dot1q  , meme logique que pvstp

RSTP    >>> rapid spanning tree 802.1W     , principalement des timers qui change dans le protocol
PVRSTP  >>> isl
PVRSTP+ >>> dot1q

MSTP    >>> RSTP par groupe de vlan

--------------------------------------------------------------------------------------------------------------------------

## Exercice apres midi

![](@attachment/Clipboard_2021-03-23-13-40-55.png)
![](@attachment/Clipboard_2021-03-23-13-47-10.png)
![](@attachment/Clipboard_2021-03-23-14-17-14.png)
![](@attachment/Clipboard_2021-03-23-14-50-37.png)
![](@attachment/Clipboard_2021-03-23-16-02-07.png)
![](@attachment/Clipboard_2021-03-23-16-31-46.png)









