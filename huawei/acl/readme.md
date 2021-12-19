
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

