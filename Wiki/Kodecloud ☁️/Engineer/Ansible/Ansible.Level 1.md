# 1

### Problem
```
An Ansible playbook needs completion on the `jump host`, where a team member left off. Below are the details:  
  
1. The inventory file `/home/thor/ansible/inventory` requires adjustments. The playbook must run on `App Server 3` in `Stratos DC`. Update the inventory accordingly.     
2. Create a playbook `/home/thor/ansible/playbook.yml`. Include a task to create an empty file `/tmp/file.txt` on `App Server 3`.

---

На хосте jump требуется доработать Ansible playbook, на котором один из членов команды остановился. Ниже приведены подробные сведения:  
   
1.Файл инвентаря /home/thor/ansible/inventory требует корректировки. Playbook должен запускаться на App Server 3 в Stratos DC. Обновите инвентарь соответствующим образом.  
2.Создайте playbook /home/thor/ansible/playbook.yml. Включите задачу по созданию пустого файла /tmp/file.txt на сервере приложений 3.
```

### Solution
```yaml
# Add playbook
-
  name: create_file
  hosts: stapp03
  tasks:
    - name: create_empty_file
      command: touch /tmp/file.txt
```



# 2

### Problem
```


---


```

### Solution
```bash

```



# 3

### Problem
```


---


```

### Solution
```bash

```



# 4

### Problem
```


---


```

### Solution
```bash

```



# 5

### Problem
```


---


```

### Solution
```bash

```
