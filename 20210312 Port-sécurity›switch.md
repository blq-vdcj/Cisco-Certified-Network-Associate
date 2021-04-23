---
attachments: [Clipboard_2021-03-12-09-23-48.png, Clipboard_2021-03-12-09-28-57.png]
title: 20210312 Port-sécurity>switch
created: '2021-03-12T08:01:44.148Z'
modified: '2021-03-12T12:56:19.644Z'
---

# 20210312 ***Port-sécurity>switch***

    > mac address
    > option: sticky
![](@attachment/Clipboard_2021-03-12-09-23-48.png)


1) limiter le nombre d'adresse MAC
2) valdier adresse MAC source

### Parametres :

    Nombre d'adresse Mac MAX (defaut=1)
    Mode de réaction (violation mode)
        >Protec
        >Restrict
        >Shutdown

![](@attachment/Clipboard_2021-03-12-09-28-57.png)

*** Par defaut le port sécurity mets le mode shutdown et une seule adresse mac***

*** Ne marche pas normalement sur port dynamic ou port trunk;sauf si port sécurity parametre sur acces et changer en trunk ou dynamique***

## Config dans le switch

      2960-7#show port-security
      Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                      (Count)       (Count)          (Count)
      ---------------------------------------------------------------------------
      ---------------------------------------------------------------------------
      Total Addresses in System (excluding one mac per port)     : 0
      Max Addresses limit in System (excluding one mac per port) : 8192

***une interface sur laquelle le port sécurity est activé apparait ici si shut ou no shut***

### ***Activer port-security***

    2960-7#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    2960-7(config)#int fa0/1
    2960-7(config-if)#switchport port-security
    Command rejected: FastEthernet0/1 is a dynamic port.      ( besoin de mettre l'interface en ACCES)
    2960-7(config-if)#switchport mode access
    2960-7(config-if)#switchport port-security


### ***verification du port sécurity***

    2960-7#show port-security
    Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                    (Count)       (Count)          (Count)
    ---------------------------------------------------------------------------
          Fa0/1              1            0                  0         Shutdown
    ---------------------------------------------------------------------------
    Total Addresses in System (excluding one mac per port)     : 0
    Max Addresses limit in System (excluding one mac per port) : 8192

### ***Configuration port-security***

    2960-7(config)#int fa0/2
    2960-7(config-if)#sw mo acc             ( switchport mode access)
    2960-7(config-if)#switchport port-security maximum ?
    <1-8192>  Maximum addresses
    2960-7(config-if)#switchport port-security maximum 2
    2960-7(config-if)#switchport port-security violation ?       >>>> changement du mode port security
      protect   Security violation protect mode
      restrict  Security violation restrict mode
      shutdown  Security violation shutdown mode
    2960-7(config-if)#switchport port-security violation restrict


    2960-7(config)#int fa0/3
    2960-7(config-if)#sw mode acc
    2960-7(config-if)#sw port-security max 1
    2960-7(config-if)#sw po violation protect     (switchport port-security etc ...)
    2960-7(config-if)#sw po mac-address ?         (la on peut encodé manuellement la MAC)
      H.H.H   48 bit mac address
      sticky  Configure dynamic secure addresses as sticky
    2960-7(config-if)#sw po mac-address sticky    (apprentissage dynamique des mac adress par le switch )

### ***Activation port-security***

    2960-7(config-if)#switchport port-security

### ***Verification switchport sécurity***

    2960-7#show port-security
    Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                    (Count)       (Count)          (Count)
    ---------------------------------------------------------------------------
          Fa0/1              1            0                  0         Shutdown
          Fa0/2              2            0                  0         Restrict
          Fa0/3              1            0                  0          Protect
    ---------------------------------------------------------------------------
    Total Addresses in System (excluding one mac per port)     : 0
    Max Addresses limit in System (excluding one mac per port) : 8192


### ***2960-7#show port-security interface <interface>*** voir le mode sticky // ou show running-config

    2960-7#sh po interface fa0/3
    % Ambiguous command:  "sh po interface fa0/3"
    2960-7#sh port-security interface fa0/3
    Port Security              : Enabled
    Port Status                : Secure-down
    Violation Mode             : Protect
    Aging Time                 : 0 mins
    Aging Type                 : Absolute
    SecureStatic Address Aging : Disabled
    Maximum MAC Addresses      : 1
    Total MAC Addresses        : 0
    Configured MAC Addresses   : 0
    Sticky MAC Addresses       : 0                       <<<<<<<<<<<<<<<<<<<<<<<< sticky mac adress
    Last Source Address:Vlan   : 0000.0000.0000:0
    Security Violation Count   : 0

---------------------------------------------------------------------------------------------------------------------------------------------------------







