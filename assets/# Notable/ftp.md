---
attachments: [Clipboard_2021-06-14-13-52-32.png, Clipboard_2021-06-14-13-59-19.png]
title: 20210614 Syslog - Tftp / ftp
created: '2021-06-14T11:44:06.079Z'
modified: '2021-06-14T12:56:08.936Z'
---

# 20210614  Syslog - Tftp / ftp


```````
Syslog = PORT 514 
         Udp ( en général ) mais aussi Tcp
`````````
par defaut sur la console
par defaut message affiché et perdu , si on veux les garder , il faut les buffers
![](@attachment/Clipboard_2021-06-14-13-52-32.png)
timestamps est optionnel
![](@attachment/Clipboard_2021-06-14-13-59-19.png)
level =  plus le level est bas plus il est important

```````
        0   Emergency                             = probleme sévère
        1   Alert                                 = probleme sévère
----------------------------------------------
        2   Critical                              = 
        3   Error                                 = problem potentiels
        4   Warning                               =
----------------------------------------------
        5   Notifications                         =   
        6   Informations                          = 
        7   Debug ( tout les messages affichés )
`````````


possibilité d'afficher ses messages , par session ou de maniere globale sur la config


## ***Execice topo gns***


  
```````
R1(config)#int fa0/0
R1(config-if)#ip add 192.168.0.1 255.255.255.0
R1(config-if)#no sh
`````````

### ***TFTP***
`````````
R1#copy startup-config tftp
Address or name of remote host []? 192.168.0.10
Destination filename [r1-confg]? R1_startup_cfg
!!
1393 bytes copied in 0.044 secs (31659 bytes/sec)
```````


## ***FTP***


```````
R1#copy startup-config ftp
Address or name of remote host []? 192.168.0.10
Destination filename [r1-confg]? R1-conf
Writing R1-conf
%Error opening ftp://192.168.0.10/R1-conf (Incorrect Login/Password)
R1#copy start
R1#copy startup-config ftp://cisco:cisco@192.168.0.10:/R1.cfg
Address or name of remote host [192.168.0.10]?
Destination filename [R1.cfg]?
Writing R1.cfg !
1393 bytes copied in 0.152 secs (9164 bytes/sec)
`````````




## ***recuperer la cfg a distance sur le router***


R1#copy ftp://cisco:cisco@192.168.0.10:/R1.cfg Sdcard

R1(config)#: ip ftp password cisco
R1(config)#: ip ftp username cisco 

= ftp://192.168.0.10:/R1.cfg Sdcard au lieu de ftp://cisco:cisco@192.168.0.10:/R1.cfg Sdcard





------------------------------------------------------------------------------------



## ***SYSLOG***

```````
`R1(config)#loggin
R1(config)#logging ?
  Hostname or A.B.C.D  IP address of the logging host
  buffered             Set buffered logging parameters
  buginf               Enable buginf logging for debugging
  cns-events           Set CNS Event logging level
  console              Set console logging parameters
  count                Count every log message and timestamp last occurrence
  discriminator        Create or modify a message discriminator
  dmvpn                DMVPN Configuration
  esm                  Set ESM filter restrictions
  exception            Limit size of exception flush output
  facility             Facility parameter for syslog messages
  filter               Specify logging filter
  history              Configure syslog history table
  host                 Set syslog server IP address and parameters
  message-counter      Configure log message to include certain counter value
  monitor              Set terminal line (monitor) logging parameters
  on                   Enable logging to all enabled destinations
  origin-id            Add origin ID to syslog messages
  persistent           Set persistent logging parameters
  queue-limit          Set logger message queue size
  rate-limit           Set messages per second limit
  reload               Set reload logging level
  server-arp           Enable sending ARP requests for syslog servers when
                       first configured
  source-interface     Specify interface for source address in logging
                       transactions
  trap                 Set syslog server logging level
  userinfo             Enable logging of user info on privileged mode enabling
```````


## ***Définir le seul d'affichage des messages***
```````
R1(config)#logging console 5                   ====================== Affiche les messages de 5 a 0
```````


## ***Buffered les messages***
`````
R1(config)#logging buffered 4096 5

VERIFICATION

R1#show logging
Syslog logging: enabled (11 messages dropped, 7 messages rate-limited,
                0 flushes, 0 overruns, xml disabled, filtering disabled)

No Active Message Discriminator.



No Inactive Message Discriminator.


    Console logging: disabled
    Monitor logging: level debugging, 1 messages logged, xml disabled,
                     filtering disabled
    Buffer logging:  level notifications, 1 messages logged, xml disabled,
                     filtering disabled
    Logging Exception size (4096 bytes)
    Count and timestamp logging messages: disabled
    Persistent logging: disabled

No active filter modules.

ESM: 0 messages dropped

    Trap logging: level informational, 43 message lines logged

Log Buffer (4096 bytes):

***Mar  1 00:37:51.771: %SYS-5-CONFIG_I: Configured from console by vty0 (192.168.0.1)     AJOUTE LES LOG ICI

```


RETIRER LES TIMESTAMPS
````
R1(config)#no service timestamps
%SYS-5-CONFIG_I: Configured from console by vty0 (192.168.0.1) >>>>>>>>>>>>>>> plus de timestamp
```



