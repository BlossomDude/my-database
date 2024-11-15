---
Owner: Blossom Dude
Last edited time: 2024-05-21T12:41
Created time: 2024-04-17T14:56
---
==**K8s probes**== - это проверки, которые осуществляются в течении жизненного цикла PODа. Они описываются для каждого контейнера.

---

- ==**Startup probe**== - запускается сразу после старта пода и применяется для приложений, которые имеют длительную загрузку. ==Пока она не завершена, другие пробы не запускаются.==
- ==**Readiness probe**== - проверка готовности пода обрабатывать трафик. Если проверка не пройдена, под не добавляется в маршрутизацию трафика сервиса.
- ==**Liveness probe**== - проверяет, функционирует ли приложение. В случае если проверка не пройдена, то приложение перезапускается.

> [!important]  
> Readiness и liveness — независимые и запускаются после прохождения startup probe.  
  
> [!important]  
> Существуют exec-, http-, tcp- и gprc-пробы. Проверки осуществляются сервисом kubelet на ноде, где запущен целевой POD.