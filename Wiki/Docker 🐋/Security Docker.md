---
Owner: Blossom Dude
Last edited time: 2024-07-11T17:37
Created time: 2024-07-11T15:47
---
# Docker demom

Тот кто внешне имеет доступ к Docker demon может полностью управлять вашими контейнерами и получить доступ root. По умолчанию докер предоставляет свою службу только через сокет - это безопасно. Но если требуется предоставить доступ извне то необходимо использовать tls в команде или добавить в файл `daemon.json`:

[[Основные команды]]

`dockerd` - запустить демон докер

`--host=1.1.1.1:2375` - по умолчанию десон докер доступен только внутри хоста, с помощью этого флага и айпи адреса мы можем открыть доступ извне.

> `2375` - порт докера по умолчанию не зашифрованный  
>   
> `2376` - порт докера зашифрованный

`--tls=true` - включить tls

`--tlscert=/var/docker/cer.cert` - указать сертификат

`--tlskey=/var/docker/key.pem` - указать ключ

`--tlscacert=cert`

`--tlsverify-true`
