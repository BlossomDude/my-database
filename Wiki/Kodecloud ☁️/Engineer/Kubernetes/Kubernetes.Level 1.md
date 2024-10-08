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



# 2: Deploy Applications with Kubernetes Deployments

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
An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image `nginx:1.17` with the latest updates.

Execute a rolling update for this application, integrating the `nginx:1.17` image. The deployment is named `nginx-deployment`.

Ensure all pods are operational post-update.

`Note:` The `kubectl` utility on `jump_host` is set up to operate with the Kubernetes cluster

---

Приложение, которое в настоящее время выполняется в кластере Kubernetes, использует веб-сервер nginx. Команда разработчиков приложений Nautilus внесла некоторые изменения, которые требуют развертывания. Они создали образ nginx:1.17 с последними обновлениями.  
  
Выполните текущее обновление для этого приложения, интегрируя образ nginx:1.17. Развертывание называется nginx-deployment.  
  
Убедитесь, что все модули работают после обновления.  
  
Примечание: Утилита kubectl на jump_host настроена для работы с кластером Kubernetes
```

### Solution
```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
```



# 6: Revert Deployment to Previous Version in Kubernetes

### Problem
```
Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named `nginx-deployment`; initiate a rollback to the previous revision.

`Note:` The `kubectl` utility on `jump_host` is configured to interact with the Kubernetes cluster.

---

Ранее сегодня команда разработчиков Nautilus выпустила новую версию приложения. Однако клиент сообщил об ошибке, связанной с этим недавним выпуском. Следовательно, команда намерена вернуться к предыдущей версии.  
  
Существует развертывание с именем nginx-deployment; инициируйте откат к предыдущей версии.  

Примечание: Утилита kubectl на jump_host настроена для взаимодействия с кластером Kubernetes.
```

### Solution
```bash
kubectl get deployment
kubectl rollout undo deployment/nginx-deployment
```



# 7: Deploy Replica Set in Kubernetes Cluster

### Problem
```
The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:  
  
1. Create a ReplicaSet using `httpd` image with `latest` tag (ensure to specify as `httpd:latest`) and name it `httpd-replicaset`.          
2. Apply labels: `app` as `httpd_app`, `type` as `front-end`.    
3. Name the container `httpd-container`. Ensure the replica count is `4`.

---

Команда Nautilus DevOps готовится к развертыванию приложений в кластере Kubernetes для целей миграции. Перед членом команды была поставлена задача создать набор реплик, описанный ниже:  
   
1)Создайте набор реплик, используя httpd image с тегом latest (обязательно укажите как httpd:latest) и назовите его httpd-replicaset.    
2)Примените ярлыки: app - как httpd_app, введите как front-end.   
3)Назовите контейнер httpd-container. Убедитесь, что количество реплик равно 4.
```

### Solution
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      rsget: yos
  template:
    metadata:
      labels:
        rsget: yos
    spec:
      containers:
        - name: httpd-container
          image: httpd:latest
```



# 8: Schedule Cronjobs in Kubernetes

### Problem
```
The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below:  
  
1. Create a cronjob named `nautilus`.        
2. Set Its schedule to something like `*/2 * * * *`. You can set any schedule for now.            
3. Name the container `cron-nautilus`.        
4. Utilize the `nginx` image with `latest tag` (specify as `nginx:latest`).        
5. Execute the dummy command `echo Welcome to xfusioncorp!`.           
6. Ensure the restart policy is `OnFailure`.

---

Команда разработчиков Nautilus настраивает повторяющиеся задачи по разным расписаниям. В настоящее время они разрабатывают сценарии для периодического выполнения. Чтобы запустить процесс, они создают cron-задания в кластере Kubernetes с помощью команд-заполнителей. Следуйте приведенным ниже инструкциям.:

1)Создайте cronjob с именем nautilus.
2)Установите для него расписание примерно на */2 * * * *. На данный момент вы можете установить любое расписание.
3)Назовите контейнер cron-nautilus .
4)Используйте образ nginx с тегом latest (укажите как nginx: latest).
5)Выполните фиктивную команду echo Добро пожаловать в xfusioncorp!.
6)Убедитесь, что политика перезапуска выполнена по ошибке.

```

### Solution
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nautilus
spec:
  schedule: "*/2 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: cron-nautilus
              image: nginx:latest
              args:
                - /bin/sh
                - -c
                - echo "Welcome to xfusioncorp!"
          restartPolicy: OnFailure
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



