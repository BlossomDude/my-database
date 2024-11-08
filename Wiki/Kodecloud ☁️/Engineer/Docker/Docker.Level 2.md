
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

apt install apache2 -y

sed -i 's/Listen 80/Listen 8089/g' ports.conf
sed -i 's/:80/:8089/g' apache2.conf
sed -i 's/#ServerName www.example.com/ServerName localhost/g' apache2.conf

service apache2 start

```



# 5: Write Dockerfile
### Problem
```text
As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file `/opt/docker/Dockerfile` (please keep `D` capital of Dockerfile) on `App server 2` in `Stratos DC` and configure to build an image with the following requirements:  
  
a. Use `ubuntu` as the base image.   
b. Install `apache2` and configure it to work on `8089` port. (do not update any other Apache configuration settings like document root etc).
```

### Solution
```bash

```


# 4
### Problem
```text

```

### Solution
```bash

```




