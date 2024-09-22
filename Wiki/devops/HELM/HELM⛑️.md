---
Owner: Blossom Dude
Last edited time: 2024-06-24T17:31
Created time: 2024-06-10T16:05
---

> Объеденяет объекты k8s в один целый пакет и позволяет управлять, обновлять, откатывать им как единым целым.  

- [[#Структура чарта|Структура чарта]]
- [[#Шаблонизация|Шаблонизация]]
- [[#Проверка чарта|Проверка чарта]]
- [[#Функции|Функции]]
- [[#Конвеер|Конвеер]]
- [[#Условные выражения|Условные выражения]]
- [[#Переменные|Переменные]]
- [[#Блок Range|Блок Range]]
- [[#Именованные шаблоны|Именованные шаблоны]]
- [[#Хуки|Хуки]]
- [[#Зависимости|Зависимости]]
- [[#Тесты чарта|Тесты чарта]]
- [[#Best practies|Best practies]]

[[Wiki/devops/HELM/Команды|Команды]]

### Структура чарта

- 🪖 **Чарт** - набор файлов, содержат в себе все инструкции, благодаря которым Helm все устанавливает. Содержит в себе:
    - 📁 **Templates -** файлы с объектами k8s, например деплоймент и сервис
        - `*.tpl` файлы с кусками кода для переиспользования
    - 📁 **Charts** - в нем могут находится другие чарты от которых зависит текущий чарт.
    - 📁 crds - Глобальные ресурсы кластера
    - 📃 **Chart.yaml** - содержит информацию о чарте, в нем содержится версия api и версия приложения, так же метаданные, имя чарта и описание.
    - 📃 **values.yaml** - нужна для пользовательских данных и шаблонизации

### Шаблонизация

  

`{{ .Release.Name }}` - подставит имя релиза

`{{ .Release.NameSpace }}`

`{{ .Release.IsUpgarde }}`

`{{ .Release.IsInstall }}`

`{{ .Release.Revision }}`

`{{ .Release.Service }}`

  

Обращение к ==**Chart.yaml**==  
  
`{{ .Chart.APIVersion }}`

`{{ .Chart.Name }}`

`{{ .Chart.KubeVersion }}`

`{{ .Chart.Desciption }}`

`{{ .Chart.Type }}`

`{{ .Chart.Keywords }}`

`{{ .Chart.Home }}`  
  

Обращение к Template  
  
`{{ .Template.Name }}` -

`{{ .Template.BasePath }}` -  
  

Обращение к свойствам values.yaml ==**(Значения чувствительны к регистру)**==

`{{ .Values.replicaCount }}` -

`{{ .Values.image }}` -

Сведения о кластере k8s

`{{ .Capabilities.HelmVersion }}`

`{{ .Capabilities.KubeVersion }}`

`{{ .Capabilities.APIVeresions }}`

  

### Проверка чарта

Для проверки есть три простых шага:

- `helm lint .`
- `helm template . --debug` - выведет сгенерированные объекты и укажет на ошибки
- `helm install *release_name* --dry-run` - проверочная установка чтобы выявить ошибки

  

### Функции

  

[https://helm.sh/docs/chart_template_guide/function_list/](https://helm.sh/docs/chart_template_guide/function_list/) - все функции.

> [!important]  
> Пример:{{ upper .Values.image.repository }} → image: NGINXquote → image: “nginx”replace “x” “y” → image: nginyshuffle → image: xngni  

  

`{{` ==`default`== `“IfNotPresent” .Value.image.pullPolicy }}` - подставит значение по умолчанию если в values.yaml значения такого нет.

  

### Конвеер

  

Передача функций конвеером:

`{{ .Values.image.repository |` ==`upper`== `|` ==`quote`== `|` ==`etc.`== `}}` - `image: “NGINX”`

  

### Условные выражения

  

```YAML
labels:
{{- if .Values.deptLAbel }}
  dept: {{ .Values.deptLabel }}
{{- else if eq .Values.deptLabel "rd" }}
  dept: "Research and Development"
{{- else }}
  dept: "unknown"
{{- end }}
```

|   |   |
|---|---|
|==**значение**==|==**описание**==|
|eq|==|
|ne|!=|
|lt|<|
|le|<=|
|gt|>|
|ge|≥|
|and|и|
|or|или|
|not|не (инверсия)|
|empty|true если пусто|

### Переменные

> Переменные локальны, они доступны только в блоке в котором объявлены.  
> Никаких сложных математических вычислений они не поддерживают.  

Объявление переменной:

`{{- $result := false -}}`

Использование переменной:

`{{ $result }}`

  

### Блок Range

```YAML
# values.yaml
vendors:
 - Ford
 - Fiat
 - Opel
 - Lada
---
# configmap.yaml
...
data:
  vendors:
  {{- range .Values.vendors }}
    - {{ . | quote }}
  {{- end }}
```

  

### Именованные шаблоны

Пример наименования - `_helpers.tpl` находятся в директории `templates`

```YAML
{{- define "labels" }}
  app.k8s.io/name: {{ Release.Name }}
  app.k8s.io/version: {{ .Chart.AppVersion }}
{{- end }}
```

```YAML
apiVersion: v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
  labels:
    {{- template "labels" . }} # Обязательно ставим точку
                               # тк в _helpers.tpl не видно переменных
spec:
  replicas: 2
  selector:
    matchLabels:
      {{- include "labels" . | indent 2 }}
    template:
      metadata:
        labels:
          {{- include "labels" . | indent 4 }}
...          
```

Важные моменты:

- Обязательно ставить точку после наименования шаблона
- В случаях если отступов нужно больше чем есть в файле `tpl` необходимо указать функцию как в примере и кол-во пробелов. А так же заменить `template` на `include`. Но я буду использовать всегда include, так как ничего не изменится и так проще запомнить.

  

### Хуки

Есть 4 действия перед которыми выполняются хуки.

- pre/post-install
- pre/post-delete
- pre/post-upgarde
- pre/post-rollback

```undefined
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ .Release.Name }}-upgrade-backup
  annotation:
    "helm.sh/hook": pre-upgrade
    "helm.sh/hook-weight": "5"
    "helm.sh/hook-delete-policy": hook-succeeded
spec:
  selector:
    matchLabels:
      app.kubernetes.io/part-of: simple-app
  template:
    metadata:
      name: {{ .Release.Name }}-upgrade-backup
      labels:
        app.kubernetes.io/part-of: simple-app
    spec:
      containers:
      - name: upgrade-backup
        image: alpine
        command: [*opt/backup.sh]
```

`"helm.sh/hook": pre-upgrade` - обозначение когда джоба должна сработать

`"helm.sh/hook-weight": "5"` - Вес хука, в порядке по возрастанию будут исполнятся хуки

`"helm.sh/hook-delete-policy": hook-succeeded` - удаляет ресурс ==**(в данном примере Бэкап)**== после успешной отработки, и сохранит если не успешно.

- `hook failed` - удаляет ресурс если выполнение не удачно
- `before-hook-creation` (по ум.) - удаляет предыдущий ресурс перед запуском нового хука

### Зависимости

```YAML
...
dependencies:

- name: maridb
  repository: https://...
  version: ~11.3              # Позволяет использовать любую версию от 11.3.0 до 11.4
  condition: mariadb.enabled  # ЗАвисимость будет использована если только его принудительно включат в values.yaml
```

После того как мы добавили зависимости в `Chart.yaml`, нужно их загрузить командой  
  
`helm dependency update,` после создаться файл `template\Chart.lock` и сам чарт в `charts\*chart_name*`

  

### Тесты чарта

Тестировать чарт можно с помощью добавления файла (например) `templates/tests/test-app.yaml,` добавим туда тесты им вызовем его с помощью аннотации.

Пример тест файла:

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: {{ .Release.Name }}-app-test
  labels:
    {{- include "simple-app.labels" . | 2 }}
    
  annotations:
    "helm.sh/hook": test
    
spec:
  containers:
  - name: upgrade-backup
    image: alpine
    command: ["wget"]
    args: ['{{ .Release.Name }}-svc:80']
  restartPolicy: Never
```

  

### Best practies

`Values.yaml`:

- Используй **camelCase** в названии переменных
- Используй вложеннные переменные.
    
    ```YAML
    image:
      repo: nginx
      pullPolicy: ifNotPresent
      ...
    ```
    
- При работе с большими числами в values.yaml бери их в ковычки. А потом уже используй функцию `int` для преобразования в число.
- Документируй `values.yaml`

  

`k8s obj`:

- используй стандартные имена объектов k8s. deployment.yaml, service.yaml и тд
- Документируй объекты кубер, создав `NOTES.txt` в директории templates.

  

`_helper.tpl` :

- обязательно документировать файл, так как очень сложно понять что там происходит.