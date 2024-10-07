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
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      name: httpd
      labels:
        app: httpd
    spec:
      containers:
        - name: front-end
          image: httpd:latest
```



# 3: Setup Kubernetes Namespaces and PODs

### Problem
```
The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:  

Create a namespace named `dev` and deploy a POD within it. Name the pod `dev-nginx-pod` and use the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.

`Note:` The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

Команда Nautilus DevOps планирует развернуть некоторые микросервисы на платформе Kubernetes. Команда уже настроила кластер Kubernetes и теперь хочет настроить некоторые пространства имен, развертывания и т.д. Основываясь на текущих требованиях, команда поделилась некоторыми подробностями, приведенными ниже:  
  
Создайте пространство имен с именем dev и разверните в нем модуль. Назовите модуль dev-nginx-pod и используйте образ nginx с тегом latest. Убедитесь, что в качестве тега указан nginx:latest.  
  
Примечание: Утилита kubectl на jump_host настроена для работы с кластером Kubernetes.
```

### Solution
```bash
# create namespace
kubectl create namespace dev

# Crate pod yaml
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
```



# 4: Set Resource Limits in Kubernetes Pods

### Problem
```
The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:

Create a pod named `httpd-pod` with a container named `httpd-container`. Use the `httpd` image with the `latest` tag (specify as `httpd:latest`). Set the following resource limits:

Requests: Memory: `15Mi`, CPU: `100m`
Limits: Memory: `20Mi`, CPU: `100m`

`Note:` The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

Команда Nautilus DevOps заметила проблемы с производительностью в некоторых приложениях, размещенных на Kubernetes, из-за нехватки ресурсов. Чтобы решить эту проблему, они планируют установить ограничения на использование ресурсов. Вот подробности:  
  
Создайте модуль с именем httpd-pod и контейнером с именем httpd-container. Используйте httpd-образ с тегом latest (укажите как httpd:latest). Установите следующие ограничения на ресурсы:  
  
Запросы: Память: 15 мб, процессор: 100 мб   
Ограничения: Память: 20 мб, процессор: 100 мб  

Примечание: Утилита kubectl на jump_host настроена для работы с кластером Kubernetes.
```

### Solution
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          cpu: 100m
          memory: 15Mi
        limits:
          cpu: 100m
          memory: 20Mi
```



# 5: Execute Rolling Updates in Kubernetes

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



