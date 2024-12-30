
ETCD - В нем хранится состояние всего кластера. Главная задача etcd — обеспечить отказоустойчивость кластера и консистентность данных. Etcd — самостоятельный проект. Он развивается отдельно от Kubernetes и применяется в разных продуктах. Работает на master-нодах;

- Etcd хранит информацию об множестве разных объектах k8s.
- Вся информация которую выдает команда `kubectl get` исходит от etcd 
- etcd работает на 2379 порту.

Для запуска etcd, необходимо скачать бинарный файл.
Запустите etcd командой `./etcd`.

Данные команды для версии API 2
`etcdctl set <key> <value>` - сохранить пару ключ-значение в etcd
`etcdctl get <key>` - чтобы получить значение переменной


### Версия API 2 и 3

Версии etcd 2 и 3 отличаются друг от друга поэтому команды `etcdctl` у двух версий разные.
По умолчанию он настроен на использование версии API 2.

`etcdctl --version` - узнать версию etcd и API

Версию API можно так же поменять создав переменную:
`export ETCD_API=3` или поставить значение на 2

Кроме того, для работы с версией 3 вы также должны указать путь к файлам сертификатов, чтобы ETCDCTL мог пройти аутентификацию на сервере ETCD API. Файлы сертификатов доступны в etcd-master по следующему пути.
``` bash
--cacert /etc/kubernetes/pki/etcd/ca.crt
--cert /etc/kubernetes/pki/etcd/server.crt
--key /etc/kubernetes/pki/etcd/server.key
```

Итого для работы `etcdctl` с версией API 3 необходимо ввести данную команду
``` bash
kubectl exec etcd-controlplane -n kube-system \ 
     -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key"
```
