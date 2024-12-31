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
# 2

### Problem
```


---


```

### Solution
```bash

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
```bash

```




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
