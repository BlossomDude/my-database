Есть несколько типов томов которые можно примонтировать в контейнер:

`HostPath`:
Монтируем в контейнер:
```
spec:
  containers:
    - name: some-po
      volumeMounts:
        - name: log-volume
          mountPath: /dest/path
```

Определяем:
```
volumes: 
  - name: log-volume 
    hostPath: 
      path: /src/path
      type: Directory
```


Так же существует Persistent volume. Подробнее: [[PersistantVolume & PVClaim]]
