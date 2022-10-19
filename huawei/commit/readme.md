Данная заметка поможет начинающим пользователям понять базовые принципы работы c конфигурацией коммутаторов Huawei CloudEngine.

В операционной системе Huawei VRP есть несколько режимов :

#### User view
```bash
˂HUAWEI>
```
Пользовательский режим — доступен просмотр конфигурации и диагностика.

#### system-view
```bash
[~HUAWEI]
```
Режим конфигурации. В устройствах CloudEngine по умолчанию включен режим two-stage конфигурации, как на Juniper.

#### system-view immediately
```bash
[HUAWEI]
```
Режим конфигурации, при котором команды применяются немедленно.

Давайте рассмотрим two-stage режим более подробно, на примере настройки порта:

#### Показать текущий Активный конфиг.
```bash
[~HUAWEI]display current
```
#### Просмотр текущей конфигурации интерфейса:

```bash
[~HUAWEI]int 10GE 0/0/38
[~HUAWEI-10GE0/0/38]display this 
#
interface 10GE0/0/38
 description default-config
 shutdown
```
#### Включаем порт:
```bash
[~HUAWEI]int 10GE 0/0/38
[~HUAWEI-10GE0/0/38]
[~HUAWEI-10GE0/0/38]undo shut
[*HUAWEI-10GE0/0/38]
```
#### Смотрим внесенные изменения:
```bash
[*HUAWEI-10GE0/0/38]display configuration candidate changes
Building configuration
  #
  interface 10GE0/0/38
-  shutdown
  #
[*HUAWEI-10GE0/0/38]
```
#### Смотрим весь конфиг, с учетом текущих изменений :
```bash
[*HUAWEI]display configuration candidate merge
```
#### Отменить изменения:
```bash
[*HUAWEI]clear configuration candidate
```
#### Применить изменения:
```bash
[*HUAWEI]commit
```
#### Применить изменения с откатом через 5 минут :
```bash
[*HUAWEI]commit trial 300
Info: The system enters the trial configuration mode.
The system will revert to previous configuration if the trial configuration is not confirmed in 300 seconds. 
```
```bash
dis configuration trial status 
```
#### Что бы окончательно применить триальные изменения, необходимо в рамках текущей ssh\консольной сессии еще раз дать команду “commit”.
```bash
commit 
```
#### Если после триального коммита выкинуло из ssh сессии и получилось еще раз зайти по ssh или через консоль, то уже нельзя будет руками откатиться или применить триальные изменения, будет ругаться. Надо будет ждать пока истечет время триала.

#### Отменить trial конфигурацию и откатить немедленно (При необходимости, можно раньше времени откатиться руками - “abort trial”.):
```bash
[~HUAWEI]abort trial 
Warning: The trial configuration will be rolled back. Continue? [Y/N]:y
Info: The trial configuration rollback succeeded.
```
#### И сохранить :
```bash
[~HUAWEI]q
˂HUAWEI>save
Warning: The current configuration will be written to the device. Continue? [Y/N]:y
Now saving the current configuration ........
Info: Save the configuration successfully.
```
#### Откатить на один назад и посмотреть изменения:
```bash
˂HUAWEI>rollback configuration last 1
Warning: This operation will revert configuration changes to the previous status. Continue? [Y/N]:y
Loading rollback changes
Committing
Check rollback result
Configuration rollback succeeded.
Please use 'display configuration commit changes last 1' to view the changes.
```
#### Посмотреть список коммитов :
```bash
˂HUAWEI>display configuration commit list
```
#### Просмотр изменений в последнем коммите:
```bash
˂HUAWEI>display configuration commit changes last 1
Building configuration
  #
  interface 10GE0/0/38
+  shutdown
```
#### История изменений во всех коммитах, вывод списком:
```bash
˂HUAWEI>display configuration commit changes
Building configuration
Commit changes of commitId 1000001274 2019-05-26 13:48:57+03:00
  #
  interface 10GE0/0/38
-  shutdown
  #
``` 
Commit changes of commitId 1000001273 2019-05-26 13:46:11+03:00
  #
  interface 10GE0/0/38
+  shutdown
  #
```
#### Изменения в конкретном коммите:
```bash
˂HUAWEI>display configuration commit changes at 1000001252
Building configuration
  #
- vlan batch 4038 4043 to 4045
  #
+ vlan batch 4033 4038 4043 to 4045
```
#### Вывести изменения начиная с указанного коммита в сравнении с текущим конфигом:
```bash
˂HUAWEI>display configuration commit changes since 1000000810
```
#### Посмотреть изменения в последний 4 коммитах:
```bash
˂HUAWEI>display configuration commit changes last 4
```
#### В операционной системе VRP так же есть функционал алиасов, который может помочь при вводе длинных команд. Я в основном работаю с Juniper, поэтому использую короткие алиасы, похожие на Junos:
```bash
command alias
 alias shc1 command "display configuration commit changes last 1"
 alias shch command " display configuration candidate changes"
 alias show command display
 alias shr command "display current"
 alias sht command "display this"
 ```
