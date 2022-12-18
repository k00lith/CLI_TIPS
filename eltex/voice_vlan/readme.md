Настройка коммутатора:
```
vlan database
vlan 10,20
map mac c0:74:ad:1b:d3:8d 48 macs-group 1
exit
```

Порта коммутатора:
```
interface gigabitethernet 1/0/10
switchport mode general
switchport general allowed vlan add 10,20 untagged
switchport general map macs-group 1 vlan 20
switchport general pvid 10
exit
```

Тут 20 влан - это голос, 10 - дата

Это для схемы подключения ПК через телефон.
На телефоне не указываем тэг (номер влана) и на ПК тоже.

Обычно первые три байта у маков телефонов совпадают, поэтому делаем маппинг таким образом:
```
map mac c0:74:ad:1b:d3:8d 24 macs-group 1
```
т.е. по первым трем байтам
