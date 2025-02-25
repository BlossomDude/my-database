---
Owner: Blossom Dude
Last edited time: 2024-05-26T16:36
Created time: 2023-11-15T14:58
---

  > [!important]  
> Бесплатное окружение на 4 часа для обучения k8s: https://labs.play-with-k8s.com  
  
### kube-controller-manager
>[!info]
>Подробнее: [[kube-controlller]]


`kube-controller-manager` - процесс, который непрерывно следит за состоянием кластера Kubernetes и работает над тем чтобы привести все к рабочему состоянию. 
- Следит за статусом объектов
- Принимает необходимые меры по исправлению ситуаций

---
### etcd
>[!info]
>Подробнее: [[etcd]]

- `ETCD` - В нем хранится состояние всего кластера. Главная задача etcd — обеспечить отказоустойчивость кластера и консистентность данных. Etcd — самостоятельный проект. Он развивается отдельно от Kubernetes и применяется в разных продуктах. Работает на master-нодах;

---
### kube-sheduler

>[!info]
>Подробнее: [[kube-scheduler]]

`kube-sheduler` - Компонент k8s, планирующий, на каких узлах разворачивать поды. Он учитывает такие факторы, как ограничения, требования к ресурсам, местонахождение данных и пр. Работает на master-нодах;

---
### kube-apiserver

>[!info]
>Подробнее: [[kube-apiserver]]

`kube-apiserver` - с его помощью обеспечивается работа API кластера, обрабатываются REST-операции и предоставляется интерфейс, через который остальные компоненты взаимодействуют друг с другом. Также через него проходят запросы на изменение состояния или чтение кластера. Работает на master-нодах;

---
### kubelet

>[!info]
>Подробнее: [[kubelet]]

`kubelet` - Это агент, который работает на каждом рабочем узле в кластере Kubernetes. Он отвечает за управление жизненным циклом подов на рабочем узле. Kubelet следит за состоянием подов, запускает и останавливает их, проверяет их здоровье, а также обеспечивает связь с мастер-узлом для получения инструкций и обновлений.

---
### kube-proxy

>[!info]
>Подробнее: [[kube-proxy]]

`kube-proxy` - Служба, которая управляет правилами балансировки нагрузки. Она конфигурирует правила IPVS или iptables, через которые выполняются проксирование и роутинг. Обеспечивает связь между сервисами внутри кластера. Работает на worker-нодах;

---
### dockershim (depricated)

> [!Info] 
> Поддержка docker (docker-shim) была прекращена с версии Kubernetes v1.24
> 
> Однако все равно есть возможность используя CRI-Dockerd самому настроить crictl для работы с docker.
> 

- `docker-shim` - интерфейс для работы docker с k8s.

- Docker имеет такое отличие т.к. в него входит много модулей которые его делают не просто container runtime.  Он содержит в себе множество инструментов такие как инструменты сборки, CLI, API, AUTH, SECURITY и containerd который сам по себе является CRE и может работать с k8s через CRI.



