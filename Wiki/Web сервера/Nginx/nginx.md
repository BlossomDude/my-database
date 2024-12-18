---
Owner: Blossom Dude
Last edited time: 2023-12-28T12:06
Created time: 2023-07-19T13:08
---
>**web-сервер, обратный прокси-сервер, почтовый прокси-сервер и tcp/udp прокси-сервер.**

**/usr/share/nginx/html/index.html** - эта страница выводится у всех кто подключается к веб серверу
  
> [!Tip]  
> Генератор конфигов от Digital Ocean- https://www.digitalocean.com/community/tools/nginx

### Команды

`nignx -t` - проверить валидность конфигурации nginx
	`-T` - то же самое, только выводит весь конфиг в STDOUT
`nginx -s reload` - обновить конфигурацию nginx без остановки web-сервера
`nginx -v` - версия nginx, параметры сборки и тд.


/etc/nginx.conf - конфигурация nginx

```nginx
# Общая структура конфигурации nginx
...
http {
    ...
    server {
        ...
        location ... {
            ...
        }
    }
}
```
Блок Server определяет общие настройки вашего сайта, а location обрабатывает конкретные пути в адресах запросов. Server и location могут определятся несколько раз. 
Как внутри, так и снаружи блоков могут располагаться директивы - строки, содержащие имя директивы и её параметры и завершающиеся точкой с запятой.

> Важно понимать, что нужно придерживаться правила: Один сайт - один файл

Конечно, можно внести все настройки прямо в файл `nginx.conf`, но это неудобно. 

1) У вас может быть несколько сайтов. Тогда вам понадобится несколько блоков server, и файл станет настолько большим, что его будет трудно читать. 
2) Во-вторых, некоторые настройки могут повторяться в разных блоках, поэтому обычно их выносят в отдельные файлы и в нужных местах подключают директивой include. 
3) В-третьих, если вынести настройки каждого блока server в отдельную конфигурацию, очень удобно включать и отключать сайты простым переносом или переименованием всего одного файла. 
4) В-четвёртых, вы всегда будете видеть, какой именно сайт был изменён последним по времени модификации файла. 
5) И в пятых, хранение настроек сайта в отдельном файле сводит к минимуму риск случайного повреждения общей конфигурации при редактировании.

### Стандартный файл nginx.conf

``` nginx
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;
include /usr/share/nginx/modules/c.conf; # Добавляет конфигурации из файлов

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    include /etc/nginx/conf.d/c.conf;
    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;
        include /etc/nginx/default.d/c.conf;
        error_page 404 /404.html;
        location = /404.html {
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
}
```

# Обратный прокси

``` nginx
server {
  listen 80;
  location / {
    proxy_pass http://1.1.1.1;
  }
}
```

# Балансировщик нагрузки

``` nginx
upstream mywebservers {
 
  least_conn;                   # Параметр least_conn выбирает сервер с наименьшим кол-вом соединений       
  server 1.1.1.4 weight=3;       # Вес >1 указывает что сервер может обрабатывать больше запросов (по ум. вес=1)
  server 1.2.3.4;
  server 2.2.2.2 weight=2 down;  # Выключить сервер
  server 3.3.3.3 backup;         # Резервный сервер. Будет включен если один выйдет из строя
}

server {
  listen 80;
  location / {
    proxy_pass http://mywebservers;
  }
}
```
