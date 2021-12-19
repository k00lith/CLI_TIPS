Боевой конфиг для небольшого офиса с одним ISP, адрес от провайдера по dhcp, внутри две локальные сети изолированные друг от друга с помощью acl


```
c1811-battle#sh run
Building configuration...

Current configuration : 8645 bytes
!
! Last configuration change at 22:47:02 MSK Thu May 30 2019 by gsp
! NVRAM config last updated at 23:02:24 MSK Thu May 30 2019 by gsp
! NVRAM config last updated at 23:02:24 MSK Thu May 30 2019 by gsp
version 15.1
service nagle
no service pad
service tcp-keepalives-in
service tcp-keepalives-out
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname c1811-battle
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$xme1$fWU9CBs7SW72nk4gJqEtz.
!
aaa new-model
!
!
aaa authentication login default local
!
!
!
!
!
aaa session-id common
!
clock timezone MSK 3 0
crypto pki token default removal timeout 0
!
!
dot11 syslog
no ip source-route
no ip gratuitous-arps
!
!
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.0.1
ip dhcp excluded-address 192.168.0.1 192.168.0.32
ip dhcp excluded-address 192.168.0.63 192.168.0.254
!
ip dhcp pool DHCP_vl40
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 81.81.0.1 81.81.2.222
 lease 3 5 30
!
ip dhcp pool DHCP_vlan_20
 network 192.168.0.0 255.255.255.0
 default-router 192.168.0.1
 dns-server 81.81.0.1 81.81.2.222
 lease 1 5 30
!
ip dhcp pool Server1
 host 192.168.0.36 255.255.255.0
 client-identifier 0100.1599.a0f3.7b
!
ip dhcp pool Server 2
 host 192.168.0.50 255.255.255.0
 client-identifier 01bc.c342.43a2.e6
!
ip dhcp pool Phone
 host 192.168.10.10 255.255.255.0
 client-identifier 0100.0b82.3d2d.82
!
ip dhcp pool MICROT
 host 192.168.10.11 255.255.255.0
 client-identifier 01e4.8d8c.d79a.6e
!
!
!
ip cef
no ip bootp server
no ip domain lookup
ip domain name local.local
login block-for 60 attempts 3 within 30
login delay 5
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
license udi pid CISCO1811/K9 sn FTX123445
archive
 log config
  hidekeys
object-group network ENEMY
 host 1.2.3.4
 host 5.6.7.8
 host 67.67.67.67
 host 0.0.0.0
 host 255.255.255.255
 127.0.0.0 255.0.0.0
 10.0.0.0 255.0.0.0
 224.0.0.0 240.0.0.0
 192.168.0.0 255.255.0.0
 172.16.0.0 255.240.0.0
 0.0.0.0 255.0.0.0
 169.254.0.0 255.255.0.0
 192.0.2.0 255.255.255.0
 192.88.99.0 255.255.255.0
 198.18.0.0 255.254.0.0
 240.0.0.0 240.0.0.0
 192.0.0.0 255.255.255.0
 198.51.100.0 255.255.255.0
 203.0.113.0 255.255.255.0
!
object-group network MGMT
 host 192.168.0.254
 host 192.168.10.10
 host 192.168.10.11
!
object-group service MGMTPORTS
 tcp eq 54882
 tcp eq 54890
 tcp eq 54894
!
object-group service RDPPORTS
 tcp eq 58917
 tcp eq 61573
!
object-group network MGMT
 62.62.11.0 255.255.255.0
!
object-group network TRUSTED
 62.62.11.0 255.255.255.0
 62.62.12.0 255.255.255.0
 host 62.62.13.3
!
vtp mode transparent
username admin secret 5 $1$P6hT$tZgbxqGkIoX9sFap9P7pC/
secure boot-image
secure boot-config
!
!
vlan 20,40
!
ip ssh time-out 15
ip ssh logging events
ip ssh version 2
!
!
!
!
!
!
!
interface FastEthernet0
 description ISP
 mac-address 2833.3387.9355
 ip address dhcp
 ip access-group INACL in
 ip access-group OUTACL out
 no ip redirects
 no ip unreachables
 no ip proxy-arp
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
 no cdp enable
!
interface FastEthernet1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet2
 description LAN_192.168.0.0
 switchport access vlan 20
 no ip address
!
interface FastEthernet3
 no ip address
!
interface FastEthernet4
 description LAN2_192.168.10.0
 switchport access vlan 40
 no ip address
!
interface FastEthernet5
 no ip address
!
interface FastEthernet6
 no ip address
!
interface FastEthernet7
 no ip address
!
interface FastEthernet8
 no ip address
!
interface FastEthernet9
 no ip address
!
interface Vlan1
 no ip address
!
interface Vlan20
 ip address 192.168.0.1 255.255.255.0
 ip access-group FROM_20 in
 ip nat inside
 ip virtual-reassembly in
!
interface Vlan40
 ip address 192.168.10.1 255.255.255.0
 ip access-group FROM_40 in
 ip nat inside
 ip virtual-reassembly in
!
interface Async1
 no ip address
 encapsulation slip
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
ip nat inside source static tcp 192.168.0.16 3389 interface FastEthernet0 58917
ip nat inside source list FOR_NAT interface FastEthernet0 overload
ip nat inside source static tcp 192.168.0.15 3389 interface FastEthernet0 61573
ip nat inside source static tcp 192.168.10.10 80 interface FastEthernet0 54890
ip nat inside source static tcp 192.168.10.11 8291 interface FastEthernet0 54894
ip nat inside source static tcp 192.168.0.254 8081 interface FastEthernet0 54882
ip route 0.0.0.0 0.0.0.0 Null0 255
!
ip access-list standard FOR_NAT
 permit 192.168.0.0 0.0.255.255
!
ip access-list extended FROM_20
 deny   ip any 192.168.10.0 0.0.0.255
 permit ip any any
ip access-list extended FROM_40
 deny   ip any 192.168.0.0 0.0.0.255
 permit ip any any
ip access-list extended INACL
 permit udp host 46.22.33.1 any eq bootpc
 permit tcp 62.62.11.0 0.0.0.255 any eq 22
 deny   ip object-group ENEMY any
 deny   icmp any any
 deny   icmp any any redirect
 deny   tcp any any fragments
 deny   udp any any fragments
 deny   icmp any any fragments
 deny   ip any any fragments
 deny   ip any any option any-options
 permit ip host 62.66.22.198 any
 permit object-group RDPPORTS object-group TRUSTED any
 permit object-group MGMTPORTS object-group MGMT any
 evaluate Mirror
 deny   ip any any
ip access-list extended OUTACL
 permit ip any any reflect Mirror timeout 300
!
access-list 100 deny   ip host 1.1.1.1 any
access-list 100 deny   ip host 2.2.2.2 any
access-list 100 deny   ip host 3.3.3.3 any
access-list 100 deny   ip 127.0.0.0 0.255.255.255 any
access-list 100 deny   ip 10.0.0.0 0.255.255.255 any
access-list 100 deny   ip 224.0.0.0 31.255.255.255 any
access-list 100 deny   ip host 0.0.0.0 any
access-list 100 deny   ip host 255.255.255.255 any
access-list 100 deny   ip 192.168.0.0 0.0.255.255 any
access-list 100 deny   ip 172.16.0.0 0.0.255.255 any
access-list 100 deny   ip 192.0.0.0 0.255.255.255 any
access-list 100 deny   icmp any any
access-list 100 deny   icmp any any redirect
access-list 100 permit tcp any any established
access-list 100 permit udp host 194.190.168.1 any eq ntp
access-list 100 deny   udp any any eq ntp
access-list 100 permit udp host 62.62.11.16 eq snmp any
access-list 100 deny   udp any eq snmp any
access-list 100 permit tcp 62.62.11.0 0.0.0.255 any eq 58917
access-list 100 permit tcp 62.62.11.0 0.0.0.255 any eq 61573
access-list 100 permit tcp 62.62.11.0 0.0.0.255 any eq 61573
access-list 100 permit tcp 62.62.11.0 0.0.0.255 any eq 58917
access-list 100 permit tcp host 195.91.33.211 any eq 58917
access-list 100 permit tcp host 195.91.33.211 any eq 61573
access-list 100 deny   tcp any any eq 61573
access-list 100 deny   tcp any any eq 58917
access-list 100 permit udp host 22.200.0.1 eq domain any
access-list 100 permit udp host 22.200.2.222 eq domain any
access-list 100 permit tcp host 22.200.0.1 eq domain any
access-list 100 permit tcp host 22.200.2.222 eq domain any
access-list 100 permit udp host 8.8.8.8 eq domain any
access-list 100 permit udp host 8.8.4.4 eq domain any
access-list 100 permit tcp host 8.8.8.8 eq domain any
access-list 100 permit tcp host 8.8.4.4 eq domain any
access-list 100 deny   tcp any eq domain any
access-list 100 deny   udp any eq domain any
access-list 100 deny   tcp any any eq domain
access-list 100 deny   udp any any eq domain
access-list 100 permit tcp 62.62.22.0 0.0.0.255 any eq 22
access-list 100 deny   tcp any any eq 22
access-list 100 permit ip any any
access-list 105 permit ip 62.62.22.0 0.0.0.255 any
access-list 105 permit ip host 192.168.0.209 any
access-list 105 deny   ip any any
no cdp run
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
banner login ^C
#################################################################
#                                                               #
#                                                               #
#     Warning! Non Authorization Access not permitted           #
#                                                               #
#                                                               #
#################################################################


^C
!
line con 0
 exec-timeout 5 0
line 1
 modem InOut
 stopbits 1
 speed 115200
 flowcontrol hardware
line aux 0
 no exec
 transport output none
line vty 0 4
 access-class 105 in
 exec-timeout 5 0
 transport input ssh
line vty 5 193
 access-class 105 in
 exec-timeout 5 0
 transport input ssh
!
scheduler interval 500
ntp server 194.190.168.1
end
```
