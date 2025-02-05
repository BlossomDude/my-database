`Admission Control` - это специальные плагины в Kubernetes, предназначенные для перехвата запросов к серверу API Kubernetes до того момента, когда объект будет сохранен, с целью проверки подлинности и авторизации запроса.

Путь к созданию объекта:
`kubectl` -> `Authentication` -> `Authorization` -> `Admission Controllers` -> `Creating pod`

>[!important]
>Для просмотра включенных Admission Control. 
>Если [[kube-apiserver]] установлен на сервер/ВМ выполните команду:
>`kube-apiserver -h | grep enable-admission-plugins`  
>Если он работает в [[Pod]]'e просто проверьте параметры запуска Pod'а.

Некоторые из Admission Control:
- `AlwaysPullImage` - гарантирует что при создании пода, образ будет загружен.
- `DefaultStorageClass` - обеспечивает предоставления [[PersistantVolume & PVClaim|PV]] стандартного [[StorageClass]]'a, если не в PVC он не указан.
- `EventRateLimit` - ограничить частоту запросов к [[kube-apiserver]] 

- `NamespaceExist` - отклоняет запросы к несуществующим [[Namespace]]'ам
- `NamespaceAutoProvisioning` - При обращении к несуществующему [[Namespace]] - создаст его.
	Эти два контроллера доступа устарели и заменены на один `NamespaceLifecycle`
	Контроллер доступа `NamespaceLifecycle` проследит за тем, чтобы запросы к несуществующему пространству имен были отклонены и чтобы пространства имен по умолчанию, такие как default, kube-system и kube-public, не могли быть удалены.

>Для добавления Admission Control просто добавьте при запуске [[kube-apiserver]] в параметр `enable-admission-plugins` новые значения

>Для удаления Admission Control по умолчанию добавьте параметр в:
> `--disable-admission-plugins=`

## Типы Admission Controllers

>[!info]
>Сначала запрос проходит по `Validating` потом по `Mutating`

Существует два типа:

1) Mutating
	Admissionn Controller который меняет исходный запрос к Api-server.

2) Validating
	Контроль доступа который просто проверяет запрос, не внося в него изменений и отклоняя его при неуспешной валидации.

3) Validating и Mutating одновременно

Мы можем создавать свои собственные, кастомные контроли доступа. Для этого существует еще два типа:

4) MutatingAdmissionWebhook -  меняет запрос.
5) ValidateAdmissionWebhook - проверяет запрос.

## Webhook Admission Controller

- Мы можем настроить данные контроли доступа, чтобы они указывали на внешний сервер или на внутренний.
- После прохода запроса по всем встроенным Admission Controller, дело доходит до собственно созданного.
- К WebhookAC отправляется специальный объект `AdmissionReview` содержащий в себе всю информацию о запросе.
- В ответ он присылает этот же объект с полем ответа - `allowed: true/false`

Для создания своего WebhookAC нужно:
1) Развернуть свой Webhook сервер. Например на [[Go]].
	Он должен принимать mutate и validate API и отвечать JSON объектом.
2) Если сервер развернут как [[Deployment]] у нас в кластере то ему необходим [[Service]]
3) Далее нам надо создать объект k8s для настройки нашего AC.
	Пример объекта:
```
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: demo-webhook
webhooks:
  - name: webhook-server.webhook-demo.svc
    clientConfig:
      service:
        name: webhook-server
        namespace: webhook-demo
        path: "/mutate"
      caBundle: |
      ADsn*7asb76...
    rules:
      - operations: [ "CREATE" ]
        apiGroups: [""]
        apiVersions: ["v1"]
        resources: ["pods"]
    admissionReviewVersions: ["v1beta1"]
    sideEffects: None
```