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



```