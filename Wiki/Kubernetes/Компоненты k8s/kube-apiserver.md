C его помощью обеспечивается работа API кластера, обрабатываются REST-операции и предоставляется интерфейс, через который остальные компоненты взаимодействуют друг с другом. Также через него проходят запросы на изменение состояния или чтение кластера. Работает на master-нодах;

Вам не обязательно использовать утилиту командой строки `kubectl` для работы с kube-apiserver. Вы можете использовать обычные POST запросы. Например:
`curl -X POST /api/v1/namespace/default/pods...`

##### Что происходит после создание объекта на примере Пода?

- После создания пода, информация обновляется в `etcd`.
- `kube-sheduler`  постоянно мониторит  `kube-apiserver` и оперативно получает информацию о новом поде без назначенной ноды. Scheduler определяет на какой ноде необходимо создать под и отправляет информацию об это  `kube-apiserver`. 
- `kube-apiserver` передает информацию об этом в `etcd` и агенту `kubelet` на соответствующей `worker node` в кластере.
- После создания пода агент передает информацию обратно `kube-apiserver` 
- `kube-apiserver` передает информацию базе данных `etcd`

##### kube-apiserver.service
По сколько апи сервер работает как демон -  у него есть свой файл systemd.
Его некоторые опции:
```
--etcd-servers=https://127.2.3.4:2379 \\
```

