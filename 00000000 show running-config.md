---
title: 00000000 show running-config
created: '2021-03-05T15:01:35.203Z'
modified: '2021-03-09T10:07:09.191Z'
---

# 00000000 show running-config



Building configuration...

Current configuration : 1764 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname P3R2
!
boot-start-marker
boot-end-marker
!
logging message-counter syslog
enable secret 5 $1$W3QY$TlwsFlBqey7dJAT3Ce3jz.
!
no aaa new-model
dot11 syslog
ip source-route
!
!
!
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
voice-card 0
!
!
!
!
!
archive
 log config
  hidekeys
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 10.32.0.1 255.255.255.0
!
interface Loopback1
 ip address 10.32.1.1 255.255.255.0
!
interface FastEthernet0/0
 ip address 172.16.0.2 255.255.255.248
 duplex auto
 speed auto
!
interface FastEthernet0/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface Serial0/1/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/1/1
 no ip address
 shutdown
 clock rate 2000000
!
ip forward-protocol nd
ip route 10.11.0.0 255.255.254.0 172.16.0.1
ip route 10.13.0.0 255.255.254.0 172.16.0.1
ip route 10.14.0.0 255.255.254.0 172.16.0.1
ip route 10.21.0.0 255.255.254.0 172.16.0.4
ip route 10.31.0.0 255.255.255.0 172.16.0.3
ip route 10.31.1.0 255.255.255.0 172.16.0.3
ip route 10.33.0.0 255.255.254.0 172.16.0.1
ip route 10.34.0.0 255.255.254.0 172.16.0.1
ip route 172.16.0.8 255.255.255.248 172.16.0.10
ip route 172.16.0.8 255.255.255.248 172.16.0.1
ip route 172.16.0.16 255.255.255.248 172.16.0.1
ip route 172.16.0.24 255.255.255.248 172.16.0.1
no ip http server
no ip http secure-server
!
!
!
!
!
!
!
!
!
!
control-plane
!
!
!
ccm-manager fax protocol cisco
!
mgcp fax t38 ecm
!
!
!
!
!
!
line con 0
 exec-timeout 0 0
 password 7 06070B2C4540
 logging synchronous
 login
line aux 0
line vty 0 4
 login

