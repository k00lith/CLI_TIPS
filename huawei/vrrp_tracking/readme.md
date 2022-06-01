
```
[~ROUTER-001-GigabitEthernet0/2/11.2222]displ this
#
interface GigabitEthernet0/2/11.2222
 ip binding vpn-instance VRF_1
 ip address 172.17.201.33 255.255.255.224
 encapsulation dot1q-termination
 dot1q termination vid 2222
 arp broadcast enable
 vrrp vrid 61 virtual-ip 172.17.201.35
#
return
```
```
[~ROUTER-001-GigabitEthernet0/2/11.2222]displ vrrp 61
GigabitEthernet0/2/11.2222 | Virtual Router 61
State          : Backup
Virtual IP     : 172.17.201.35
Master IP      : 172.17.201.34
Local IP       : 172.17.201.33
PriorityRun    : 100
PriorityConfig : 100
MasterPriority : 150
Preempt        : YES   Delay Time : 0s   
Hold Multiplier: 3
TimerRun       : 1s
TimerConfig    : 1s
Auth Type      : NONE
Virtual MAC    : 0000-5e00-013d
Check TTL      : YES
Config Type    : normal-vrrp
Backup-forward : disabled
Create Time       : 2022-05-31 17:52:23+03:00
Last Change Time  : 2022-05-31 17:52:23+03:00
```

===================================


```
[~ROUTER-002-GigabitEthernet0/2/9.2222]displ this
#
interface GigabitEthernet0/2/9.2222
 ip binding vpn-instance VRF_1
 ip address 172.17.201.34 255.255.255.224
 encapsulation dot1q-termination
 dot1q termination vid 2222
 arp broadcast enable
 vrrp vrid 61 virtual-ip 172.17.201.35
 vrrp vrid 61 priority 150
 vrrp vrid 61 preempt-mode timer delay 10
 vrrp vrid 61 track ip route 0.0.0.0 0.0.0.0 reduced 100
#
```
```
[~ROUTER-002-GigabitEthernet0/2/9.2222]displ vrrp 61
GigabitEthernet0/2/9.2222 | Virtual Router 61
State          : Master
Virtual IP     : 172.17.201.35
Master IP      : 172.17.201.34
Local IP       : 172.17.201.34
PriorityRun    : 150
PriorityConfig : 150
MasterPriority : 150
Preempt        : YES   Delay Time : 10s   
Hold Multiplier: 3
TimerRun       : 1s
TimerConfig    : 1s
Auth Type      : NONE
Virtual MAC    : 0000-5e00-013d
Check TTL      : YES
Config Type    : normal-vrrp
Backup-forward : disabled
Track IP Route    : 0.0.0.0/0  Priority Reduced : 100
IP Route State    : Reachable 
Create Time       : 2022-05-31 17:52:26+03:00
Last Change Time  : 2022-05-31 17:52:29+03:00
```



```
[~ROUTER-002-GigabitEthernet0/2/9.2222]displ ip routing-table vpn-instance VRF_1 
Route Flags: R - relay, D - download to fib, T - to vpn-instance, B - black hole route
------------------------------------------------------------------------------
Routing Table : VRF_1
         Destinations : 19       Routes : 19        

Destination/Mask    Proto   Pre  Cost        Flags NextHop         Interface

        0.0.0.0/0   EBGP    255  0             RD  8.8.119.97   GigabitEthernet0/2/9.61
```		
		
		


Группа VRRP, связанная с маршрутом, не может содержать владельца IP-адреса.

Мастер и резерв в группе VRRP должны работать в режиме приоритета. Рекомендуется, чтобы задержка вытеснения была равна 0 на резервной копии и отличной от 0 на главной.


https://support.huawei.com/enterprise/en/doc/EDOC1100064351/a4ed4596/vrrp-vrid-track-ip-route		

