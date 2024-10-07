# 1: Deploy Pods in Kubernetes Cluster

### Problem
```
he Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

1. Create a pod named `pod-httpd` using the `httpd` image with the `latest` tag. Ensure to specify the tag as `httpd:latest`.   
2. Set the `app` label to `httpd_app`, and name the container as `httpd-container`.

`Note`: The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

Команда разработчиков Nautilus внедряется в Kubernetes для управления приложениями. Перед одним из членов команды поставлена задача создать модуль в соответствии с приведенными ниже подробностями:

1)Создайте модуль с именем pod-httpd, используя изображение httpd с тегом latest . Обязательно укажите тег как httpd:latest .
2)Установите для метки приложения значение httpd_app и назовите контейнер httpd-container .

Примечание: Утилита kubectl на jump_host настроена для работы с кластером Kubernetes.

```

### Solution
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
```



# 2

### Problem
```
The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details: 

Create a deployment named `httpd` to deploy the application `httpd` using the image `httpd:latest` (ensure to specify the tag)

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

Команда разработчиков Nautilus изучает возможности Kubernetes для управления приложениями. Одному из членов команды необходимо создать развертывание, следуя этим инструкциям:  
  
Создайте развертывание с именем httpd для развертывания приложения httpd с использованием образа httpd:latest (обязательно укажите тег).  
  
Примечание: Утилита kubectl на jump_host настроена для взаимодействия с кластером Kubernetes.
```

### Solution
```bash

```



# 3

### Problem
```


---


```

### Solution
```bash

```



# 4

### Problem
```


---


```

### Solution
```bash

```



# 5

### Problem
```


---


```

### Solution
```bash

```



# 6

### Problem
```


---


```

### Solution
```bash

```



# 7

### Problem
```


---


```

### Solution
```bash

```



# 8

### Problem
```


---


```

### Solution
```bash

```



# 9

### Problem
```


---


```

### Solution
```bash

```



# 10

### Problem
```


---


```

### Solution
```bash

```



# 11

### Problem
```


---


```

### Solution
```bash

```



# 12

### Problem
```


---


```

### Solution
```bash

```



# 13

### Problem
```


---


```

### Solution
```bash

```



# 14

### Problem
```


---


```

### Solution
```bash

```



