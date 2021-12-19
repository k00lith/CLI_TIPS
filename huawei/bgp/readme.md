Просто bgp конфиг на три провайдера с префикс листами, local preference и prepend

Первый сосед - предпочтительней всех на выход из AS - так как самая высокая lp

Второй сосед - ничего не анонсируем и не принимаем, lp  - самая низкая (это как кандидат на отключение услуги)

Третий сосед - резервный, т.к. lp ниже чем у первого + добавлены препенды - это влияет на входящий трафик

```
#
bgp 65432
 peer 11.11.11.11 as-number 65001
 peer 11.11.11.11 description "--> ISP-1 <--"
 peer 11.11.11.11 connect-interface Vlanif777
 peer 22.22.22.22 as-number 65002
 peer 22.22.22.22 description "--> ISP-2 <--"
 peer 22.22.22.22 timer keepalive 20 hold 60
 peer 22.22.22.22 connect-interface Vlanif888
 peer 33.33.33.33 as-number 65003
 peer 33.33.33.33 description "--> ISP-3 <--"
 peer 33.33.33.33 timer keepalive 20 hold 100
 peer 33.33.33.33 connect-interface Vlanif999
 #
 ipv4-family unicast
  undo synchronization
  network 1.1.1.0 255.255.248.0
  network 2.2.2.0 255.255.252.0
  network 3.3.3.0 255.255.254.0
  peer 11.11.11.11 enable
  peer 11.11.11.11 ip-prefix ISP-1-IN import
  peer 11.11.11.11 ip-prefix ISP-1-OUT export
  peer 11.11.11.11 preferred-value 2500
  peer 22.22.22.22 enable
  peer 22.22.22.22 ip-prefix DENY_ANY import
  peer 22.22.22.22 ip-prefix DENY_ANY export
  peer 22.22.22.22 route-policy PREPEND_AS export
  peer 22.22.22.22 preferred-value 1500
  peer 33.33.33.33 enable
  peer 33.33.33.33 ip-prefix ISP-3-IN import
  peer 33.33.33.33 ip-prefix ISP-3-OUT export
  peer 33.33.33.33 route-policy PREPEND_AS export
  peer 33.33.33.33 preferred-value 2000
#


#
ip route-static 1.1.1.0 255.255.240.0 55.55.55.1
ip route-static 2.2.2.0 255.255.248.0 55.55.55.1
ip route-static 3.3.3.0 255.255.252.0 55.55.55.1
#

#
route-policy PREPEND_AS permit node 5
 apply as-path 65432 65432 65432 65432 65432 65432 additive
#
ip ip-prefix ISP-1-IN index 10 permit 0.0.0.0 0
ip ip-prefix ISP-1-OUT index 11 permit 1.1.1.0 21
ip ip-prefix ISP-1-OUT index 12 permit 2.2.2.0 22
ip ip-prefix ISP-1-OUT index 13 permit 3.3.3.0 23
ip ip-prefix ISP-1-OUT index 15 deny 0.0.0.0 0 less-equal 32
ip ip-prefix ISP-2-IN index 10 permit 0.0.0.0 0
ip ip-prefix ISP-3-IN index 10 permit 0.0.0.0 0
ip ip-prefix ISP-3-OUT index 11 permit 1.1.1.0 21
ip ip-prefix ISP-3-OUT index 12 permit 2.2.2.0 22
ip ip-prefix ISP-3-OUT index 13 permit 3.3.3.0 23
ip ip-prefix ISP-3-OUT index 15 deny 0.0.0.0 0 less-equal 32
ip ip-prefix DENY_ANY index 10 deny 0.0.0.0 0 less-equal 32
#
```
