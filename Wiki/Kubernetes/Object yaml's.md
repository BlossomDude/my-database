У всех манифестов должны быть определены четыре блока:
```yaml
apiVersion:  # Версия Api для данного ресурса. ПОмогает правильно его обработать
kind:        # Тип объекта 
metadata:    # Метаданные. Указывать name нужно обязательно
spec:        # Основной блок. Спецификация нашего объекта.
```


## Pod

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

## Replication Controller & Replica Set

Данные объекты нужны для автоматического создания и поддержания требуемого количество реплик подов в кластере, что обеспечивает высокую доступность и отказоустойчивость.

- Replication Controller на данный момент считается старой версией Replica Set.

- Главное отличие в них, это то что Replica Set требует наличие блока selector. В то время как в RC вы можете указывать, а можете не указывать этот блок. 

- Selector позволяет благодаря сопоставлению меток создавать не только реплики пода который указан в блоке spec, но и уже существующих и будущих подов.   


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
    MatchLabels:
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