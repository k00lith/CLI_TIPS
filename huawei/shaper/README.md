S5300 V200R001C00SPC300

Вешаем шейпер на интерфейс
-----------------------------------

```
traffic behavior 4Mbps
 car cir 4096 pir 4096 cbs 512000 pbs 512000 green pass yellow pass red discard
 statistic enable
traffic behavior 10Mbps
 car cir 10240 pir 10240 cbs 1280000 pbs 1280000 green pass yellow pass red discard
 statistic enable
traffic behavior 15Mbps
 car cir 15360 pir 15360 cbs 1920000 pbs 1920000 green pass yellow pass red discard
 statistic enable
traffic behavior 25Mbps
 car cir 25600 pir 25600 cbs 3200000 pbs 3200000 green pass yellow pass red discard
 statistic enable
traffic behavior 30Mbps
 car cir 30720 pir 30720 cbs 3840000 pbs 3840000 green pass yellow pass red discard
 statistic enable
traffic behavior 40Mbps
 car cir 40960 pir 40960 cbs 5120000 pbs 5120000 green pass yellow pass red discard
 statistic enable
traffic behavior 50Mbps
 car cir 51200 pir 51200 cbs 6400000 pbs 6400000 green pass yellow pass red discard
 statistic enable
traffic behavior 100Mbps
 car cir 102400 pir 102400 cbs 12800000 pbs 12800000 green pass yellow pass red discard
 statistic enable 
```

Полиси

```
traffic policy 10Mbps
 classifier ALL behavior 10Mbps
traffic policy 15Mbps
 classifier ALL behavior 15Mbps
traffic policy 25Mbps
 classifier ALL behavior 25Mbps
traffic policy 30Mbps
 classifier ALL behavior 30Mbps
traffic policy 40Mbps
 classifier ALL behavior 40Mbps
traffic policy 50Mbps
 classifier ALL behavior 50Mbps
traffic policy 100Mbps
 classifier ALL behavior 100Mbps
```

ACL (тут можно указать конкретные сетки)
```
acl name ALL 2997
 rule 5 permit
 

acl name TO-NET 3997
 rule 5 permit ip destination 44.3.23.0 0.0.0.15
acl name FROM-NET 3992
 rule 5 permit ip source 44.3.23.0 0.0.0.15
```

Classifier
```
traffic classifier ALL operator and
 if-match acl ALL
```
или мапим к влану
```
traffic classifier class-vlan333 operator and
 if-match vlan-id 333
```

Непосркедственно вешаем на интерфйес в обе стороны
```
vlan 44
 description ---> CLIENT <---
 multicast drop-unknown
 mirroring to observe-port 1 inbound
 traffic-policy 50Mbps inbound
 traffic-policy 50Mbps outbound
```
 
