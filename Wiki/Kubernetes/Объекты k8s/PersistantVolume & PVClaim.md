# PersistantVolume

**PersistentVolume (PV)** — это объект кластера, который предоставляет способ абстрагирования физического хранилища, доступного в кластере.
- PV создаётся и управляется на уровне кластера, а не в контексте конкретного приложения или пространства имен. Это похоже на выделение дискового пространства в облаке или файлового сервера.
-  Типы хранилища:
	- Локальные диски узлов.
	- Сетевые файловые системы (NFS, GlusterFS, CephFS).
	- Облачные хранилища (AWS EBS, GCP Persistent Disk, Azure Disk, etc.).
	- Объектное хранилище (S3 через сторонние драйверы).
---
###### Пример yaml:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data
```
---
###### Спецификация PV

`capacity` - Объём выделенной памяти

`accessMode`:
- `ReadWriteOnce (RWO)`: Доступ для записи только с одного узла.
- `ReadOnlyMany (ROX)`: Доступ только для чтения с нескольких узлов.
- `ReadWriteMany (RWX)`: Доступ для записи с нескольких узлов.

`persistentVolumeReclaimPolicy`:
- `Retain`: PV остаётся после удаления PersistentVolumeClaim (PVC). (ПО УМОЛЧАНИЮ)
- `Recycle`: Примитивное очищение PV (удаление файлов).
- `Delete`: PV и связанные ресурсы (например, диск в облаке) удаляются.

---
# PersistentVolumeClaim

**PersistentVolumeClaim (PVC)** в Kubernetes — это объект, который позволяет пользователю запрашивать хранилище с определёнными характеристиками. PVC действует как интерфейс между приложениями и физическим хранилищем, абстрагируя детали его реализации.

- PVC используется для запроса определённого объёма хранилища и нужных режимов доступа. Он указывает, какой объём пространства требуется приложению и как оно собирается взаимодействовать с хранилищем (чтение/запись).

- Kubernetes автоматически подбирает **PersistentVolume (PV)**, который соответствует требованиям PVC. Если такого PV нет, PVC остаётся в статусе _Pending_ до появления подходящего PV.

- Если PVC ссылается на **StorageClass**, Kubernetes создаёт новый PV динамически, используя параметры StorageClass.

- PVC подключается к поду как **Volume**, предоставляя приложению доступ к хранилищу.

- Типы доступа аналогичны PV
---
###### Статусы PVC:
- `Pending`: Ожидание PV, который соответствует требованиям.
- `Bound`: PVC привязан к PV.
- `Lost`: Связанный PV больше недоступен (например, удалён).
---
###### Пример yaml:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: standard
```
---
###### Привязка PVC к Поду:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      volumeMounts:
        - mountPath: "/data"
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: my-pvc
```
