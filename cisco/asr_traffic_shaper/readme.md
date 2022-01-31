
Пример ограничения скорости на ASR1001-X


```
ip access-list extended ACL-TRAF-CLI
 permit ip any host 103.23.34.5

class-map match-all CL-TRAF-CLI
 match access-group name ACL-TRAF-CLI

policy-map PM-TRAF-CLI-INET
 class CL-TRAF-CLI
  police cir 100000000 pir 150000000 conform-action transmit  exceed-action drop  violate-action drop 
  
interface Port-channel1.1515
 service-policy input PM-TRAF-CLI-INET
``` 
