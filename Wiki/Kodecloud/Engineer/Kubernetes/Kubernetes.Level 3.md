# 1: Deploy Apache Web Server on Kubernetes CLuster

### Problem
```
There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below:
  
1. Create a `namespace` named as `httpd-namespace-nautilus`.    
2. Create a `deployment` named as `httpd-deployment-nautilus` under newly created namespace. For the deployment use `httpd` image with `latest` tag only and remember to mention the tag i.e `httpd:latest`, and make sure replica counts are `2`.
3. Create a `service` named as `httpd-service-nautilus` under same namespace to expose the deployment, nodePort should be `30004`.
```

### Solution
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deployment-nautilus
spec:
  replicas: 2
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
      labels:
        app: apache
    spec:
      containers:
        - name: httpd
          image: httpd:latest

---

apiVersion: v1
kind: Service
metadata:
  name: httpd-service-nautilus
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
  selector:
    app: apache
```
After create manifest:
```bash
kubectl create namespace http-namespace-nautilus
kubectl apply -f values.yaml -n httpd-namespace-nautilus
```

---
# 2 Deploy Lamp Stack on Kubernetes Cluster

### Problem

### Solution
```yaml
kubectl create secret generic database \
--from-literal=MYSQL_ROOT_PASSWORD=nbtech \
--from-literal=MYSQL_DATABASE=kodekloud \
--from-literal=MYSQL_USER=nbtechsupport \
--from-literal=MYSQL_PASSWORD=nbtech \
--from-literal=MYSQL_HOST=mysql-service
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  MYSQL_ROOT_PASSWORD: password
  MYSQL_DATABASE: database
  MYSQL_USER: user
  MYSQL_PASSWORD: password
  MYSQL_HOST: $HOSTNAME

---

apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: php-config
data:
  php.ini: |
     variables_order = "EGPCS"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: lamp-wp
  name: lamp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp-wp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: lamp-wp
    spec:
      containers:
      - image: webdevops/php-apache:alpine-3-php7
        name: httpd-php-container
        resources: {}
        ports:
        - containerPort: 80
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_DATABASE
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_USER
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_HOST
        volumeMounts:
        - name: php-ini
          mountPath: /opt/docker/etc/php/php.ini
          subPath: php.ini
      - image: mysql:5.6
        name: mysql-container
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_DATABASE
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_USER
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_HOST
        ports:
        - containerPort: 3306
        resources: {}
      volumes:
      - name: php-ini
        configMap:
          name: php-config
status: {}
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: lamp-wp
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: lamp-service
spec:
  type: NodePort
  selector:
    app: lamp-wp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008

```

``` php
<?php
$dbname = getenv('MYSQL_DATABASE');
$dbuser = getenv('MYSQL_USER');
$dbpass = getenv('MYSQL_PASSWORD');
$dbhost = getenv('MYSQL_HOST');

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";
```




----

# 3

### Problem
```


---


```

### Solution
```bash

```




----

# 4

### Problem
```


---


```

### Solution
```bash

```




----

# 5

### Problem
```


---


```

### Solution


----

# 6

### Problem
```


---


```

### Solution
```bash

```




----

# 7

### Problem
```


---


```

### Solution
```bash

```




----

# 8

### Problem
```


---


```

### Solution
```bash

```




----

# 9

### Problem
```


---


```

### Solution
```bash

```




----

# 10

### Problem
```


---


```

### Solution
```bash

```




----
