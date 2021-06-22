---
title: 20210519 dhcp snooping // arp inspection
created: '2021-05-19T07:12:53.182Z'
modified: '2021-05-19T09:56:58.286Z'
---

# 20210519 dhcp snooping // arp inspection

exercices dans gns3 2021-05-19 dhcp snooping



### ***dhcp snooping***

au niveau des switchs

    port : --> trust
          --> pas trust


construction base de donné dhcp snooping binding

| mac | ip | port |
| --- | -- | ---- |

### ***icmp redirect***

    Rogue_DHCP#conf t
    Rogue_DHCP(config)#int fa0/0
    Rogue_DHCP(config-if)#no ip redirects


### ***Configuration du dhcp snooping***

    d'abord options avant d'activations

***Options***

    MLS1(config)#int g0/2
    MLS1(config-if)#ip dhcp snooping trust    (trust pr tt les vlans et trunk etc)indiquez dans le switch tt les interface menant au dhcp

***Activation***

    MLS1(config)#ip dhcp snooping vlan 1
    MLS1(config)#ip dhcp snooping ( activation commande vide)

### ***Vérification***

***MLS1#sh ip dhcp snooping***

    Switch DHCP snooping is disabled
    Switch DHCP gleaning is disabled
    DHCP snooping is configured on following VLANs:
    1
    DHCP snooping is operational on following VLANs:
    1
    DHCP snooping is configured on the following L3 Interfaces:

    Insertion of option 82 is enabled                >>>> options necessaire relay dhcp
      circuit-id default format: vlan-mod-port^      
      remote-id: 0cd6.28a7.8000 (MAC)
    Option 82 on untrusted port is not allowed
    Verification of hwaddr field is enabled
    Verification of giaddr field is enabled
    DHCP snooping trust/rate is configured on the following Interfaces:

    Interface                  Trusted    Allow option    Rate limit (pps)
    -----------------------    -------    ------------    ----------------
    GigabitEthernet0/2         yes        yes             unlimited
      Custom circuit-ids:

***MLS1#sh ip dhcp snooping statistic***

      Packets Forwarded                                     = 6
      Packets Dropped                                       = 0
      Packets Dropped From untrusted ports                  = 0


***MLS1#sh ip dhcp snooping binding***

    MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
    ------------------  ---------------  ----------  -------------  ----  --------------------
    00:50:79:66:68:00   192.168.0.11     86255       dhcp-snooping   1     GigabitEthernet0/0
    Total number of bindings: 1

### ***desactiver l'option 82 sur un layer 2 ( sans RELaY DHCP )***

    MLS1(config)#no ip dhcp snooping information option

--------------------------------

# ***Dynamic Arp Inspection***

    Configuration (vlan par vlan)

    MLS1#conf t
    MLS1(config)#ip arp inspection vlan 1


***verficiation arp inspection***

      MLS1#sh ip arp inspection

      Source Mac Validation      : Disabled
      Destination Mac Validation : Disabled
      IP Address Validation      : Disabled

      Vlan     Configuration    Operation   ACL Match          Static ACL
      ----     -------------    ---------   ---------          ----------
          1     Enabled          Active

      Vlan     ACL Logging      DHCP Logging      Probe Logging
      ----     -----------      ------------      -------------
          1     Deny             Deny              Off

      Vlan      Forwarded        Dropped     DHCP Drops      ACL Drops
      ----      ---------        -------     ----------      ---------
          1              0              0              0              0

      Vlan   DHCP Permits    ACL Permits  Probe Permits   Source MAC Failures
      ----   ------------    -----------  -------------   -------------------
          1              0              0              0                     0

      Vlan   Dest MAC Failures   IP Validation Failures   Invalid Protocol Data
      ----   -----------------   ----------------------   ---------------------

      Vlan   Dest MAC Failures   IP Validation Failures   Invalid Protocol Data
      ----   -----------------   ----------------------   ---------------------
          1                   0                        0                       0


ip arp inspection validate ( mac source pr valider identié de kelk1)

      MLS1(config)#ip arp inspection validate ?
        dst-mac  Validate destination MAC address
        ip       Validate IP addresses
        src-mac  Validate source MAC address

***inspection validate***

      MLS1(config)#ip arp inspection validate src-mac ( verification des mac sources)



***sh ip arp inspection interfaces***

      MLS1#sh ip arp inspection interfaces

      Interface        Trust State     Rate (pps)    Burst Interval
      ---------------  -----------     ----------    --------------
      Gi0/0            Untrusted               15                 1
      Gi0/1            Untrusted               15                 1
      Gi0/2            Untrusted               15                 1
      Gi0/3            Untrusted               15                 1
      Gi1/0            Untrusted               15                 1
      Gi1/1            Untrusted               15                 1
      Gi1/2            Untrusted               15                 1
      Gi1/3            Untrusted               15                 1
      Gi2/0            Untrusted               15                 1
      Gi2/1            Untrusted               15                 1
      Gi2/2            Untrusted               15                 1
      Gi2/3            Untrusted               15                 1
      Gi3/0            Untrusted               15                 1
      Gi3/1            Untrusted               15                 1
      Gi3/2            Untrusted               15                 1
      Gi3/3            Untrusted               15                 1


***cfg de l'interface du server dhcp en trust***

      MLS1#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      MLS1(config)#int g0/2
      MLS1(config-if)#ip arp inspection trust               >>>> inspect interfaces offf car trust



***rentrer une information manuellement dans la table arp inspetions***


    MLS1(config)#arp access-list STATIC_IP
    MLS1(config-arp-nacl)#permit ip host 192.168.0.47 mac host 1111.2222.3333

    MLS1(config)#ip arp inspection filter STATIC_IP vlan 1 static

