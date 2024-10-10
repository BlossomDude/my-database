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



# 9: Create Countdown Job in Kubernetes

### Problem
```
The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:

1. Create a job named `countdown-datacenter`. 
2. The spec template should be named `countdown-datacenter` (under metadata), and the container should be named `container-countdown-datacenter`  
3. Utilize image `centos` with `latest` tag (ensure to specify as `centos:latest`), and set the restart policy to `Never`.   
4. Execute the command `sleep 5`

---
Команда Nautilus DevOps разрабатывает задания в кластере Kubernetes. Разрабатывая реальные сценарии/команды, они в настоящее время настраивают шаблоны и тестируют задания с фиктивными командами. Пожалуйста, создайте шаблон задания в соответствии с приведенными ниже подробностями: 

- Создайте задание с именем countdown-data center. 
- Шаблон спецификации должен называться countdown-datacenter (в разделе "метаданные"), а контейнер должен называться container-countdown-datacenter 
- Используйте centos-образ с тегом latest (обязательно укажите как centos:latest) и установите для политики перезапуска значение Never. 
- Выполните команду sleep 5

```

### Solution
```bash
apiVersion: batch/v1
kind: Job
metadata:
  name: nautilus
spec:
  template:
    metadata:
      name: spec-name
    spec:
	  containers:
		- name: cron-nautilus
		  image: centos:latest
		  args:
			- /bin/sh
			- -c
			- sleep 5
	  restartPolicy: Never
```



# 10: Set Up Time Check Pod in Kubernetes

### Problem
``` txt
The Nautilus DevOps team needs a time check pod created in a specific Kubernetes namespace for logging purposes. Initially, it's for testing, but it may be integrated into an existing cluster later. Here's what's required:

1. Create a pod called `time-check` in the `nautilus` namespace. The pod should contain a container named `time-check`, utilizing the `busybox` image with the `latest` tag (specify as `busybox:latest`).   
2. Create a config map named `time-config` with the data `TIME_FREQ=10` in the same namespace.   
3. Configure the `time-check` container to execute the command: `while true; do date; sleep $TIME_FREQ;done`. Ensure the result is written `/opt/data/time/time-check.log`. Also, add an environmental variable `TIME_FREQ` in the container, fetching its value from the config map `TIME_FREQ` key.    
4. Create a volume `log-volume` and mount it at `/opt/data/time` within the container.

---

Команде разработчиков Nautilus нужен модуль проверки времени, созданный в определенном пространстве имен Kubernetes для ведения журнала. Изначально он предназначен для тестирования, но позже может быть интегрирован в существующий кластер. Вот что требуется для этого.:

1. Создайте модуль с именем time-check в пространстве имен nautilus. Модуль должен содержать контейнер с именем time-check, использующий изображение busybox с тегом latest (укажите как busybox:latest).
2. Создайте карту конфигурации с именем time-config с данными TIME_FREQ= 10 в том же пространстве имен.
3. Настройте контейнер проверки времени для выполнения команды: while true; do date; sleep $TIME_FREQ;готово. Убедитесь, что результат записан /opt/data/time/time-check.log. Также добавьте переменную среды TIME_FREQ в контейнер, извлекая ее значение из ключа TIME_FREQ конфигурационной карты.
4. Создайте журнал тома-volume и смонтируйте его в /opt/data / time внутри контейнера.
```

### Solution
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: devops
data:
  TIME_FREQ: "3"

---

apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: devops
spec:
  volumes:
    - name: log-volume
      emptyDir: {}
  containers:
    - name: time-check
      image: busybox:latest
      volumeMounts:
        - mountPath: /opt/devops/time
          name: log-volume
      envFrom:
        - configMapRef:
            name: time-config
      command: ["/bin/sh", "-c"]
      args:
        [
        "while true; do date; sleep $TIME_FREQ;done > /opt/devops/time/time-check.log",
        ]



```


# 11: Resolve Pod Deployment Issue

### Problem
```
A junior DevOps team member encountered difficulties deploying a stack on the Kubernetes cluster. The pod fails to start, presenting errors. Let's troubleshoot and rectify the issue promptly.

  
1. There is a pod named `webserver`, and the container within it is named `httpd-container`, its utilizing the `httpd:latest` image.
    
2. Additionally, there's a sidecar container named `sidecar-container` using the `ubuntu:latest` image.
    

Identify and address the issue to ensure the pod is in the `running` state and the application is accessible.

`Note:` The `kubectl` utility on `ju`

---

Младший сотрудник команды DevOps столкнулся с трудностями при развертывании стека в кластере Kubernetes. Модуль не запускается, что приводит к появлению ошибок. Давайте быстро разберемся с неполадками и устраним проблему.  
  
Существует модуль с именем webserver, а контейнер внутри него называется httpd-container, в котором используется образ httpd:latest.  
  
Кроме того, есть контейнер sidecar с именем sidecar-container, использующий ubuntu:latest image.  
  
Определите и устраните проблему, чтобы убедиться, что модуль находится в запущенном состоянии и приложение доступно.
```

### Solution
```bash
kubectl get pods -o yaml > pod.yaml
```



# 12: Update Deployment and Service in Kubernetes

### Problem
```
An application deployed on the Kubernetes cluster requires an update with new features developed by the Nautilus application development team. The existing setup includes a deployment named `nginx-deployment` and a service named `nginx-service`. Below are the necessary changes to be implemented without deleting the deployment and service:

1.) Modify the service nodeport from `30008` to `32165`
2.) Change the replicas count from `1` to `5`
3.) Update the image from `nginx:1.18` to `nginx:latest`

---

Приложению, развернутому в кластере Kubernetes, требуется обновление с новыми функциями, разработанными командой разработчиков приложений Nautilus. Существующая установка включает в себя развертывание с именем nginx-deployment и службу с именем nginx-service. Ниже приведены необходимые изменения, которые необходимо внести без удаления развертывания и службы.:  
    
1.) Измените порт служебного узла с 30008 на 32165   
2.) Измените количество реплик с 1 на 5
3.) Обновите изображение с nginx:1.18 на nginx:latest
```

### Solution
```bash
#1
kubectl edit svc nginx-service
#2
kubectl edit deploy nginx-deployment
#3
kubectl set image deployment/nginx-deployment nginx-container=nginx:latest
```



# 13: Deploy Highly Available Pods with Replication Controller

### Problem
```
The Nautilus DevOps team is establishing a `ReplicationController` to deploy multiple pods for hosting applications that require a highly available infrastructure. Follow the specifications below to create the `ReplicationController`:

1. Create a `ReplicationController` using the `httpd` image, preferably with `latest` tag, and name it `httpd-replicationcontroller`.
    
2. Assign labels `app` as `httpd_app`, and `type` as `front-end`. Ensure the container is named `httpd-container` and set the replica count to `3`.  

All `pods` should be running state post-deployment.  

---

Команда разработчиков Nautilus разрабатывает ReplicationController для развертывания нескольких модулей для размещения приложений, которым требуется высокодоступная инфраструктура. Чтобы создать ReplicationController, следуйте приведенным ниже спецификациям:  
  
Создайте ReplicationController, используя httpd-образ, желательно с тегом latest, и назовите его httpd-replicationcontroller.  
  
Присвойте приложению labels имя httpd_app и введите как front-end. Убедитесь, что контейнер называется httpd-container, и установите число реплик равным 3.  
  
Все модули должны быть запущены после развертывания.
```

### Solution
```bash
apiVersion: v1
kind: ReplicationController
metadata:
  name: httpd-replicationcontroller
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
        - name: httpd-container
          image: httpd:latest
```



# 14: Resolve Volume Mounts Issue in Kubernetes

### Problem
```
We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:  

The pod name is `nginx-phpfpm` and configmap name is `nginx-config`. Identify and fix the problem.  

Once resolved, copy `/home/thor/index.php` file from the `jump host` to the `nginx-container` within the nginx document root. After this, you should be able to access the website using `Website` button on the top bar.

---

Сегодня утром мы столкнулись с проблемой при настройке Nginx и PHP-FPM в кластере Kubernetes, которая привела к остановке его работы. Изучите и устраните проблему:  
  
Имя модуля - nginx-phpfpm, а имя configmap - nginx-config. Определите и устраните проблему.  
  
После устранения скопируйте файл /home/thor/index.php с хоста jump в nginx-контейнер в корневом каталоге документов nginx. После этого вы сможете получить доступ к веб-сайту, используя кнопку "Веб-сайт" на верхней панели.
```

### Solution
```bash

```



```yaml
1: /usr/official-t1q2/official-t1q2.yml
---------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: official-nginx-t1q2
spec:
  containers:
    - name: container-nginx
      image: nginx:latest
---------------------------------------
2

```