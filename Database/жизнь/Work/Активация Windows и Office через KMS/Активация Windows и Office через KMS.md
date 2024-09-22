## Активация Windows

Запустить от имени Администратора "cmd"

и ввести следующие команды

1. Задать сервер KMS

**slmgr /skms** **lan****-kms1.lan.lanit.ru****:1688**

1. Для запуска активации введите следующую команду

**slmgr /ato**

## Активация Office

Запустить от имени Администратора "cmd"

и ввести следующие команды

1. Перейти в папку со скриптом ospp.vbs

Для Office 2016:

**cd "C:\Program Files (x86)\Microsoft Office\Office16"**

1. Задать адрес сервера KMS

**cscript ospp.vbs /sethst:lan****-kms1.lan.lanit.ru**

Если все верно сделано должно появиться  сообщение:

_**Successfully applied setting.**_

1. Для запуска активации введите следующую команду

**cscript ospp.vbs /act**