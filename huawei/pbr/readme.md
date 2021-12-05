### Меняем next-hop (1.1.1.1) для определенных сеток 

#### Обратите внимание, что тут два условия:
1. Подпадает под ACL
2. По таблице маршрутизации выбран исходящий интерфейс GigabitEthernet0/0/2

```
traffic classifier TO-EXTERNAL-NET operator and
 if-match acl TO-EXTERNAL-NET
 if-match outbound-interface GigabitEthernet0/0/2
#


traffic behavior TO-EXTERNAL-NET
 redirect ip-nexthop 1.1.1.1
 statistic enable
#

traffic policy TO-EXTERNAL-NET
 classifier TO-EXTERNAL-NET behavior TO-EXTERNAL-NET
#

#
traffic-policy TO-EXTERNAL-NET global inbound
#
acl name TO-EXTERNAL-NET 3998
 rule 5 permit ip source 192.168.10.0 0.0.0.255
 rule 10 permit ip source 10.110.10.0 0.0.0.255
 rule 15 permit ip source 10.120.10.0 0.0.0.255
 ```
