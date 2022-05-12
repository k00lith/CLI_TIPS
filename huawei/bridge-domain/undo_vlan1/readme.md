Изначально имеем такой выхлоп:

```
<HUAWEI-13>displ mac-address interf Eth-Trunk 13
Flags: * - Backup  
       # - forwarding logical interface, operations cannot be performed based 
           on the interface.
BD   : bridge-domain   Age : dynamic MAC learned time in seconds
-------------------------------------------------------------------------------
MAC Address    VLAN/BD       Learned-From        Type                Age
-------------------------------------------------------------------------------
8023-4509-2443 1/-           Eth-Trunk13         dynamic           64659
e42a-d40c-8f00 1/-           Eth-Trunk13         dynamic           64661
e000-84ae-66a9 -/1300        Eth-Trunk13.1300    dynamic           64684
-------------------------------------------------------------------------------
Total items: 3
```

Видим , что маки изучаются и там и там. Это также может влиять на stp (но это не точно)

-----------


Чтобы придавить влан 1 на стыке в транке , например , с другим коммутатором, надо делать вот так:

##### BEFORE
```
#
interface Eth-Trunk13
#	
interface 25GE1/0/13
 eth-trunk 13
 device transceiver 10GBASE-FIBER
 port mode 10G
#
interface Eth-Trunk13.1300 mode l2
 encapsulation dot1q vid 1200
 bridge-domain 1300
#
```



##### AFTER
```
#
interface Eth-Trunk13
 port link-type trunk
 undo port trunk allow-pass vlan 1
#	
interface 25GE1/0/13
 eth-trunk 13
 device transceiver 10GBASE-FIBER
 port mode 10G
#
interface Eth-Trunk13.1300 mode l2
 encapsulation dot1q vid 1200
 bridge-domain 1300
#
```
