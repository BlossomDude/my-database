Самый малый объект kubernetes.

Помните, что вы НЕ МОЖЕТЕ редактировать характеристики существующего POD, кроме приведённых ниже:

- `spec.containers[*].image`
- `spec.initContainers[*].image`
- `spec.activeDeadlineSeconds`
- `spec.tolerations`

Но можно его отредактировать если он был запущен в Deployment.

В Поде может существовать несколько контейнеров:
```yaml
containers:
  - name: nginx
    image: nginx
    
  - name: agent-monitoring
    image: prometheus_agent:latest
```

Так же бывает когда нам нужен такой контейнер который будет выполнять какоето свое действие при запуске пода. Их называют init container:
```
containers:
  - name: nginx
    image: nginx
initContainers:
  - name: init-service
    image: busybox
    command: [...]

```

