
- [[#Pod ]]
- [[#Replica Set]]
- [[#Deployment]]
- [[#Service]]
- [[#Namespace]]

У всех манифестов должны быть определены четыре блока:
```yaml
apiVersion:  # Версия Api для данного ресурса. ПОмогает правильно его обработать
kind:        # Тип объекта 
metadata:    # Метаданные. Указывать name нужно обязательно
spec:        # Основной блок. Спецификация нашего объекта.
```


## [[Pod]]

```yaml
 apiVersion: v1
  kind: Pod
    metadata:
      name: my-web
        labels:          
          env  : prod                
          app  : main
    	  tier : frontend
    	  owner: NikitaNikolaev
    spec:
      containers:
	    - name: container-web
		  image: httpd:latest
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


## [[Deployment]]


Файл конфигурации выглядит аналогично как и у RS:

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
    matchLabels:
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