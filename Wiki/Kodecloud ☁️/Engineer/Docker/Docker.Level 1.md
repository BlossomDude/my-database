
# 1
### Problem
```text
Установить докер и запустить службу
```

### Solution
```bash
Следовал инструкции с оф. сайта по установке docker engine

sudo systemctl start docker
```




# 2
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



# 2
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



# 3
### Problem
```text

```

### Solution
```bash

```



# 3
### Problem
```text

```

### Solution
```bash

```
