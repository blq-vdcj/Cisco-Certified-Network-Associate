# 20210317 explication supplémentaire 

##### ***P3R2#:show history***

    historique des commandes dans l'ordre ou on les a tappés si fermeture de la session plus d'historique
    
##### ***P3R2(config): DO show history***

    historique du mode de configuration 

##### ***P3R2#:terminal history size <nombres de lignes>***   ( pas sauvegardé dans la config)

##### ***augmenter le nombre de ligne de l'historique***

    P3R2(config):line con 0
    P3R2(config-line):history size <nombres de lignes>

##### ***traceroute plus rapide ***

    P3R2#: traceroute 10.11.0.1 numeric            ( bypass des resolutions dns )

##### ***desactiver la resolution de nom***
    
    P3R2#:conf t
    P3R2#:no ip domain-lookup ( desactive la resolution de nom par le routeur )

##### ***ALIAS***

    P3R2#: show aliases
    Exec mode aliases:
    h                     help
    lo                    logout
    p                     ping
    r                     resume
    s                     show
    u                     undebug
    un                    undebug
    w                     where


##### ***configuration client dhcp***

    P3R2#conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    P3R2(config)#int fa0/0
    P3R2(config-if)#ip address dhcp
    P3R2(config-if)#no shut

    P3R2#show ip route
    S       172.30.1.10/32 [254/0] via 172.30.13.1, FastEthernet0/0
    C       172.30.13.0/24 is directly connected, FastEthernet0/0
    S*   0.0.0.0/0 [254/0] via 172.30.13.1              (mise automatiquement par dhcp car fournis passerelle par defaut)


##### ***information fournis par dhcp***

    P3R2#show dhcp lease
    Temp IP addr: 172.30.13.34  for peer on Interface: FastEthernet0/0
    Temp  sub net mask: 255.255.255.0
      DHCP Lease server: 172.30.1.10, state: 5 Bound
      DHCP transaction id: 2FA
      Lease: 691200 secs,  Renewal: 345600 secs,  Rebind: 604800 secs
    Temp default-gateway addr: 172.30.13.1
      Next timer fires after: 3d23h
      Retry count: 0   Client-ID: cisco-001e.7a4c.a88e-Fa0/0
      Client-ID hex dump: 636973636F2D303031652E376134632E
                          613838652D4661302F30
      Hostname: P3R2








#####  TEST CABLE DIAGNOSTICS TDR      (fonctionne tres bien sur des cable de longues distances )

      2960-7#test cable-diagnostics tdr int fa0/1
      Link state may be affected during TDR test
      TDR test started on interface Fa0/1
      A TDR test can take a few seconds to run on an interface
      Use 'show cable-diagnostics tdr' to read the TDR results.
      2960-7#show cable-diagnostics tdr
      % Incomplete command.

      2960-7#show cable-diagnostics tdr int fa0/1
      TDR test last run on: March 01 01:13:46

      Interface Speed Local pair Pair length        Remote pair Pair status
      --------- ----- ---------- ------------------ ----------- --------------------
      Fa0/1     100M  Pair A     0    +/- 15 meters Pair B      Normal
                      Pair B     0    +/- 15 meters Pair A      Normal
                      Pair C     N/A                Pair D      Not Supported
                      Pair D     N/A                Pair C      Not Supported

