
ACL:

```
#
acl name NETS 3999
 rule 5 permit ip source 123.123.123.0 0.0.15.255
 rule 10 permit ip source 172.16.3.0 0.0.0.255
 rule 15 permit ip source 172.17.0.0 0.0.7.255
 rule 20 permit ip source 172.18.0.0 0.0.255.255
 rule 25 permit udp source-port eq bootpc destination-port eq bootps
 rule 30 deny ip
#
```

Вешаем на интерфейс

```
#
interface GigabitEthernet0/0/1
 port link-type access
 port default vlan 20
 loopback-detect enable
 loopback-detect action block
 traffic-filter inbound acl name NETS
 undo ntdp enable
 undo ndp enable
 port discard tagged-packet
 multicast-suppression block outbound
 unicast-suppression block outbound
#
```

Еще пример:

```
acl name DEFEND-ROUTER 3995
 rule 5 permit ip source 8.8.8.0 0.0.0.15
 rule 10 permit ip source 55.55.55.55 0
 rule 15 deny ip
acl name ATTACK_ICMP 3996
 rule 5 deny udp destination 4.4.4.4 0 destination-port range snmp snmptrap
 rule 10 permit udp destination-port range snmp snmptrap
```

На интерфейс:

```
#
interface GigabitEthernet0/0/1
 description ---> SOME_DEV <---
 port link-type trunk
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 712
 traffic-mirror inbound acl name NetFlow to observe-port 1
 traffic-filter inbound acl name ATTACK
 undo ntdp enable
 undo ndp enable
 port-mirroring to observe-port 2 outbound
#
```
