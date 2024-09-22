---
Owner: Blossom Dude
Last edited time: 2024-07-05T12:00
Created time: 2024-07-05T11:54
---
```Docker
FROM node AS builder

COPY . ./

RUN yarn install
RUN yarn build

FROM nginx AS ready_app

COPY --from=builder dist /usr/share/nginx/html

CMD [ "nginx", "-g", "daemon off;" ]
```

Пример Dockerfile многоэтапной сборки.  
В нем создается сначала приложение, и передается в другой контейнер для запуска.  

  

- multi-stage builds могут помочь оптимизировать Dockerfiles: сделать их логику более наглядной, а их операции простыми для чтения и поддержки
- они помогают нам сохранять размер образов небольшим
- многоэтапные сборки помогают нам избежать необходимости создавать и обслуживать несколько Dockerfiles и их скриптовую `**обмазку**`
- помогает избежать создания промежуточных образов