Возможности:
- Автоматическое развёртывание приложение в мульти-кластере
- Отслеживание приложений по API
- поддержка SSO
- Поддержка Webhooks
- Откат до любого коммита в гит
- Web Ui
- Автоматическое определение изменения конфигурации
- Встроенные метрики prometheus
- Поддержка инструментов конфигурации HELM и тд 
- Поддержка canary, blue/green deployment и тд
- Поддержка многопользовательской среды и возможность создавать свои политики безопасности (RBAC)
- CLI и поддержка токенов доступа
- Анализ состояния приложений
- Автоматическая или ручная конфигурация приложения до нужного состояния


Команды
`argocd`
`app`
	`create app_name` - Создать приложение с именем
		`--repo` - из какой репы брать
		`--path` - место назначение приложения
		`--dest-namespace` - в какой неймспейс использовать
		`--dest-server` - сервер ArgoCD 
	`list` - вывести список приложений
	`sync` - синхронизировать приложение 
`project`
	`get proj_name` - получить информацию о проекте
		`-o yaml` - получить в формате ямл
	`list` - вывести список проектов
	`create`



You can access the `Gitea` server by using the following credentials.  
  
    Username: `bob`  
  
    Password: `bob@123`  
  
  
You can access the `ArgoCD UI` and `ArgoCD CLI` by using the following credentials.  
  
    User: `admin`  
  
    Password: `admin123`