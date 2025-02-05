
Ingress Controller - решает проблему управления входящем трафиком, направляя его к нужным сервисам. А так же:
- Маршрутизация трафика
- Балансировка нагрузки
- Терминация TLS/SSL. 
	Снимает нагрузку по шифрованию и дешифрованию с сервисов приложений.
- Мониторинг и логирование

Существует много разных сторонних решений. Например nginx, GCE, traefik, HAproxy etc. (Первые два поддерживаются проектом kubernetes.) 
`Ingress Controller` не поставляется по умолчанию с кластером `kubernetes`.

---
##### Настройка Ingress Controller
Ingress Controller разворачивается в Deployment.
На примере nginx:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-ingress
  template:
    metadata;
      labels:
        app: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: <nginx-controller>
          
		  # Запуск ingress и добавление файла конфигурации из CM
		  args: 
		    - /nginx-ingress-controller
		    - --configmap=$(POD_NAMESPACE)/nginx-configuration
		  
		  ## Нужно передать две переменные для работы nginx
		  env:
		    - name: POD_NAME
		      valueFrom:
		        fieldRef:
		          fieldPath: metadata.name
		    - name: POD_NAMESPACE
		      valueFrom:
		        fieldRef:
		          fieldPath: metadata.namespace
		  
		  ports:
		    - name: http
		      containerPort: 80
		    - name: https:
		      containerPort: 443
```

Так же необходим Config Map Для написания в него конфигурации nginx:
```yaml
apiVersion: v1
kind: Configmap
metadata:
  name: nginx-configuration 
```

Еще необходимо создать Service типа NodePort для доступа к внешнему миру:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
spec:
  type: NodePort
  ports:
    - port: 80
      targetport: 80
      protocol: TCP
      name: http
    - port: 443
      targetport: 443
      protocol: TCP
      name: https
      
```

И для работы ingress controller так же необходим ServiceAccount с правильно настроенными ролями и привязкой роли.
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: ngin-controller-service-account
```

---
##### Ingress Resources

Набор правил которые вы настраиваете на Ingress Controller'е называется `Ingress Resources`

Для этого создается файл с типом `Ingress`:

- С одним путем/хостом: 
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  backend:
    service:
      name: wear-service
      port: 
	    number: 80
```

- С несколькими путями
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - http:
      paths:
      - path: /wear
        pathType: Prefix
        backend:
          service:
            name: wear-service
            port: 
              number: 80
      - path: /watch
        backend:
          service:
            name: watch-service
            port:
              number: 80
```

- С несколькими хостами
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - host: wear.hostname.com
    http:
      paths:
      - backend:
          service:
            name: wear-service
            port: 
              number: 80
  - host
    http:
      paths:
	  - backend:
	      service:
		    name: watch-service
		    port:
		      number: 80
```

- Мы можем создавать Ingress используя команды:
```bash
kubectl create ingress ingress-test \
	--rule="wear.my-online-store.com/wear*=wear-service:80"**
```

---
## Rewrite target option

Опция **`rewrite-target`** в Kubernetes Ingress используется для изменения пути запроса перед его отправкой в backend-сервис. Это позволяет абстрагировать клиентский путь от внутреннего пути, используемого вашим приложением.

- Когда Ingress получает HTTP-запрос, он может перенаправить его в соответствующий сервис. При этом `rewrite-target` изменяет путь запроса, чтобы он соответствовал маршрутам внутри приложения.

Перезапись проходит по такому шаблону - `replace(path, rewrite-target)`

Пример использования. `replace("/path","/")` :
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        pathType: Prefix
        backend:
          service:
            name: test-ingress
	        port:
	          number: 8282
```

`replace("/something(/|$)(.*)", "/$2")`
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  name: rewrite
  namespace: default
spec:
  rules:
  - host: rewrite.bar.com
    http:
      paths:
      - backend:
          service:
            name: http-svc
	        port:
	          number: 80
	      pathType: Prefix
          path: /something(/|$)(.*)
```

### Параметры при запуске
`--default-backend-service=<namespace>/<svc>` - указывает сервис по умолчанию. Если запрос пришел на несуществующий путь.