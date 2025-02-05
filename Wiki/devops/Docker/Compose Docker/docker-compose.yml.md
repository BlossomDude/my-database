---
Owner: Blossom Dude
Last edited time: 2024-07-09T20:51
Created time: 2023-10-26T14:36
---
Официальная документация по ==[docker-compose.yml](https://docs.docker.com/compose/compose-file/compose-file-v3/)==  
docker-compose.yml  

```YAML
version:  "3.9"
services:

vote:
	image: voting-app
	pull_policy: never
	build: 
		context: ./vote
		target: dev

	healthcheck: 
		test: ["CMD", "curl", "-f", "http://localhost"]
		interval: 15s
		timeout: 5s
		retries: 3
		start_period: 10s
	depends_on:
		redis:
			condition: service_healthy
	deploy:
	  replicas: 2

	volumes:
	- type: bind
	  source: ./vote
	  target: /usr/local/app

	ports:
	- published: "5000-5001"
	  target: "80"

	networks:
	- frontend
	- backend

result:
	image: result-app
	pull_policy: never
	build: ./result

	entrypoint: nodemon --inspect=0.0.0.0 server.js

	healthcheck: 
		test: ["CMD", "curl", "-f", "http://localhost"]
		interval: 15s
		timeout: 5s
		retries: 3
		start_period: 10s
	depends_on:
		db:
			condition: service_healthy 

	volumes:
	- type: bind
	  source: ./result
	  target: /usr/local/app

	ports:
	- published: "6000"
	  target: "80"
	- host_ip: 127.0.0.1
	  target: "9229"
	  published: "9229"
	networks:
	- frontend
	- dataplane

redis:
	image: redis:alpine

	healthcheck:
		test: /healthchecks/redis.sh
		interval: "5s"

	volumes:
	- type: bind
	  source: ./healthchecks
	  target: /healthchecks

	networks:
	- backend

worker:
	image: dockersamples/examplevotingapp_worker

	depends_on:
		redis:
			condition: service_healthy 
		db:
			condition: service_healthy 

	networks:
	- backend
	- dataplane

db:
	image: postgres:15-alpine

	env_file:
	- .env

	healthcheck:
		test: /healthchecks/postgres.sh
		interval: "5s"

	volumes:
	- type: bind
	  source: ./healthchecks
	  target: /healthchecks
	- type: volume
	  source: db-data
	  target: /var/lib/postgresql/data

	networks:
	- dataplane

seed:
	build: ./seed-data

	depends_on:
		vote:
			condition: service_healthy 

	profiles: ["seed"]

	restart: "no"

	networks:
	- frontend

networks:
    frontend:
        ipam:
            config:
            - subnet: 172.28.10.0/24
    backend:
        ipam:
            config:
            - subnet: 172.28.20.0/24
    dataplane:
        ipam:
            config:
            - subnet: 172.28.30.0/24

volumes:
  db-data:
```

`version: 3` - обязательно указываем версию в самом начале
`services: …` - в этом блоке будем указывать наши сервисы (контейнеры)
`any_image:` - указываем наш сервис.
`build: /app/src` - если приложение еще не собрано в образ, то можно указать где лежит код приложения, и он соберется.
`depends_on: python`- сервис any_image зависит от python, то есть, any_image будет ждать запускa python, и после этого запустится сам.
`networks:` - добавляем в сеть **frontend** данный контейнер
`- frontend`
`networks:` - объявляем две сети, frontend и backend
`frontend:`
`backend:`
`restart:` - определяет условия перезапуска контейнера.
`“no”` - по умолчанию. Не перезагружать ни при каких обстоятельствах
`always` - всегда
`on-failure` - при сбое
`unless-stopped` - только если он не остановлен вручную