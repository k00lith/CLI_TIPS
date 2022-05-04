
Задача:
Написать роут мапу , чтобы анонсить соседу только определенные префиксы.
Зачем это нужно:
Например, если два стыка и по одному принимаем дефолт, чтобы этот дефолт далее не анонсить во второй стык и тем самым не стать транзитом для других провайдеров

  

Пишем префикс лист:
```
ip ip-prefix OUR_NETS_OUT index 10 permit 172.17.1.0 29
ip ip-prefix OUR_NETS_OUT index 20 permit 172.17.2.0 29
ip ip-prefix OUR_NETS_OUT index 30 permit 172.17.3.0 29
ip ip-prefix OUR_NETS_OUT index 40 permit 172.17.4.0 29
ip ip-prefix OUR_NETS_OUT index 50 permit 172.17.5.0 29
```
Пишем роут мапу:
```
route-policy RP_OUR_NETS_OUT permit node 1
 if-match ip-prefix OUR_NETS_OUT
```

Навешиваем роут мапу к соседу
```
bgp 65200
 ipv4-family vpn-instance VRF_55
 peer 98.183.176.1 route-policy RP_OUR_NETS_OUT export
```

Посмотреть что мы анонсим соседу (bgp у нас живет в отдельной vrf):
```
displ bgp vpnv4 vpn-instance VRF_55 routing-table peer 98.183.176.1 advertised-routes
```


Если нужно разрешить определенные сети на анонс и еще сделать к ним препенд, то:

```
route-policy PFRETSP_OUR_NETS_OUT permit node 1
 if-match ip-prefix PFRETSP_OUR_NETS_OUT
 apply as-path 65212 65212 additive
```
