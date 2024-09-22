---
Owner: Blossom Dude
Last edited time: 2023-12-01T17:56
Created time: 2023-12-01T17:54
---
[Сравнение всех Ingress Controller](https://docs.google.com/spreadsheets/d/191WWNpjJ2za6-nbG4ZoUMXMpUK8KlCIosvQB0f-oq3k/edit#gid=907731238)

### ==Ingress Controller==

- Является pod’ом, на котором запущен ==контейнер-приложение.== Это приложение решает кому будет пренадлежать текущий трафик.
- В аналогию можно привести маршрутизатор со встроеным ==PAT==.

![[/Untitled 7.png|Untitled 7.png]]

IС наглядно. 1 ip для 3-ех приложений

```YAML
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: ingress-hosts

spec:
	rules:
		- host: www.adv-it.net       # Если обратятся на это имя
			http:
				paths:
					- backend:
							serviceName: main  # перекинет на этот сервис
							servicePort: 80    # и порт
		
		- host: web1.adv-it.net
			http:
				paths:
					- backend:
							serviceName: web1
							servicePort: 80
		
		- host: web1.adv-it.net
			http:
				paths:
					- backend:
							serviceName: web1
							servicePort: 80
```