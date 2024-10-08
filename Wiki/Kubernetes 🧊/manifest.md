---
Owner: Blossom Dude
Last edited time: 2023-12-13T11:55
Created time: 2023-11-28T20:25
---
## Значения полей

- `apiVersion: v1` - версия api объекта ==k8s==. Актуальную версию объекта можно узнать командой _`kubectl explain *obj_name*`_
- `kind: Pod` - тип объекта (Deployment, Service,ReplicaSet etc.)
- `metadata:` - обязательное поле для метаданных
    - `name: myapp-pod` - имя объекта, **обязательное единственное поле в metadata**
    - `labels:` - лэйблы, так называемые переменные-описания
        - `owner: nik` - пример обозначения лэйбла
- `spec:` - спецификация, для каждого объекта своя спецификация

  

- Pods
    
    ```YAML
    apiVersion: v1
    kind: Pod                      # то что мы создаем
    metadata:
    	name: my-web                 # имя пода
    	labels:                      # лэйблы, очень важная вещь
    		env  : prod                # можно определять любые
    		app  : main                # главное что-бы без пробелов
    		tier : frontend
    		owner: NikitaNikolaev
    spec:                          # specify
    	containers:
    	  - name: container-web      # имя контейнера
		    image: httpd:latest      # какой контейнер (можно менять на горячую)
    	    ports:
    		  - containerPort: 80    # порт открываемый на контейнере
    ```
    
- Replication Controller + Pods
    
    ```YAML
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
    spec: 
      template:                           # ниже идет описание пода
    		metadata:                           
    		  name: myapp-pod
    		spec:
    			containers:
    				- name:  nginx-container
    					image: nginx:latest
      replicas: 3                         # указывается минимальное колличество реплик
    ```
    
- Replica Set + Pods
    
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
    
- Deployment + Replica Set + Pods
    
    ```YAML
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    	name: myapp-deploy 
    spec:
    	template:
    		metadata:
    			name: myapp-pod
    			labels:
    				type: front-end
    		spec:
    			containers:
    				- name:  nginx-container
    					image: nginx:latest
    	replicas: 3
    	selector:                        
    		matchLabels:                    
    			type: front-end               
    ```
    
      
    
- Service
    
    ```YAML
    apiVersion: v1
    kind: Service
    metadata:
      name: my-service
    spec:
      type: NodePort            # Or LoadBalancer or ClusterIP(по ум.)
      ports:
        - targetPort: 80        # Порт на поде Если не указать то возьмет значение из port:
          port: 80              # Порт на объекте Service, един-ое ОБЯЗАТЕЛЬНОЕ ПОЛЕ
          nodePort: 30008       # Порт на ноде Если не указать то возьмет свободный порт из диапазона 30000-32767 
      selector:
        app:  myapp             # Поды с идентичными метками будет \
        type: frontend          # обрабатывать эта служба
    ```
    

  

---

- manifest
    
    ```YAML
    apiVersion : apps/v1            # api version k8s
    kind: Deployment                # oбозначаем наш деплой 
    metadata:                       # метаданные деплоя
      name: my-web-deployment
      labels: 
        app : my-k8s-app
    spec:                           # обозначаем с какими подами \
      selector:                     # наш деплой будет работать 
        matchLabels:                # ОН БУДЕТ РАБОТАТЬ С ПОДАМИ
          project: kgb              # которые соотв. лэйблу project: kgb
      template:                     # ниже до конца описываем pod
        metadata:
          labels:
            project: kgb
        spec:                       
          containers:
            - name: kgb-web
              image: adv4000/k8sphph:version1
              ports:
                - containerPort: 80
    
    ---
    apiVersion: autoscaling/v2beta2
    kind: HorizontalPodAutoscaler       # Создаем HPA
    metadata:
      name: my-autoscale                 # Имя HPA
    spec:
      scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment                 # Этот hpa будет скейлить
        name: my-web-deployment          # деплой с данным именем
      minReplicas: 3                     # Мин и макс реплик подов
      maxReplicas: 5                    
      metrics:                           # Контроль ресурсов
        - type: Resource
          resource:
            name: cpu
            targetAverageUtilization: 70 # Максимальная утилизация
        - type: Resource                 # cpu 70 процентов, а 
          resource:                      # ram - 80 процентов
            name: memory
            targetAverageUtilization: 80
    ```
    
    ```YAML
    apiVersion : apps/v1                    # DEPLOYMENT
    kind: Deployment
    metadata: 
      name: my-web-deployment-autoscaling
      labels:
        app: my-k8s-deployment
    spec:
      selector:
        matchLabels:
          project: kgb
    	template:
    		metadata:
    			labels:
    				project: kgb
    		spec:                               # 2 PODS
    			containers:
    				- name : kgb-web
    					image: adv4000/k8sphp:latest
    					ports:
    						- containerPort: 80
    			containers:
    				- name : not-my-web
    					image: tomcat:latest
    					ports:
    						- containerPort: 8080
    
    ---
    apiVersion: autoscaling/v2beta1          # AUTOSCALER
    kind: HorizontalPodAutoscaler
    metadata:
    	name: my-autoscaler
    spec: 
    	scaleTargetRef:                        # Описываем, какой деплой
    		apiVersion: apps/v2bets1v1           # будет скейлить HPA
    		kind: Deployment
    		name: my-web-deployment-autoscaling
    	minReplicas: 2
    	maxReplicas: 6
    	metrics:
    		- type: Resource
    			resource:
    				name: cpu
    				targetAverageUtilization: 70
    		- type: Resource
    			resource:
    				name: memory
    				targetAverageUtilization: 80
    ---
    apiVersion: v1                             # SERVICE
    kind: service
    metadata:
    	name: my-multi-pod-service
    	labels:
    		env  : prod
    		owner: me
    spec:
    	type: LoadBalancer                       # тип LB
    	selector:
    		project: kgb                    # Выбираем поды с этим лэйблом
    	ports:
    		- name      : my-web-app-listener      #прописываем лисенер
    			protocol  : TCP                      #к каждому контейнеру
    			port      : 80                       # порт на LB
    			targetPort: 80                       # порт на Pod
    
    		- name      : not-my-web-app-listener
    			protocol  : TCP
    			port      : 8080                     # порт на LB
    			targetPort: 8080                     # порт на Pod
    	
    ```
    
    ```YAML
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    	name: ingress-hosts
    
    spec:
    	rules:
    		- host: www.adv-it.net       # Если обратятся на это имя
    			http:
    				paths:
    					- backend:
    							serviceName: main  # перекинет на этот сервис
    							servicePort: 80    # и порт
    		
    		- host: web1.adv-it.net
    			http:
    				paths:
    					- backend:
    							serviceName: web1
    							servicePort: 80
    		
    		- host: web1.adv-it.net
    			http:
    				paths:
    					- path: "/page1"        # перекинет на 
    					- backend:              # web1.adv-it.net/page1
    							serviceName: web1
    							servicePort: 80
    ```