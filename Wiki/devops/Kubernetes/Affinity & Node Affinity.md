```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```

В данном примере Pod будет размещен на любом узле где `color` равен любому значению из `vaues`. 

##### Типы Affinity:

`requiredDuringScheduling`:
Требуемый во время планирования, игнорируемый во время выполнения
- Если нужный узел не будет найдет то под не будет задеплоен

`prefferedDuringScheduling`:
Предпочтительный во время планирования, игнорируемый во время выполнения
- Если нужный узел не будет найден то под будет добавлен на не подходящий узел

`IgnoredDuringExecution` - означает что дальнейшие изменения в узле, никак не повлияют на работающие поды. Например если удалится метка узла благодаря которой поды были размещены именно на этом узле, то поды останутся на этом узле.

`RequiredDuringExecution` - При удаленни метки узла, под будет перемещен на другой подходящий узел

| type | DuringScheduling | DuringExecution |
| ---- | ---------------- | --------------- |
| 1    | required         | ignored         |
| 2    | preffered        | ignored         |
| 3    | required         | required        |
| 4    | preffered        | required        |


##### matchExpressions Operators
В качестве `operator` может быть указаны данные значения:
- `In` - equal - Размещать на узлах с любой меткой из указанных в values
- `NotIn` - not equal - Размещать на узлах на которых нет любой метки из values.
- `Exists` - Размещать на узлах где есть параметр из `key`, т.е. чему оно равно значение не имеет
