Объект namespace нужен для разграничения областей в рамках одного кластера.

`kube-system` - административный namespace для системных объектов k8s 
`kube-public` - namespace который доступен всем.
`default` - дефолтный домен по умолчанию


Для указания namespace в манифесте, его необходимо прописать в блоке metadata:
``` yaml
apiVersion: v1
kind: Pod
metadata: 
  name: my-pod
  namespace: dev
...
```

Если хотите указать namespace при  запуске какой либо команды, будь то удаление, создание или просто просмотр существующих объектов, необходимо указать параметр командной строки:
`kubectl get pods --namespace=dev`