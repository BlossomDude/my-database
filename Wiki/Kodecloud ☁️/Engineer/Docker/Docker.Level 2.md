
# 1: Pull docker image
### Problem
```text
Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

a. Pull `busybox:musl` image on `App Server 3` in Stratos DC and re-tag (create new tag) this image as `busybox:blog`.
```

### Solution
```bash
docker pull busybox:musl
docker image tag busybox:musl busybox:blog
```




# 2: Docker Update Permissions
### Problem
```text
take docker rights to ammar 
```

### Solution
```bash
sudo usermod -aG docker ammar
```



# 3: Create a Docker Image From Container
### Problem
```text
One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:

a. Create an image `media:nautilus` on `Application Server 1` from a container `ubuntu_latest` that is running on same server.
```

### Solution
```bash
docker commit ubuntu_latest media:nautilus
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




