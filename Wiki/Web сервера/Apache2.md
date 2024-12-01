---
Owner: Blossom Dude
Last edited time: 2023-12-18T15:48
Created time: 2023-12-18T15:02
---

`apt install httpd - установка apache

> [!important]  
> Логи находятся по пути : /var/log/httpd/access_log
> Логи ошибок доступа : /var/log/htppd/error_log  

  

- Файл конфигурации: `/etc/httpd/conf/httpd.conf

``` yaml
# Порт на котром будет работать сервер 
Listen 80

# директория где храняться все файлы сайта.
DocumentRoot "/var/www/html" 

# DNS имя сервера
ServerName www.dnsname.com:80 

# Объявить виртуальный хост
<VirtualHost *:80>
  ServerName www.otherdns.com
  DocumentRoot /var/www/otherdns
</VirtualHost>

```


##### Добавление заголовков (headers)
`httpd.conf:`
Header always set X-XSS-Protection "1; mode=block"
Header always set X-Frame-Options "SAMEORIGIN"
Header always set X-Content-Type-Options nosniff