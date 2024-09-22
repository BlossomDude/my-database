---
Owner: Blossom Dude
Last edited time: 2024-02-02T16:36
Created time: 2023-12-23T18:18
---
- Приложение написано на Go, можно скачать архив с оф сайта.  
    Конфигурация находится по пути  
    `/etc/prometheus/prometheus.yml`
    
    ```Bash
    - job_name: 'Jenkins'
     metrics_path: /prometheus/
     static_configs:
     - targets: ['localhost:8085']
    ```
    

[[Типы метрик]]