---
title: 20210609 Exercices Gns3
created: '2021-06-09T09:16:49.545Z'
modified: '2021-06-09T09:37:31.033Z'
---

# 20210609 Exercices Gns3

creation loopback pr le tunnel
```````

R1(config)#int l0
R1(config-if)#
*Mar  1 00:15:31.655: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up
R1(config-if)#ip add 1.1.1.1 255.255.255.255

```````


Eigrp R1 

`````````

R1(config-if)#routeur eigrp 1
                  ^
% Invalid input detected at '^' marker.

R1(config-if)#router eigrp 1
R1(config-router)#no auto
R1(config-router)#pass deff
                          ^
% Invalid input detected at '^' marker.

R1(config-router)#no pass s0/0
R1(config-router)#no pass s0/1
R1(config-router)#net 1.1.1.1 0.0.0.0
R1(config-router)#net 192.168.0.0
````````

Config R2
``````
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.

R2(config)#router eigrp 1
R2(config-router)#pass def
R2(config-router)# no pass s0/0
R2(config-router)# no pass s0/1
R2(config-router)#net 192.168.0.0   ( normalement on mets le wildcar pour )


etc etc etc sur les routeur plus loopback sur R4
`````````






# ***Interface tunnel*** 
`````````
R1(config)#int
*Mar  1 00:22:48.871: %SYS-5-CONFIG_I: Configured from console by console
R1(config)#int tunnel 0
R1(config-if)#tunnel mode                  ( mode par defaut = gre ip)
R1(config-if)#tunnel mode gre ip

R1(config-if)#tunnel source l0
R1(config-if)#tunnel destination 4.4.4.4
*Mar  1 00:27:40.387: %LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up    ( up si interface source est up et qu'il a une route vers la destionation )

R1(config-if)#ip add 172.16.0.5 255.255.255.252
`````````



Cfg R4

`````````

R4(config)#int tun0
R4(config-if)#tunnel source l
*Mar  1 00:28:18.959: %LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to down
R4(config-if)#tunnel source l0
R4(config-if)#tunnel destination 1.1.1.1
R4(config-if)#ip add 172.16.0.6 255.255.255.252
`````````




Creation ospf 


`````````
R5(config)#router ospf 1
R5(config-router)#router
R5(config-router)#router-id 5.5.5.5
R5(config-router)#pass def
R5(config-router)#no pass fa0/0
R5(config-router)#int l0
R5(config-if)#ip ospf 1 area 0
R5(config-if)#l1
               ^
% Invalid input detected at '^' marker.

R5(config-if)#int l1
R5(config-if)#ip ospf 1 area 0
R5(config-if)#int fa0/0
R5(config-if)#ip ospf 1 area 0



