Taints - правило которое устанавливается но узле
Toleration - правило которое устнанавливается на под

>[!important]
>Включенная Taint на узле не говорит поду который толерантен к этому узлу идти именно на этот узел. Он может быть назначен планировщиком на любой другйо подходящий узел. 

- По умолчанию taint включен на master-node так как на нем не рекомендуется размещать рабочие нагрузки.

Чтобы установить taint на узле, воспользуйтесь командой:
`kubectl taint nodes <node> <key>=<value>:<taint_effect>`
Для удаления необходимо ввести ту же команду только с минусом в конце 
`kubectl taint nodes <node> <key>=<value>:<taint_effect>-`

Существует три "taints effect":
- NoSchedule - поды не будут размещаться на узле
- PreferNoSchedule - kube-sheduler будет пытаться не размещать на данном узле поды, но это не гарантированно.
- NoExecute - новые поды не будут размещены на узле, а новые будут удалены если они не проходят taint

Чтобы добавить toleration в под, необходимо определить это в манифесте:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: nginx
      image: nginx
  tolerations:      # Все значения должны быть заключены в двойные кавычки.
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```
Эти значения подошли бы если бы использовали данную команду для назначения taint на узел:
`kubectl taint nodes some-node app=blue:NoSchedule`
