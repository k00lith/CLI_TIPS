#### Насйтрока VRRP пары из двух NetEngine 8000 M1A.

Анонс по BGP в два разны провайдера один и тотже префикс.
Из этого префикса - один из ip - это шлюз для сетей внутри - и он под защитой VRRP.
Настроен пинг тест гугла, при недоступности мастер нода переключаться на слэйв (перестает пропускать трафик через себя и анонсить префикс - это 
нужно проверить, скорее всего входящий трафик может проходить через слэйв.. и анонс ?)



master
```
#
interface GigabitEthernet0/2/14
 description --> GATE <--
 undo shutdown
 ip address 89.89.89.1 255.255.255.0
 vrrp vrid 71 virtual-ip 89.89.89.100
 vrrp vrid 71 priority 200
 vrrp vrid 71 preempt-mode timer delay 50
 vrrp vrid 71 track nqa TEST_INST TEST_ISP1_TEST reduced 110
 undo dcn
#


#
bgp 65432
 peer 12.12.12.1 as-number 1234
 peer 12.12.12.1 bfd enable
 #
 ipv4-family unicast
  undo synchronization
  network 89.89.89.0 255.255.255.0
  peer 12.12.12.1 enable
#

#
nqa test-instance TEST_INST TEST_ISP1_TEST
 test-type icmp
 destination-address ipv4 8.8.8.8
 interval seconds 1
 timeout 1
 source-interface GigabitEthernet0/2/0
 frequency 5
 start now
#
```


slave
```
#
interface GigabitEthernet0/2/14
 description --> GATE <--
 undo shutdown
 ip address 89.89.89.2 255.255.255.0
 vrrp vrid 71 virtual-ip 89.89.89.100
 dcn
#

#
bgp 65432
 peer 57.57.57.1 as-number 4321
 #
 ipv4-family unicast
  undo synchronization
  network 89.89.89.0 255.255.255.0
  peer 57.57.57.1 enable
  peer 57.57.57.1 route-policy as_prepend export
#


#
route-policy as_prepend permit node 5
 apply as-path 65432 65432 65432 additive
#
```
