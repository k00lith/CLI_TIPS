
Делаем под root

1. вытаскиваем /config/juniper.conf.gz из донора
2. удаляем у целевого SRX файл /config/juniper.conf.gz
3. закачиваем из донора в целевой /config/juniper.conf.gz
4. из режима конфигурации выполняем load override /config/juniper.conf.gz
5. делаем commit
6. заходим по консоле
7. делаем run request system autorecovery state save
8. делаем request system reboot

смотрим ошибки 

show system alarms
