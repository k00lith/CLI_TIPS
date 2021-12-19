Собираем стэк из двух свитчей - s6800,
Конфигурация стэка не отображается в - displey current, её можно посмотреть вот так - 

```
displ stack configuration
```

а настроить вот так:

```
stack enable
stack slot 0 priority 150
interface stack-port 0/1
 port interface 40GE0/0/1 enable
 port interface 40GE0/0/2 enable
```

Это для мастера, тут приоритет больше, итого конфиги ниже.
После настройки одного, потом второго, выключить оба, подсоединить провода и включить оба - стэк должен собраться

Первый свитч - master:

```
[HUAWEI-SW01]displ stack configuration 
*    : Invalid-configuration
#    : Unsaved configuration
---------------Configuration on slot 0 Begin---------------
stack enable
stack slot 0 renumber 0
stack slot 0 priority 150
stack reserved-vlan 4093
stack timer mac-address switch-delay 10

interface stack-port 0/1
 port interface 40GE0/0/1 enable
 port interface 40GE0/0/2 enable

interface stack-port 0/2
---------------Configuration on slot 0 End-----------------

-------------------------------------------------------------------------
```


Второй свитч - slave:

```
[HUAWEI-SW02]displ stack config
*    : Invalid-configuration
#    : Unsaved configuration
---------------Configuration on slot 0 Begin---------------
stack enable
stack slot 0 renumber 0
stack slot 0 priority 100
stack reserved-vlan 4093
stack timer mac-address switch-delay 10

interface stack-port 0/1

interface stack-port 0/2
 port interface 40GE0/0/1 enable
 port interface 40GE0/0/2 enable
---------------Configuration on slot 0 End-----------------
```
