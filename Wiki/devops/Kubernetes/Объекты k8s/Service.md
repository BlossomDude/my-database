Service - это объект kubernetes который помогает взаимодействовать подам в кластере, а так же конечному пользователю с подом в кластере. Он предоставляет точку доступа в к подам.

>[!info]
>DNS имя:
> `some-service.dev.svc.cluster.local`
> `some-service` - имя сервиса
> `dev` - имя неймспейса
> `svc` - тип сервис. (Поддомен)
> `cluster.local` - домен

Важно отметить что Service не привязан ни к какому узлу. Он работает в кластере. Если поду назначается Service с типом NodePort то этот под будет доступен с указанного порта на всех узлах кластера. 

##### Основные функции Service:

 **Обеспечение стабильного адреса**:
    - Поды имеют динамические IP-адреса, которые меняются при их перезапуске. Service предоставляет фиксированное DNS-имя и IP, чтобы к приложению можно было обращаться стабильно.
    
**Балансировка нагрузки**:
    - Service распределяет трафик между подами, которые соответствуют заданным меткам (labels).

**Типы объекта**:
    - **ClusterIP** (по умолчанию): Создает виртуальный ip, доступен только внутри кластера.
    - **NodePort**: Открывает доступ через определённый порт на всех узлах кластера.
    - **LoadBalancer**: Создаёт внешний балансировщик нагрузки (например, в облаках).
    - **ExternalName**: Преобразует запросы в DNS-имя внешнего сервиса.


#### NodePort
При определении в манифесте данного типа сервиса нужно указать три порта:
- `targetPort` - порт на Pod
	- если не указан то возьмется значение из `port`
- `port` - Порт на Service
	- обязательно нужно объявить `port`
- `nodePort` - порт на узле в кластере
	- По умолчанию назначается случайны в диапазоне 30000-32767 
