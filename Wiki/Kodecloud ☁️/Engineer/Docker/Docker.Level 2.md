
# 1: Pull docker image
### Problem
```text
Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

a. Pull `busybox:musl` image on `App Server 3` in Stratos DC and re-tag (create new tag) this image as `busybox:blog`.
```

### Solution
```bash

```




# 2: Deploy Nginx Container on Application Server
### Problem
```text
The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on `Application Server 2`. Complete the task with the following instructions:

1. On `Application Server 2` create a container named `nginx_2` using the `nginx` image with the `alpine` tag. Ensure container is in a `running` state.

---

Команда Nautilus DevOps проводит тесты развертывания приложений на выбранных серверах приложений. Для этого требуется развертывание контейнера nginx на сервере приложений 2. Выполните задачу, следуя приведенным ниже инструкциям:  

1. На сервере приложений 2 создайте контейнер с именем nginx_2, используя образ nginx с тегом alpine. Убедитесь, что контейнер находится в рабочем состоянии.
```

### Solution
```bash
sudo docker run -d --name nginx_2 nginx:alpine
```



# 3: Delete Docker Container
### Problem
```text
A container named `kke-container` was created by one of the Nautilus project developers on `App Server 2`. It was solely for testing purposes and now requires deletion. Execute the following task:  

Delete the `kke-container` on `App Server 2` in Stratos DC.

---

Контейнер с именем kke-container был создан одним из разработчиков Nautilus project на App Server 2. Он был создан исключительно для тестирования и теперь требует удаления. Выполните следующую задачу:  
  
Удалите kke-контейнер на App Server 2 в Stratos DC.
```

### Solution
```bash
sudo docker stop kke-container && docker rm kke-container 
```



# 4: Copy File to Docker Container
### Problem
```text
The Nautilus DevOps team possesses confidential data on `App Server 1` in the `Stratos Datacenter`. A container named `ubuntu_latest` is running on the same server.  
  
Copy an encrypted file `/tmp/nautilus.txt.gpg` from the docker host to the `ubuntu_latest` container located at `/tmp/`. Ensure the file is not modified during this operation.

---

Команда Nautilus DevOps располагает конфиденциальными данными на сервере приложений 1 в центре обработки данных Stratos. На том же сервере запущен контейнер с именем ubuntu_latest.  
  
Скопируйте зашифрованный файл /tmp/nautilus.txt.gpg с хоста docker в контейнер ubuntu_latest, расположенный по адресу /tmp/. Убедитесь, что файл не был изменен во время этой операции.
```

### Solution
```bash
sudo docker cp /tmp/nautilus.txt.gpg 61bda:/tmp/nautilus.txt.gpg
```



# 5: Troubleshoot Docker Container Issue
### Problem
```text
An issue has arisen with a static website running in a container named `nautilus` on `App Server 1`. To resolve the issue, investigate the following details:  
  

1. Check if the container's volume `/usr/local/apache2/htdocs` is correctly mapped with the host's volume `/var/www/html`.      
2. Verify that the website is accessible on host port `8080` on `App Server 1`. Confirm that the command `curl http://localhost:8080/` works on `App Server 1`.

---

Возникла проблема со статическим веб-сайтом, запущенным в контейнере с именем nautilus на сервере приложений 1. Чтобы устранить проблему, изучите следующие сведения:  

1.Проверьте, правильно ли сопоставлен том /usr/local/apache2/htdocs контейнера с томом /var/www/html хоста.  
2.Убедитесь, что веб-сайт доступен через порт хоста 8080 на сервере приложений 1.    Убедитесь, что команда curl http://localhost:8080/ работает на сервере приложений 1.

```

### Solution
```bash
docker run -d -v /var/www/html:/usr/local/apache2/htdocs --name nautilus -p 8080:80 httpd
```


# 4
### Problem
```text

```

### Solution
```bash

```




