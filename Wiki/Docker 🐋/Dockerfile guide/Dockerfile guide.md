---
Owner: Blossom Dude
Last edited time: 2024-07-04T16:12
Created time: 2023-08-12T16:04
---
> [!important]  
> Dockerfile нужен для создания образа, он содержит инструкцию по созданию образа.[[Инструкции Dockerfile]]  
  
> [!important]  
> При установке пакетов в любом из Linux дистрибутивов желательно очищать кэш утилиты для работы с пакетами с целью экономии места.  
⠀Пример в нескольких дистрибутивах:Ubuntu RUN apt update && \  
apt upgrade -y && \  
apt install ... -y; && \  
apt clean #(или rm -rf /var/lib/apt/lists/*) && \  
rm -rf /var/cache/apt/archives /var/lib/apt/lists/*  
AlpineRUN apk add -U ...; \  
apk cache clean  
# Или просто  
RUN apk add -U --no-cache ...Так же, для оптимизации размера контейнера используй .Dockerignore  

Чек-лист для уменьшения размера образа:

- Изначально использовать легковесные образы (например alpine)
- Уменьшение колличества слоев путем объеденения комманд
    - `RUN command1 && command2`
- Используй `apt clean` или `rm -rf /var/lib/apt/lists/*`
- Копируй ==(COPY)== указывая точный путь чтобы не скопировать лишние файлы с ==(WORKDIR)== используй ==.==**==Dockerignore==**
- Удаление временных файлов.
- Использование многоступенчатой сборки.
    
    Сначала собирается образ с необходимыми инструментами для сборки, а затем копируются только необходимые файлы в новый, чистый образ.
    
    ```Docker
    # Стадия сборки
    FROM golang:1.13 AS build
    WORKDIR /src
    COPY . .
    RUN go build -o /bin/myapp
    
    # Финальный образ
    FROM alpine
    COPY --from=build /bin/myapp /bin/myapp
    ENTRYPOINT ["/bin/myapp"]
    ```