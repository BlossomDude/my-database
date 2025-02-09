Разворачивание кластера через бинарные файлы. Неудобный способ, но очень полезен для понимания.

- Предполагается что все ВМ уже установлены и готовы для настройки кластера kubernetes.

### Информация о кластере

|VM|VM Name|Purpose|IP|Forwarded Port|RAM|
|---|---|:-:|--:|--:|--:|
|controlplane01|kubernetes-ha-controlplane01|Master|192.168.56.11|2711|2048|
|controlplane02|kubernetes-ha-controlplane02|Master|192.168.56.12|2712|1024|
|node01|kubernetes-ha-node01|Worker|192.168.56.21|2721|512|
|node02|kubernetes-ha-node02|Worker|192.168.56.22|2722|1024|
|loadbalancer|kubernetes-ha-lb|LoadBalancer|192.168.56.30|2730|1024|
### Установка kubectl

Скачиваем бинарный файл, назначаем права и переместим его в стандартную директорию для бинарных файлов.

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${ARCH}/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```


### Настройка сервера PKI

Для разворачивания кластера нам нужны сертификаты. Для лабораторной работы мы создадим свой центр сертификации на controlplane01.

Создаем переменные с ip адресами хостов
```bash
export CONTROL01=$(dig +short controlplane01)
export CONTROL02=$(dig +short controlplane02)
export LOADBALANCER=$(dig +short loadbalancer)
export SERVICE_CIDR=10.96.0.0/24
export API_SERVICE=$(echo $SERVICE_CIDR | awk 'BEGIN {FS="."} ; { printf("%s.%s.%s.1", $1, $2, $3) }')
```

##### Рутовый сертификат и ключ.
```bash
openssl genrsa -out kube-controller-manager.key 2048

openssl req -new -key kube-controller-manager.key \
-subj "/CN=system:kube-controller-manager/O=system:kube-controller-manager" -out kube-controller-manager.csr

openssl x509 -req -in kube-controller-manager.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out kube-controller-manager.crt -days 1000
```

##### Сертификат клиента и сервера

> Сертификат и ключ для наших узлов

```bash
openssl genrsa -out admin.key 2048

openssl req -new -key admin.key -subj "/CN=admin/O=system:masters" -out admin.csr

openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out admin.crt -days 1000
```

##### Сертификат для kube-controller-manager
```bash
openssl genrsa -out kube-controller-manager.key 2048

openssl req -new -key kube-controller-manager.key \
-subj "/CN=system:kube-controller-manager/O=system:kube-controller-manager" -out kube-controller-manager.csr

openssl x509 -req -in kube-controller-manager.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out kube-controller-manager.crt -days 1000
```

##### Сертификат для kube proxy
```bash
openssl genrsa -out kube-proxy.key 2048

openssl req -new -key kube-proxy.key \
-subj "/CN=system:kube-proxy/O=system:node-proxier" -out kube-proxy.csr

openssl x509 -req -in kube-proxy.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out kube-proxy.crt -days 1000
```

##### Сертификат для kube-scheduler
```bash
openssl genrsa -out kube-scheduler.key 2048

openssl req -new -key kube-scheduler.key \
-subj "/CN=system:kube-scheduler/O=system:kube-scheduler" -out kube-scheduler.csr

openssl x509 -req -in kube-scheduler.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out kube-scheduler.crt -days 1000
```


##### Сертификат для kube-apiserver

>Сертификат сервера kube-apiserver требует, чтобы все имена, которые могут быть доступны различным компонентам, были частью альтернативных имен. К ним относятся различные DNS-имена и IP-адреса, такие как IP-адрес серверов controlplane, IP-адрес подсистем балансировки нагрузки, IP-адрес службы kube-api и т.д. Они обеспечивают идентификацию сертификата, который является ключевым в процессе SSL для сервера, подтверждающего его подлинность.

```bash
cat > openssl.cnf <<EOF
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[v3_req]
basicConstraints = critical, CA:FALSE
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster
DNS.5 = kubernetes.default.svc.cluster.local
IP.1 = ${API_SERVICE}
IP.2 = ${CONTROL01}
IP.3 = ${CONTROL02}
IP.4 = ${LOADBALANCER}
IP.5 = 127.0.0.1
EOF
```

```bash
openssl genrsa -out kube-apiserver.key 2048

openssl req -new -key kube-apiserver.key \
-subj "/CN=kube-apiserver/O=Kubernetes" -out kube-apiserver.csr -config openssl.cnf

openssl x509 -req -in kube-apiserver.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out kube-apiserver.crt -extensions v3_req -extfile openssl.cnf -days 1000
```


##### Сертификат клиента API-сервера Kubelet

>Этот сертификат предназначен для проверки подлинности сервера API с помощью kubelets, когда он запрашивает информацию

```bash
cat > openssl-kubelet.cnf <<EOF
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[v3_req]
basicConstraints = critical, CA:FALSE
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth
EOF
```

```bash
openssl genrsa -out apiserver-kubelet-client.key 2048

openssl req -new -key apiserver-kubelet-client.key \
-subj "/CN=kube-apiserver-kubelet-client/O=system:masters" -out apiserver-kubelet-client.csr -config openssl-kubelet.cnf

openssl x509 -req -in apiserver-kubelet-client.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out apiserver-kubelet-client.crt -extensions v3_req -extfile openssl-kubelet.cnf -days 1000
```

##### Cертификат для ETCD

>Аналогично, сертификат сервера ETCD должен содержать адреса всех серверов, входящих в кластер ETCD.

```bash
cat > openssl-etcd.cnf <<EOF
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
[alt_names]
IP.1 = ${CONTROL01}
IP.2 = ${CONTROL02}
IP.3 = 127.0.0.1
EOF
```

```bash
openssl genrsa -out etcd-server.key 2048

openssl req -new -key etcd-server.key \
-subj "/CN=etcd-server/O=Kubernetes" -out etcd-server.csr -config openssl-etcd.cnf

openssl x509 -req -in etcd-server.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out etcd-server.crt -extensions v3_req -extfile openssl-etcd.cnf -days 1000
```

##### Сертификат для ServiceAccount
```bash
openssl genrsa -out service-account.key 2048

openssl req -new -key service-account.key \
-subj "/CN=service-accounts/O=Kubernetes" -out service-account.csr

openssl x509 -req -in service-account.csr \
-CA ca.crt -CAkey ca.key -CAcreateserial -out service-account.crt -days 1000
```


### Создание конфигурационных файлов Kubernetes для аутентификации

##### Публичный IP-адрес Kubernetes

>Для подключения каждому kubeconfig требуется сервер API Kubernetes. Для поддержки высокой доступности будет использоваться IP-адрес, назначенный подсистеме балансировки нагрузки, поэтому давайте сначала введем адрес подсистемы балансировки нагрузки в переменную оболочки, чтобы мы могли использовать его в настройках kubeconfigs для служб, которые выполняются на рабочих узлах. Диспетчеру контроллера и планировщику необходимо взаимодействовать с локальным API-сервером, поэтому они используют адрес localhost.

```bash
export LOADBALANCER=$(dig +short loadbalancer)
```

##### Конфигурационный файл kube-proxy для Kubernetes

```bash
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=/var/lib/kubernetes/pki/ca.crt \
    --server=https://${LOADBALANCER}:6443 \
    --kubeconfig=kube-proxy.kubeconfig

kubectl config set-credentials system:kube-proxy \
    --client-certificate=/var/lib/kubernetes/pki/kube-proxy.crt \
    --client-key=/var/lib/kubernetes/pki/kube-proxy.key \
    --kubeconfig=kube-proxy.kubeconfig

kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-proxy \
    --kubeconfig=kube-proxy.kubeconfig

kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
```


##### Конфигурационный файл kube-controller-manager для Kubernetes
```bash
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=/var/lib/kubernetes/pki/ca.crt \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-controller-manager.kubeconfig

kubectl config set-credentials system:kube-controller-manager \
    --client-certificate=/var/lib/kubernetes/pki/kube-controller-manager.crt \
    --client-key=/var/lib/kubernetes/pki/kube-controller-manager.key \
    --kubeconfig=kube-controller-manager.kubeconfig

kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-controller-manager \
    --kubeconfig=kube-controller-manager.kubeconfig

kubectl config use-context default --kubeconfig=kube-controller-manager.kubeconfig
```

##### Конфигурационный файл kube-scheduler Kubernetes
```bash
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=/var/lib/kubernetes/pki/ca.crt \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-scheduler.kubeconfig

kubectl config set-credentials system:kube-scheduler \
    --client-certificate=/var/lib/kubernetes/pki/kube-scheduler.crt \
    --client-key=/var/lib/kubernetes/pki/kube-scheduler.key \
    --kubeconfig=kube-scheduler.kubeconfig

kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-scheduler \
    --kubeconfig=kube-scheduler.kubeconfig

kubectl config use-context default --kubeconfig=kube-scheduler.kubeconfig
```

##### Файл конфигурации администратора Kubernetes
```bash
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=admin.kubeconfig

kubectl config set-credentials admin \
    --client-certificate=admin.crt \
    --client-key=admin.key \
    --embed-certs=true \
    --kubeconfig=admin.kubeconfig

kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=admin \
    --kubeconfig=admin.kubeconfig

kubectl config use-context default --kubeconfig=admin.kubeconfig
```

##### Копируем файлы конфигурации на узлы
```bash
for instance in node01 node02; do
  scp kube-proxy.kubeconfig ${instance}:~/
done

for instance in controlplane01 controlplane02; do
  scp admin.kubeconfig kube-controller-manager.kubeconfig kube-scheduler.kubeconfig ${instance}:~/
done
```