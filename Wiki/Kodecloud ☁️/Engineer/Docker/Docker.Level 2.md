
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
One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on `App Server 3` in `Stratos Datacenter`. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:

a. Install `apache2` in `kkloud` container using `apt` that is running on `App Server 3` in `Stratos Datacenter`.  
  
b. Configure Apache to listen on port `6200` instead of default `http` port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.  
  
c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.
```

### Solution
```bash
docker commit ubuntu_latest media:nautilus

```



# 4: Docker exec command
### Problem
```text
One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on `App Server 2` in `Stratos Datacenter`. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:

a. Install `apache2` in `kkloud` container using `apt` that is running on `App Server 2` in `Stratos Datacenter`.  
  
b. Configure Apache to listen on port `8089` instead of default `http` port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.   

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.
```

### Solution
```bash
docker exec -it kkloud /bin/sh

sed -i 's/Listen 80/Listen 8089/g' ports.conf
sed -i 's/:80/:8089/g' apache2.conf
sed -i 's/#ServerName www.example.com/ServerName localhost/g' 


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




