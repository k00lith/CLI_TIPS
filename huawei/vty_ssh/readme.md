
#### Увеличить кол-во одновременных сессий vty 

```bash
user-interface maximum-vty 21

user-interface vty 0 20
 acl 2001 inbound
 authentication-mode aaa
 idle-timeout 15 0
 protocol inbound ssh
#

commit trial 60

commit
```


#### Ограничить доступ по ssh , вешаем прямо на сервер ssh - так ограничим по всем VRF и всем интерфейсам

```bash
ssh server acl 2001
```

#### Тут всякие ACL-ки , команды
```bash
ssh server-source -i Ethernet0/0/0
ssh server-source -i Ethernet0/0/1
undo ssh server-source all-interface

user-interface vty 0 20
 acl 2001 inbound
 
 
acl number 2001
 description ***SSH_IN***
 rule 2 permit source 10.0.0.0 0.255.255.255
 rule 5 permit source 172.19.0.0 0.0.255.255
 
 
acl number 2002
 rule 1 permit vpn-instance __LOCAL_OAM_VPN__ source 172.16.0.0 0.15.255.255
 rule 5 permit vpn-instance __LOCAL_OAM_VPN__ source 10.0.0.0 0.255.255.255
 rule 10 permit vpn-instance __LOCAL_OAM_VPN__ source 192.168.0.0 0.0.255.255
 ```


