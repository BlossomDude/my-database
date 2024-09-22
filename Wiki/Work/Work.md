---
Owner: Blossom Dude
Last edited time: 2024-08-09T12:06
Created time: 2024-03-04T11:56
---
> [!important]  
> ACTUAL TASKS[[76323]][[заявка серты]]  

  

[[Доступы]]

[[KotlinDSL]]

[[Ссылки]]

[[Полигоны]]

[[Вопросы]]

[[Контактные лица]]

[[TeamCity]]

[[Термины]]

[[actuator]]

[[Quality Gate]]

[[AppSec]]

[[Wiki/Work/Проблемы и решения]]

[[Процесс сборки библиотек]]

[[Инициализация серверов]]

[[Wiki/Work/Ansible]]

[[OpenIG - EPA]]

[[backlog]]

[[Omni-api-gateway]]

> [!important]  
> Переход с F5 на Nginx![[Untitled.png]]  
  

- Проблема 404
    
    Проблема 404 у кого либо  
    Попросить скинуть богданов трейс, curl -v  
    
      
    

Как проходит запросы:  
IBM MQ → MSB → Nginx (  
~~F5~~)

IBM MQ → MSB → Nginx balancer (переходим на это)

Если в теге есть v то он отбрасывается

  

> [!important]  
> Static приложения - в нем набор файлов и\или конфигов находятсЯ в других репах  

  

1) Настроить автомердж на П2

2) Сделать триггер на джобу deploy_routes

*omni-devops-avtomatic - репа с тригерром