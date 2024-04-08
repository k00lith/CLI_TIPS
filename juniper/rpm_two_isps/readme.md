(https://habr.com/ru/articles/135726/)

Есть два провайдера, нужно сделать трекинг хостов в инете и по триггеру недоступности переклчаться с одного на другого

Правим зону, добавляем разрешение на пинг
```
edit security zones security-zone UNTRUST
screen untrust-screen;
host-inbound-traffic {
    system-services {
        ping;
    }
}
interfaces {
    fe-0/0/1.0;
    fe-0/0/2.0;
}
```

Правим фаервол (фильтр, который навешен на интерфейс)
```
Firewall filter
filter RE-COMMON-SERVICE-IN {
    interface-specific;
    term ACCEPT-ICMP-RPM-CHECK {
        from {
            address {
                77.88.8.8/32;
                8.8.8.8/32;
            }
            protocol icmp;
        }
        then {
            count C-A-ICMP;
            accept;
        }
    }
```


Делаем чеки хостов в инете
```
probe SLA_ISP_1_TEST {
    test CHECK_MAIN {
        probe-type icmp-ping;
        target address 77.88.8.8;
        probe-count 10;
        probe-interval 5;
        test-interval 5;
        thresholds {
            successive-loss 5;
        }
        destination-interface fe-0/0/1.0;
        next-hop 1.1.1.1;
    }
    test CHECK_MAIN_2 {
        probe-type icmp-ping;
        target address 8.8.8.8;
        probe-count 10;
        probe-interval 5;
        test-interval 5;
        thresholds {
            successive-loss 5;
        }
        destination-interface fe-0/0/1.0;
        next-hop 1.1.1.1;
    }
}
```

Статический маршрут для основного
```
static {
    route 0.0.0.0/0 {
        next-hop 194.186.250.166;
        metric 50;
    }
```


Проверяем статус
```
admin@srx> show services ip-monitoring status

Policy - ISP_1_FAILOVER (Status: FAIL)
  RPM Probes:
    Probe name             Test Name       Address          Status
    ---------------------- --------------- ---------------- ---------
    SLA_ISP_1_TEST       CHECK_MAIN      77.88.8.8        FAIL
    SLA_ISP_1_TEST       CHECK_MAIN_2    8.8.8.8          FAIL
  Route-Action:
    route-instance    route             next-hop         state
    ----------------- ----------------- ---------------- -------------
    inet.0            0.0.0.0/0         2.2.2.2     APPLIED

admin@srx>

admin@srx> show services ip-monitoring status

Policy - ISP_1_FAILOVER (Status: PASS)
  RPM Probes:
    Probe name             Test Name       Address          Status
    ---------------------- --------------- ---------------- ---------
    SLA_ISP_1_TEST       CHECK_MAIN      77.88.8.8        PASS
    SLA_ISP_1_TEST       CHECK_MAIN_2    8.8.8.8          PASS
  Route-Action:
    route-instance    route             next-hop         state
    ----------------- ----------------- ---------------- -------------
    inet.0            0.0.0.0/0         2.2.2.2     NOT-APPLIED

admin@srx>
```
