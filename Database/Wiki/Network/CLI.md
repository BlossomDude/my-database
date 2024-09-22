---
Owner: Blossom Dude
Last edited time: 2023-08-22T21:39
Created time: 2023-08-22T17:10
---
Используйте Tab и “?” для удобства ввода

`enable` - перейти в превилигированный режим

`write` - сохранить **running-config** в **startup-config**

`configure terminal` - режим глобальной конфигурации.

- `enable password *pass123*` - задать пароль на enable
    - `service password-encryption` - зашифрует все пароли
- `enable secret *pass123*` - более безопасный вариант шифрования пароля

> [!important]  
> “no” перед командами выше удалит пароль  

Существует два конфигурационных файла:

- **Running-config** - файл который мы редактируем в текущей сессии
- **Startup-config** - файл который используется при включении устройства