kube-controller-manager - процесс, который непрерывно следит за состоянием кластера Kubernetes и работает над тем чтобы привести все к рабочему состоянию. 
- Следит за статусом объектов
- Принимает необходимые меры по исправлению ситуаций

Он состоит из следующих контроллеров.
###### node-Controller
- Следит за состоянием нод и приложениями на них
- Проверяет статус каждые 5 секунд. 
	- Если узел не доступен, то он помечается как notReady, после 40 секунд.
	- Если за 5 минут узел не был восстановлен то на нем удаляются поды и перемещаются на рабочие узлы если поды были частью Replica Set. 
```
--node-monitor-grace-period=40s
--node-monitor-period=5s
--pod-eviction-timeout=300s
```
Данные параметры можно настроить в systemd файле или в манифесте контроллера.

###### Replication Controller.
- Следит за состоянием Replica Sets.
- Обеспечивает необходимое количество подов в любое время в Replica Set

>[!important]
>Так же есть много контроллеров, такие как:
>namespace-controller, deployment-controller, daemon-set-controller и тд
>Многие объекты имеют собственные контроллеры

Вот список стандартных встроенных контроллеров:
- CSR-SIGNING - отвечает за сертификаты.
- CSR-APPROVING - отвечает за подписание сертификатов.
- Node Controller
- Replication Controller
- ReplicaSet Controller
- Deployment Controller
- StatefulSet Controller
- DaemonSet Controller
- Job Controller
- CronJob Controller
- Service Controller
- Endpoint Controller
- Namespace Controller
- Service Account Controller
- ResourceQuota Controller
- LimitRange Controller
- PersistentVolume Controller
- PersistentVolumeBinder Controller
- Horizontal Pod Autoscaler Controller
- TTL Controller
- Garbage Collector Controller
- Token Controller
- CertificateSigningRequest Controller
- Lease Controller
- Cloud Route Controller
- Cloud Node Controller
- Cloud Service Controller
- IngressClass Controller
- Volume Expansion Controller
- Volume Attachment Controller