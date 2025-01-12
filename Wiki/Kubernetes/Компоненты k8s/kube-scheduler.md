
- [[#Описание]]
- 
---
### Описание
---
kube-sheduler - Компонент k8s, планирующий, на каких узлах разворачивать поды. Он учитывает такие факторы, как ограничения, требования к ресурсам, местонахождение данных и пр. Работает на master-нодах;

```
# Файл с конфигурацие пода планировщика.
/etc/kubernetes/manifests/kube-scheduler.yaml
```

> [!important] Он только принимает решения куда будет установлен под. Но не размещает поды на узлах, это работа kubelet.

---
### Scheduling Lifecycle
- Shceduling Queue - очередь на планировку.
В очереди находятся поды которых создали. По умолчанию никакого приоритета нет, они идут в порядке очереди. Но если создать объект типа PriorityClass и присвоить значение поду в `spec.priorityClassname` то можно таким образом настроить очередность на планировку.

- Filtering - фильтрация.
На данном этапе фильтруются узлы и отбрасываются если на них нельзя размещать данный под, например если он требует слишком много ресурсов.

- Scoring - Оценка узлов
Тут происходит оценка узлов, планировщик смотрит на узлы и дает определенную оценку каждому узлу и выбирает наилучший.

- Binding - Связывание
На данном этапе под и узел связываются.



---
### Выборка нод
---
В каждом файле манифеста пода есть неявная директива `spec.nodeName` в которую можно указать узел в котором будет развернут под.
- Если узел не найден то под будет в состоянии `Pending`
- Узел нельзя поменять при уже работающем контейнере
- Если узел не указан явно то kube-scheduler назначит его автоматически используя механизмы afinity, taints, tolerations и учитывать оценку узлов.

Как scheduler ставит оценку узлам:
1) Сначала он фильтрует узлы. Например нашему под нужно 2 cpu. Значит ноды со свободными cpu<2 не подходят.
2) Далее идет ранжировка узлов. Kube-scheduler дает оценку нодам от 1 до 10, используя функцию приоритета.
3) После получения оценки он выбирает лучший узел.
#### Варианты назначения узла:
- Оценка узла. Назначение узла исходя из его оценки планировщиком.
- `spec.tolerations`. Назначение исходя из [[Taints & Tolerations]].
- `spec.nodeName`. Строгое назначение используя явный выбор узла.
- `spec.nodeSelector`. Строгое назначение по лейблам  
- `spec.affinity` .

#### Перепривязка работающего пода к другому узлу  
Чтобы перепривязать под к другому узлу во время его работы необходимо использовать binding:
```yaml
apiVersion: v1
kind: Binding
metadata:
  name: some-name
target:
  apiVersion: v1
  kind: Node
  name: node_name
```
Далее необходимо конвертировать этот файл в json формат и отправить Post запрос имитируя настоящий `kube-scheduler`:
``` bash
curl --header "Content-Type:application/json" \
     --request POST \
     --data $(kubectl apply -f values.yaml --dry-run=client -o json) \
     http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding

```

---
#### Multiple Schedulers

При запуске сервиса `kube-scheduler`, указывается параметр который указывает файл с конфигурацией планировщика.
`--config=/etc/kuberenetes/config/kube-scheduler.yaml`
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: my-scheduler
leaderElection:   
  leaderElect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

Опция `leaderElection` нужна для выбора лидера если у вас несколько мастер узлов, на которых работаю планировщики. Если `leaderElect=true` тогда данные планировщик является главным, и он будет работать пока не выйдет из строя. Тогда его место займет другой scheduler. 

Пример настройки планировщика как Pod с несколькими планировщиками:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: custom-kube-scheduler
spec:
  containers:
    - name: scheduler
      image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
      command:
        - kube-scheduler
        - --address=127.0.0.1
        - --kubeconfig=/etc/kubernetes/scheduler.conf
        - --config=/etc/kubernetes/my-scheduler-config.yaml
        ...
```

Для того чтобы определить каким планировщиком будет задеплоен определенный Pod нужно указать под с планировщиком  явно в его спецификации:
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-con
      image: nginx
  schedulerName: my-custom-scheduler-pod  
```

Чтобы посмотреть какой планировщик был использован при деплое можно использовать команды:
`kubectl get events -o wide`
`kubectl logs pods scheduler-pod`

#### Multi-Profiles Scheduler

В каждом этапе есть свои `extension points` в которые можно подключать или отключать разные плагины.

Kubernetes позволяет использовать несколько профилей планировщиков в одном поде планировщика.
```
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: my-scheduler-1
    plugins:
      score:
        disabled:                   # Отключаем TaintToleration
          - name: TaintToleration
        enabled:
          - name: MyCustomPlugin    # Подключаем два своих плагина
          - name: MyOtherPlugin

  - schedulerName: my-scheduler-2
    pligins:
      score:                        # Отключаем все плагины score и preScore
        disabled:
          - name: '*'
      preScore:
        disabled:
          - name: '*'

  - schedulerName: my-scheduler-3
```

Таблица соответствия extension point и плагинов 

|                  | extension points | plugins                                                                                           |
| ---------------- | ---------------- | ------------------------------------------------------------------------------------------------- |
| Scheduling Queue | queueSort        | prioritySort                                                                                      |
| Filtering        | preFilter        | nodeResourcesFit<br>nodePorts                                                                     |
|                  | filter           | nodeResourcesFit<br>nodeName<br>nodeUnschedulable<br>taintToleration<br>nodePorts<br>nodeAffinity |
|                  | postFilter       |                                                                                                   |
| Scoring          | preScore         | taintToleration                                                                                   |
|                  | score            | nodeResourcesFit<br>imageLocality<br>taintToleration<br>nodeAffinity                              |
|                  | reserve          |                                                                                                   |
| Binding          | permit           |                                                                                                   |
|                  | preBind          |                                                                                                   |
|                  | bind             | defaultBinder                                                                                     |
|                  | postBind         |                                                                                                   |
