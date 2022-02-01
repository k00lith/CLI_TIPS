Тест доступности привязанный к VRRP , можно еще к маршрутам привязать - ip sla типа

```
#
interface GigabitEthernet0/2/14
 description GATE
 undo shutdown
 ip address 4.4.4.4 255.255.255.0
 vrrp vrid 43 virtual-ip 4.4.4.5
 vrrp vrid 43 priority 200
 vrrp vrid 43 preempt-mode timer delay 50
 vrrp vrid 43 track nqa NQA_ADMIN NQA_ISP1_TEST reduced 110
 undo dcn
#
```

```
#
nqa test-instance NQA_ADMIN NQA_ISP1_TEST
 test-type icmp
 destination-address ipv4 8.8.8.8
 interval seconds 1
 timeout 1
 source-interface GigabitEthernet0/2/0
 frequency 5
 start now
#
```

#### frequency 

– это интервал между тестами, по умолчанию не настроен, и означает, что тест будет исполнен только один раз.
	
#### interval 

– это интервал между отправками тестовых пакетов, по умолчанию 20 миллисекунд для jitter тестов, 4 секунды для всех остальных тестов.
		
#### timeout

The timeout period refers to the waiting time after a probe is sent. If no response packet is received when the timeout period expires, the test fails. 
The timeout period is set based on the actual networking.
On an unstable network with a low transmission rate, you need to prolong the timeout period for sending probe packets to ensure that response packets can be received.


### Итого:

У нас стоит на пинге адрес - 8.8.8.8, один пинг через одну секунду, т.е. 3 проверки за 5 секунд (это можно посмотреть в nqa history там есть команда специальная), 
если ответный пакет не прилетел или прилетел 
с задержкой более 1 секунды - тест считается проваленым и происходит переключение пограничных маршрутизаторов с мастера на слэйв.
