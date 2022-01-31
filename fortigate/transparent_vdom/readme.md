Чтобы создать транспарент вдом, через cli, перезагружать firewall не нужно

Firmware	v6.4.7 build1911 (GA)

```
OST-CU-IPS01 # 
OST-CU-IPS01 # config vdom 
OST-CU-IPS01 (vdom) # edit VD01-CLIENT01
current vf=VD01-CLIENT01:5

OST-CU-IPS01 (VS01-MCFR) # 
OST-CU-IPS01 (VS01-MCFR) # 
OST-CU-IPS01 (VS01-MCFR) # 
OST-CU-IPS01 (VS01-MCFR) # config system settings 
OST-CU-IPS01 (settings) # set opmode transparent 
OST-CU-IPS01 (settings) # set manageip 10.1.1.9 255.255.255.0
OST-CU-IPS01 (settings) # end

Changing to TP mode

OST-CU-IPS01 (VS01-MCFR) # end
OST-CU-IPS01 # 
OST-CU-IPS01 #
```
