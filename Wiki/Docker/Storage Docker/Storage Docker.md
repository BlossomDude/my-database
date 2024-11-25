---
Owner: Blossom Dude
Last edited time: 2024-07-08T17:19
Created time: 2023-11-13T20:24
---
  

> [!important]  
> Файловая система на хосте:📁/var/lib/docker📁overlay2📁containers📁image📁volumes  

  

> Образ докер имеет слоеную архитектуру, и изменить образ после сборки нельзя, только через запуск новой сборки. Только верхний слой является rw. Этот слой существует пока жив контейнер, если контейнер разрушается, слой уничтожается.

  

**Copy-On-Write**

Если во время работы контейнера я измени файл из нижнего слоя, по сути файл этот не изменится, я буду редактировать его копию перемещенную в верхний слой, **поэтому сохраняется целостность образа.**

  

### В Docker в данный момент есть 4 типа монтирований:

  

- первый тип - монтирование во время сборки , он берёт данные только из контекста сборки.

```Docker
RUN --mount=type=bind,source=key.json,destination=cloud-key.json \
    ls -al > ls.txt
```

`docker image build --secret id=my-cloud-key,src=key.json -f Dockerfile -t test .`

  

- второй тип - `**bind mount**`, когда файл или директория хоста монтируется внутрь контейнера.
- третий тип - `**tmpfs**` (для Windows есть своя техника `**Named pipes**`)
- четвертый - использовать объект `**volume**`

  

  

Длинная нотация  
  

![[/Untitled 8.png|Untitled 8.png]]

  

Плюсы определенного монтирования.  
  

Volumes

- убрать зависимость размещения контейнера от одного docker-хоста.
- убрать отличия конфигураций разных docker-хостов.
- организовать удалённое хранение данных.
- единая точка управления данными: перенос данных, бэкап и т.д.
- расшарить данных между контейнерами.
- ускорение работы Docker Desktop на платформах, отличных от Linux.

Bind

- при разработке приложения, например работая по методике dev-container. Или для отладки приложения в определенной среде, когда код можно править на хосте, а запускать в контейнере.
- прокинуть конфигурационные файлы хоста в контейнер.
- расшарить директорию между контейнерами, в основном используется для передачи настроек приложения.
- для запуска systemd внутри контейнера.

TMP

- хранение чувствительной информации
- создание буфера для операций загрузки/выгрузки
- хранение временных файлов с высокой скоростью доступа