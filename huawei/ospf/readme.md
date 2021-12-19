
OSPF

```
#
ospf 3319 router-id 86.5.12.1
 import-route static
 import-route bgp
 silent-interface all
 undo silent-interface Vlanif700
 peer 86.5.12.2
 area 1.1.1.1
  network 35.35.35.0 0.0.15.255
#
```

Также не забываем про доп настройки на интерфейсах, например:
```
#
interface Vlanif777
 description ---> TO NEIGHBOR <---
 ip address 86.5.12.1 255.255.255.252
 ospf network-type nbma
 ospf smart-discover
#
```
