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

```bash
kubectl cp /tmp/index.php lamp-wp-4jndi88h:/app -c httpd-php-container
```


----

# 3

### Problem
```
There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.   

1. Create a `Deployment` named as `ic-deploy-datacenter`.           
2. Configure `spec` as replicas should be `1`, labels `app` should be `ic-datacenter`, template's metadata lables `app` should be the same `ic-datacenter`.
3. The `initContainers` should be named as `ic-msg-datacenter`, use image `debian`, preferably with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'echo Init Done - Welcome to xFusionCorp Industries > /ic/beta'`. The volume mount should be named as `ic-volume-datacenter` and mount path should be `/ic`.      
4. Main container should be named as `ic-main-datacenter`, use image `debian`, preferably with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'while true; do cat /ic/beta; sleep 5; done'`. The volume mount should be named as `ic-volume-datacenter` and mount path should be `/ic`.  
5. Volume to be named as `ic-volume-datacenter` and it should be an emptyDir type.
```

### Solution
```bash
k create deployment --image debian ic-deploy-datacenter --dry-run=client -o yaml
vi deploy.yaml
# Edit deploy.yaml
k aplly -f deploy.yaml

k logs <container> -f
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: ic-datacenter
  name: ic-deploy-datacenter
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-datacenter
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ic-datacenter
    spec:
      containers:
      - image: debian
        name: main-volume-datacenter
        volumeMounts:
        - name: ic-volume-datacenter
          mountPath: /ic
        command: ["/bin/bash", "-c"]
        args: ["while true; do cat /ic/beta; sleep 5; done"]
      initContainers:
      - name: ic-msg-datacenter
        image: debian
        volumeMounts:
        - name: ic-volume-datacenter
          mountPath: /ic
        command: ["/bin/bash", "-c"]
        args: ["echo Init Done - Welcome to xFusionCorp Industries > /ic/beta"]
      volumes:
        - name: ic-volume-datacenter
          emptyDir: {}
```


----

# 4

### Problem
```
The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

  
1. Create a `PersistentVolume` named as `pv-xfusion`. Configure the `spec` as storage class should be `manual`, set capacity to `3Gi`, set access mode to `ReadWriteOnce`, volume type should be `hostPath` and set path to `/mnt/dba` (this directory is already created, you might not be able to access it directly, so you need not to worry about it).
    
2. Create a `PersistentVolumeClaim` named as `pvc-xfusion`. Configure the `spec` as storage class should be `manual`, request `3Gi` of the storage, set access mode to `ReadWriteOnce`.
    
3. Create a `pod` named as `pod-xfusion`, mount the persistent volume you created with claim name `pvc-xfusion` at document root of the web server, the container within the pod should be named as `container-xfusion` using image `httpd` with `latest` tag only (remember to mention the tag i.e `httpd:latest`).
    
4. Create a node port type service named `web-xfusion` using node port `30008` to expose the web server running within the pod.
```

### Solution
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-xfusion
spec:
  storageClassName: manual
  capacity: 
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/dba
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-xfusion
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-xfusion
  labels:
    app: httpd
spec:
  containers:
  - image: httpd:latest
    name: container-xfusion
    volumeMounts:
    - mountPath: /var/www/html
      name: pvc
  volumes:
    - name: pvc
      persistentVolumeClaim:
        claimName: pvc-xfusion

---
apiVersion: v1
kind: Service
metadata:
  name: web-xfusion
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: httpd
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
