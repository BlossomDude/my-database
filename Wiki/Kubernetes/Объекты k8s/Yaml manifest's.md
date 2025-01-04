- [[#Binding]]
- [[#ConfigMap & Secret]]
- [[#Deployment]]
- [[#LimitRange]]
- [[#Namespace]]
- [[#Pod ]]
- [[#Replica Set]]
- [[#Service]]



У всех манифестов должны быть определены четыре блока:
```yaml
apiVersion:  # Версия Api для данного ресурса. ПОмогает правильно его обработать
kind:        # Тип объекта 
metadata:    # Метаданные. Указывать name нужно обязательно
spec:        # Основной блок. Спецификация нашего объекта.
```



## Binding
```
apiVersion: v1
kind: Binding
metadata:
  name: some-name
target:
  apiVersion: v1
  kind: Node
  name: node_name
```

## [[ConfigMap & Secret]]

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-cm
data:
  APP_COLOR: blue
  KEY: value
  ...
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
  DB_PASS: pa$$word
  TOKEN: HU&*%GY#(SJA
```


## [[Daemon Set]]

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-ds
spec:
  selector:
    matchLabels:
      agent: prometheus
  template:
    metadata:
      labels:
        agent: prometheus
    spec:
      containers:
        - name: agent
          image: prometheus-agent:latest
```

## [[Deployment]]


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  template:
    metadata:
      name: my-pod
    spec:
      containers:
        - name: container
          image: nginx
  replicas: 2  
  selector:
    matchLabels:
      type: back-end
```

## LimitRange
Для назначения дефолтных настроек requests & limits создается специальный объект k8s - Limit Range:

```
apiVersion: v1
kind: LimitRange
metadata:
  name: example-limitrange
  namespace: default  # Указываем namespace, в котором будет применяться LimitRange
spec:
  limits:
  - max:
      cpu: "2"        # Максимум 2 ядра CPU для контейнера
      memory: "1Gi"    # Максимум 1 ГБ памяти для контейнера
    min:
      cpu: "0.1"      # Минимум 0.1 ядра CPU для контейнера
      memory: "128Mi"  # Минимум 128 MiB памяти для контейнера
    default:
      cpu: "0.5"       # По умолчанию контейнер получает 0.5 ядра CPU
      memory: "256Mi"  # По умолчанию контейнер получает 256 MiB памяти
    defaultRequest:
      cpu: "0.2"       # Если контейнер не указывает requests, по ум. будет 0.2 CPU
      memory: "200Mi"  # По умолчанию контейнер получит 200 MiB памяти
    type: Container    # Тип ресурса: контейнер
```

## [[Namespace]]
```
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

Манифест ResourceQuota для namespace:
```
apiVersion: v1
kind: Namespace
metadata:
  name: my-rq
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memoty: 10Gi
    
  
```

## [[Pod]]

```yaml
 apiVersion: v1
  kind: Pod
    metadata:
      name: my-web
    spec:
      containers:
	  - name: container-web
	    image: httpd:latest
        command: ["sleep"]     # Переопределяет команду в ENTRYPOINT
        args: ["100"]          # Переопределяет параметр в CMD
	    env:
	      - name: polygon      # Пример использования переменной окружения
	        value: prod
	      - name: polygon      # Использование переменной из configMap 
	        valueFrom:
	          configMapKeyRef:
	            key: APP_COLOR
	            name: app_config
	      - name: polygon      # ИСпользование секрета как переменной
	        valueFrom:
	          secretKeyRef:
	    envFrom:               # Прикручивание CM в под
	      - configMapRef:
	        name: my-cmap       
        
        ports:
		- containerPort: 80 
		
```


## [[Replica Set]]

##### Replication Controller:
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-rc
spec:
  template:
    metadata:
      name: my-pod
    spec:
      containers:
        - name: container
          image: nginx
  replicas: 2  
```
##### Replica Set:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-rs
spec:
  template:
    metadata:
      name: my-pod
    spec:
      containers:
        - name: container
          image: nginx
  replicas: 2  
  selector:
    matchLabels:
      type: back-end
```

Все поды у которых в labels указан аналогичный label будут реплицированы силами RS:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: any-pod
  labels:
    type: back-end
...
```

## ResourceQuota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-rq
spec:
  hard:
    requests.cpu: 4
    requests.memory: 4Gi
    limits.cpu: 10
    limits.memory. 10Gi
    pods: "10"
    services: "10"
```

## [[Service]]

NodePort:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-svc
spec:
  type: NodePort
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30080
  selector:
    app: backend
```

ClusterIP:
```
apiVersion: v1
kind Service
metadata:
  name: my-svc
spec:
  ports:
    - port: 80
      targetPort: 80
  selector:
    matchLabels:
      app: backend
```
Не обязательно явно указывать type: ClusterIP, тк это тип является по умолчанию.


Load Balancer:
- Манифест файл сервиса типа LoadBalancer аналогичен NodePort. 
- Если вы не работаете с облачным провайдером то LoadBalncer будет иметь такой же эффект как и NodePort

