На данный момент полнофункционального решения kubernetes, для мониторинга кластера и его компонентов. Но есть metric server, prometheus, ELK etc:

---
##### Metric Server
---
Metrics Server — это компонент Kubernetes, который собирает данные об использовании ресурсов в кластере и через [Metrics API](https://github.com/kubernetes/metrics) передает их в другие компоненты Kubernetes. На основании данных от Metrics Server работает горизонтальное (Horizontal Pod Autoscaling) и вертикальное автомасштабирование подов (Vertical Pod Autoscaling). Посмотреть метрики можно с помощью `kubectl top`.

Так же данные в нем хранятся в оперативной памяти, в связи с чем вы не сможете увидеть старые данные. (Но можно опционально хранить историю)

Метрики metric server получает от kubelet, а именно от его компонента CAdvisor.

По умолчанию не включен, поэтому необходимо клонировать репозитории на мастер ноду и запустить набор объектов:
`kubectl apply -f \`
`https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`

После этого появляется возможность запускать команду `kubectl top node(pod)` 

---