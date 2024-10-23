---
Owner: Blossom Dude
Last edited time: 2023-12-18T15:48
Created time: 2023-12-18T15:02
---
web-server - Предназначен преимущественно для статических сайтов.

`apt install apache2` - установка apache

> [!important]  
> Логи находятся по пути : /var/log/apache2/access_log
> Логи ошибок доступа : /var/log/apache2/error_log  

  

- Файл конфигурации: `/etc/httpd/conf/httpd.conf
    
    `DocumentRoot /var/www/html` - директория где храняться все файлы сайта.