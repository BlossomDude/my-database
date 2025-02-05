Для обращения к kube-API необходимо всегда указывать файлы сертификатов и другие опции, например:
```bash
curl https://master-node:6443/api/v1/pods \
--server ...
--client-key ...
--client-certificate...
--certificate-authority...
# И так далее...
```

Для того чтобы не указывать всегда данные параметры, можно создать `config` для удобной работы с АПИ.
Kubeconfig создается посредством создания файла манифеста объекта k8s:
```yaml
apiVersion: v1
kind: Config

current-context: kubernetes-admin@kubernetes

clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://controlplane:6443
  name: kubernetes

contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes

users:
- name: kubernetes-admin
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
```

Конфиг состоит из трех списков:
- `cluster` - указываются неймспейсы, сертификаты и т.д.
- `context` - в этом разделе собственно все перечисленные контексты `<users>@<cluster>`
- `users` - указывается пользователь, сертификаты и т.д.

Так же указывается `current-context` - который указывает на текущий контекст. Его можно поменять командой: `kubectl config use-context <context>`

Для установки нового кастомного файла можно воспользоваться экспортом переменной
`export KUBECONFIG=<path>`
