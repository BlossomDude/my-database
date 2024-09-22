---
Owner: Blossom Dude
Last edited time: 2023-11-29T14:19
Created time: 2023-11-29T12:17
---
> [!important]  
> HPA - HorizontalPodAutoscaler  

Горизонтальное распределение нагрузки **pods**

```YAML
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler       # Создаем HPA
metadata:
  name: my-autoscale                 # Имя HPA
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment                 # Этот hpa будет скейлить
    name: my-web-deployment          # деплой с данным именем
  minReplicas: 3                     # Мин и макс реплик подов
  maxReplicas: 5                    
  metrics:                           # Контроль ресурсов
    - type: Resource
      resource:
        name: cpu
        targetAverageUtilization: 70 # Максимальная утилизация
    - type: Resource                 # cpu 70 процентов, а 
      resource:                      # ram - 80 процентов
        name: memory
        targetAverageUtilization: 80
```