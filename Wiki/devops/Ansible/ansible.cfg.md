Файл с настройками по умолчанию. 
Находится  в `/etc/ansible/ansible.cfg`

Содержит в себе настройки ансибла. Такие как: 

```
[defaults]

remote_user = username - Добавить пользователя SSH по умолчанию.
host_key_checking = False - отключить проверку ssh ключей по ум.
inventory = <path> - путь к инвентори
log_path = <path> - путь к логам
library =
roles_path =
timeout =  - таймаут SSH

[inventory]

enable_plugins = - включенные плагины
```
и тд.