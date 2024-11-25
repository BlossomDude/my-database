---
Owner: Blossom Dude
Last edited time: 2024-07-05T10:39
Created time: 2024-07-05T10:18
---
```Docker
...
RUN --mount=type=cache,mode=0777,target=/var/cache/apt \
    apt-get update && apt-get install -y git
...
```

У сборщика buildkit появилась возможность использовать кэш из других билдов.

Когда этот слой будет запущен на сборку, Docker сначала подгрузит данные из специального тома в директорию `**/var/cache/apt**`, и внутренний apt будет использовать эти файлы в своей работе. В конечный образ этот каталог не пойдёт, в образ попадёт только результат работы `**apt**`.

`docker buildx build --output type=registry` - отправит образ в реджистри, если выберем тип `image`, то просто соберет образ.

  

Для ускорения других сборок, BuildKit поддерживает экспорт своего локального кэша сборки во внешнее расположение.

Для работы с кэшем в драйвере по умолчанию есть механизмы `**inline**` и `**local cache**`.

- ==inline== встраивает кэш сборки в собираемый образ из другого локального образа.
- ==local==: записывает кэш сборки в локальный каталог файловой системы.
- ==registry==: встраивает кэш сборки в отдельный образ и помещает его в отдельный реестр.
- ==gha==: отправляет кэш в GitHub Actions cache.
- ==s3==: загружает кэш сборки в S3-bucket.
- ==azblob==: загружает кэш в хранилище BLOB-объектов Azure.

```Bash
docker buildx build --push -t <registry>/<image> \
  --cache-to type=inline,ref=<registry>/<cache-image>[,parameters...] \
  --cache-from type=local,src=path/to/local/dir .
```

Итак, эти два механизма для разных целей:

- `**cache-to**` и `**cache-from**` используются для сохранения результата шагов сборки и повторного использования из в будущих сборках, что позволяет избежать необходимости повторной сборки слоя. Кэш хранится в постоянном месте вне сборщика, например, в реджистри, а другие сборщики могут скачать кэш и просто пропустить ранее завершенные шаги сборки. Т.е. они могут не иметь образа в локальной системе, но все равно использовать кэш.
- `**-mount type=cache**` создает монтирование внутри временного контейнера, который выполняется в инструкции `**RUN**`. Файлы из этого монтирования повторно используются при последующих выполнениях сборки. Это полезно, когда на шаге есть множество внешних зависимостей, которым нужно присутствовать в образе и которые можно безопасно переиспользовать между сборками. Это хранилище кэша является локальным для сборщика и представляет собой пустой каталог при первом использовании.