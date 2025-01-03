Requests & Limits - это параметры управления ресурсами в Kubernetes

>[!info]
>По умолчанию контейнер не имеет ограничения на потребляемые ресурсы.

`Request` - Минимальное количество ресурсов требуемое для контейнера.
`Limits` - Максимальное количество ресурсов которое можно выделить для контейнера.

Пример использования:
``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      resources:
        requests:
          memory: "1Gi"
          cpu: 1
        limits:
          memory: "2Gi"
          cpu: 2
```

Способы указания ресурсов:
`CPU`:
- `0.1` - одна десятая CPU
- `300m` - 0.3 CPU - 300 милли cpu. `1m` = 0.001 CPU
`Memory`:
- `"256 Mi"` - 256 Мебибайта
- `"256 Ki"` - Кибибайт
- `256 Gi` - Гибибайт
- `"256M"` - 256 Мегабайта
- `"256G"` - Гигабайт
- `"256K"` - килобайт
-  `256` - 256 байта

##### Превышение лимита:
За превышением следит kubelet. Если превышается:
- **CPU:** Контейнер "душится" (throttling) и не может использовать больше CPU, чем указано в лимите.
- **Memory:** Если контейнер превышает лимит памяти, он завершится с ошибкой **OOMKilled** (Out of Memory).

##### Какую лучше тактику использовать:

Для CPU лучше использовать Requests без Limits:
Так как в этом случае гарантированно всем контейнерам достанется cpu.

Для памяти надо отталкивать от обстоятельств. Идеального решения нет. Если не ограничить контейнер в памяти, то он может потреблять огромное количество оперативки. Но в то же время если его ограничить то он может завершать с ошибкой OOM.


##### LimitRange

- Указывает дефолтные настройки для контейнеров в которых не прописан requests & limits
- Не дает превысить requests & limits. Т.е. запросить для одного контейнера cpu или mem больше чем указано в LimitRange нельзя. 

```
apiVersion: v1
kind: LimitRange
metadata:
  name: example-limitrange
  namespace: default  # Указываем namespace, в котором будет применяться LimitRange
spec:
  limits:
  - max:
      cpu: "2"        # Максимум 2 ядра CPU для контейнера
      memory: "1Gi"    # Максимум 1 ГБ памяти для контейнера
    min:
      cpu: "0.1"      # Минимум 0.1 ядра CPU для контейнера
      memory: "128Mi"  # Минимум 128 MiB памяти для контейнера
    default:
      cpu: "0.5"       # По умолчанию контейнер получает 0.5 ядра CPU
      memory: "256Mi"  # По умолчанию контейнер получает 256 MiB памяти
    defaultRequest:
      cpu: "0.2"       # Если контейнер не указывает requests, по ум. будет 0.2 CPU
      memory: "200Mi"  # По умолчанию контейнер получит 200 MiB памяти
    type: Container    # Тип ресурса: контейнер
```

##### ResourceQuota
Для ограничения СУММАРНО лимитов и реквестов в неймспейсе, а так же ограничение кол-ва подов и других объектов. Используйте объект ResourceQuota:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-rq
  namespace: default
spec:
  hard:
    requests.cpu: 4
    requests.memory: 4Gi
    limits.cpu: 10
    limits.memory. 10Gi
    pods: "10"
    services: "10"
```


Важно понимать что при конфигурации выше, можно создать в неймспейсе:
- 10 сервисов 
- 10 контейнеров с конфигурацией в среднем 
	- `requests.cpu` - 0.4
	- `requests.memory` - 400Mi
	- `limits.cpu` - 1
	- `limits.memory` - 1Gi 

Какие-то контейнеры могут запрашивать больше или меньше, но СУММАРНО вы не можете превышать данные значения в неймспейсе.