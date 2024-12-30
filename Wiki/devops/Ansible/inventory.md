---
Owner: Blossom Dude
Last edited time: 2024-02-20T21:19
Created time: 2023-11-17T10:17
---
> [!important]  
> Для того чтобы отметить группу необхоимо сделать запись следующего вида:[web_servers]nginx apache nginx2 Так же для удобства нужно всегда создавать группу [all_servers:children]в эту группу будут входить только группы. Обязательно добавить children. Это определяет то что в эту группу будут входить другие группы.[all_servers:children]web_serversbackend_serversdatabases  

В данном файле содержаться информация об управляемых хостах.
ПРИМЕР:
`target1 ansible_host=172.19.91.16 ansible_user=root ansible_ssh_pass=password123`

  

Параметры инвентори:

`ansible_connection=(`**`ssh/winrm/local/docker`**`)` - тип соединения
`ansible_host=172.19.91.16` - ip адресс хоста или **dns-имя**
`ansible_password` - пароль для подключения к windows
`ansible_port=22/5986` - определяет на какой порт подключаться (**22 по умол.**)
`ansible_ssh_pass=password123` - пароль к уз пользователся
`ansible_user=root` - пользователь под каким будем логинится (**root по умол.**)