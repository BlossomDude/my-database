## ConfigMap 
Это объект k8s для хранения каких-то данных, например переменных.
Пример манифеста можно посмотреть тут ([[Yaml manifest's]])

Config Map можно использовать в поде по разному:

Как volume:
```yaml
volumes:
  - name: my-cmap-volume
    configMap:
      name: my-cmap
```

Взять все переменные:
```yaml
envFrom:
  - configMapRef:
	name: my-cmap
```

Взять одну переменную:
```yaml
- env:
  - name: APP_COLOR
	valueFrom:
	  configMapKeyRef:
		key: APP_COLOR
		name: app_config   # Имя config-map откуда берем переменную
```

>[!important]
>ConfigMap указывается в `spec`'e пода

Для того чтобы взять из ConfigMap файл, необходимо указать блок volume так:
```yaml
volume:
  - name: volume_name
    configMap:
      name: cm_name
      items:
        - key: index.php      # Имя файла из CM 
          path: index.php     # Путь в который будет скопированы данные

---

volumeMount:
  - mountPath: /app/index.php # Указываем полный путь
    name: volume_name 
    subPath: index.php        # Указываем файл который передаем
```

## Secret

`Secret` - по сути тот же Config Map, только предназначен для другого.
У них идентичный манифест файлы. Секрет используется для чувствительных данных, такие как пароли, токены и тд.

Есть несколько типов Secrets, по умолчанию - `opaque` - он же `generic`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
  DB_PASS: pa$$word
  TOKEN: HU&*%GY#(SJA
```

Не рекомендовано хранить в секрете пароль в открытом виде. Лучше закодировать пароль в base64:
`echo -n "pa$$word" | base64`
Можно раскодировать добавив опцию:
echo -n "6.s8&hd%"

Для передачи значения из Secret или весь Secret:

Включить весь secret:
```yaml
envFrom:
  - secretRef:
	name: my-secret
```

Взять одну или несколько переменных секретов:
```yaml
- env:
  - name: DB_PASS
	valueFrom:
	  configMapKeyRef:
		name: my-secret
		key: DB_PASS
```

Как volume:
```yaml
volume:
  - name: my-cmap-volume
    secret:
      secretName: my-cmap
```

В данном случае каждый секрет должен иметь свой файл в директории volume. Имя файла = имя секрета, содержимое = сам секрет.

- Секреты только кодируются, но не шифруются
- Проверяйте что вы не пушите секреты в SCM
- Secret не шифруется в etcd, поэтому используйте шифрования диска.
- Любой у кого есть права на создание подов и деплойментов есть права на просмотр секретов

Для безопасности:
- настройте ролевую модель
- Для хранения секретов не в etcd, используйте сторонние решения для хранения секретов 

> [! important]
> Для шифрования секретов etcd можно настроить создать и настроить объект `EncryptionConfiguration`

