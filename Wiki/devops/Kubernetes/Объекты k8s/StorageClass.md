`StorageClass` — это объект, который описывает классы хранилищ, используемых в кластере, и определяет параметры, которые должны быть применены при создании **PersistentVolume (PV)** и **PersistentVolumeClaim (PVC)**. Он позволяет администраторам кластера создавать различные типы хранилищ с различными характеристиками (например, производительность, доступность, стоимость) и автоматически выбирать нужный тип хранилища для подов.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/gce-pd  # Использует Google Cloud Persistent Disk
parameters:
  type: pd-ssd  # Тип хранилища (SSD)
reclaimPolicy: Retain  # Оставить данные при удалении PVC
volumeBindingMode: WaitForFirstConsumer  # Ожидать потребителя перед выделением
```


`WaitForFirstConsumer` - Ожидает создание первого пода в котором будет указан PVC, только после этого создаться PV из StorageClass

Пример указания StorageClass в PersistantVolumeClaim:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: fast-ssd  # Указывает, что нужно использовать StorageClass 'fast-ssd'
```