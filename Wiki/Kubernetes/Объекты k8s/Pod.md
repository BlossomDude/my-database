Самый малый объект kubernetes.

Помните, что вы НЕ МОЖЕТЕ редактировать характеристики существующего POD, кроме приведённых ниже:

- `spec.containers[*].image`
- `spec.initContainers[*].image`
- `spec.activeDeadlineSeconds`
- `spec.tolerations`

Но можно его отредактировать если он был запущен в Deployment.