---
Owner: Blossom Dude
Last edited time: 2024-02-02T18:24
Created time: 2023-11-15T00:35
---
  

Сервер-хранилище образов Docker.

Самым простым примером является **DockerHub**.

Так же им может являться частный сервер, локальный и тд.

  

## Развертывание Private registry

Для Разворачивание своего частного реестра, необходимо развернуть контейнер:

`docker run -d -p 5000:5000 --name registry registry:2` - Разворачивание стандартного официального образа реестра докер.

`docker image tag my-image` `[localhost](http://localhost)``:5000/my-image`

`docker push localhost:5000/my-image` - Загружаем на него собственный образ с тэгом `my-image`