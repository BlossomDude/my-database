ConfigMap - объект k8s для хранения каких-то данных, например переменных.
Пример манифеста можно посмотреть тут ([[Yaml manifest's]])

Config Map можно использовать в поде по разному:

Как volume:
```yaml
volume:
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
