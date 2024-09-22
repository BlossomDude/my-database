---
Owner: Blossom Dude
Last edited time: 2024-01-29T15:53
Created time: 2023-10-24T09:59
---
[[Pipeline’s]]


> [!important]  
> Переменные GITLAB  

- Разворачивание на докер:
    
    ```Bash
    sudo docker run --detach \
    --hostname gitlab.learn-cicd.com \
    --publish 8443:443 --publish 8080:80 --publish 8022:22 \
    --name gitlab \
    --restart always \
    --volume $GITLAB_HOME/config:/etc/gitlab \
    --volume $GITLAB_HOME/logs:/var/log/gitlab \
    --volume $GITLAB_HOME/data:/var/opt/gitlab \
    --shm-size 256m \
    gitlab/gitlab-ce:latest
    ```
    
    ```Bash
    sudo docker exec -it gitlab /bin/bash #Заходим на контейнер/
    ```
    
    ```Bash
    gitlab-ctl start #Запускаем службу гитлаб
    ```
    
    ```Bash
    sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
    # узнаем пароль и переходим на сайт
    ```
    
- Разворачивание Раннера
    
    ```Bash
    docker run -d --name gitlab-runner --restart always \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -v gitlab-runner-config:/etc/gitlab-runner \
        --env TZ="Europe/Moscow" \
    		--link gitlab:gitlab-ci \
        gitlab/gitlab-runner:latest
    ```
    

---

**CI - Continuous Integration** - Непрерывная интеграция

**CD - Continuous Delivery/Development**(без админа, все на автомате) - непрерывня доставка

![[/Untitled 14.png|Untitled 14.png]]