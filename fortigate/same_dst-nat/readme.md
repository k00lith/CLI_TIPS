НАТ снаружи внутрь, например у нас есть адрес 5.5.5.5 на фортике, нужно сделать нат:

```
5.5.5.5 port 22 --> 192.168.0.10 port 22
5.5.5.5 port 22 --> 192.168.0.12 port 22
```

Это делается по фильтру сорс адреса, конечно, если они статичсекие и мы их знаем
  
```
 config firewall vip     
 
 edit "VIPtest1"         
 set src-filter "1.1.1.1"         
 set service "SSH"         
 set extip 5.5.5.5         
 set extintf "WAN_INTERFACE"         
 set portforward enable         
 set mappedip "192.168.0.10"         
 set mappedport 22     
 
 next     
 edit "VIPtest2"         
 set src-filter "2.2.2.2"         
 set service "SSH"         
 set extip 5.5.5.5         
 set extintf "WAN_INTERFACE"         
 set portforward enable         
 set mappedip "192.168.0.12"         
 set mappedport 22     
 
 next 
 end
 ```
