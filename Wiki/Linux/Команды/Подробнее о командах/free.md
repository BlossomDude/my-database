
```bash
dude@bloss-pc:/$ free -h
               total        used        free      shared  buff/cache   available
Mem:           3.8Gi       348Mi       3.2Gi       2.0Mi       277Mi       3.2Gi
Swap:          1.0Gi          0B       1.0Gi
```

- `total` – общий размер ОЗУ
- `used` – реально использующая в данный момент и зарезервированная системой память
- `free` – свободная память (total - used)
- `shared` – разделяемая память
- `buffers` – буферы в памяти – страницы памяти, зарезервированные системой для выделения их процессам, когда им это потребуется
- `cached` – файлы, которые недавно были использованы системой/процессами и хранящиеся в памяти на случай, если они снова потребуются
- `available` – то что доступно, можно освободить удалив кэш файлов.

```
kubectl get deploy -n admin2406 \
-o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers.image,
```