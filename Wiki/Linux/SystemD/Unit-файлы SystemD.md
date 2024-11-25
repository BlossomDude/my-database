---
Owner: Blossom Dude
Last edited time: 2023-12-14T15:37
Created time: 2023-12-14T15:10
---
[Гайд по Unit-файлам](https://linux-notes.org/pishem-systemd-unit-fajl/)

> [!important]  
> systemctl daemon-reload - прикажет systemd прочитать заново unit-файлы и определить новые.  

  

Unit-файлы systemD нужны для того, чтобы определять свое приложение как службу и задавать правила поведения.

  

Unit-файл ниже - определит [run.py](http://run.py) как службу в ОС.

```Bash
[Unit]    # Описание
Description=Simple Python App
# Указывает команду man с документацией
Documentation=man:systemd-sysv-generator(8) 
# Запустить после определенного(ых) сервиса 
After=postgresql.service

[Service]
# Задание которое будет выполнено до старта службы
ExecStartPre=/opt/web/backup_db.sh
# определяет приложение как сервис
ExecStart=/usr/bin/python3 /opt/web/run.py
# Задание которое будет выполнено после старта
ExecStartPost=/opt/web/send_notification.sh
# Задание которое будет выполнено во время reload
ExecReload=/opt/web/reload.sh
# При завершении процесса завершить только этот процесс а не дочерние
KillMode=process
# Выбрать пользователя владельца сервиса
User=root
# После падения сервера, сервис будет запускаться автоматом
Restart=Always
# После падения сервиса, будет ждать 10 секунд, после перезапустится
RestartSec=10 

[Install]
# ждет загрузки 3 уровня загрузки (non-graphic)
WantendBy=multi-user.target 
```


Примеры значений некоторых переменных.

`KillMode=control-group` - убьет все дочерние процессы
`Restart=on-failure` - перезагружать при ошибке
`Restart=always` - Перезагружать всегда
`RestartSec=1` - Подождать 1 сек перед перезагрузкой
`RestartPreventExitStatus=255` - Не будет перезагружать приложение если статус 255
