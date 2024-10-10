---
Owner: Blossom Dude
Last edited time: 2024-02-08T12:36
Created time: 2024-02-08T12:18
---
### COMMAND

Выполняет команду на удаленном хосте

```YAML
-
  name: Play1
  hosts: all
  tasks:
    - name: Execute command date
      command: date
    - name: Display resolv.conf
      command: cat resolv.conf chdir=/etc
    - name: Create directory
      command: mkdir /folder creates=/folder  ## эта команда будет запущена если папки не существует
```

  

### SCRIPT

Выполняет скрипт который **находится на пк контроллере Ansible**

```YAML
-
  name: Play1
  hosts: target1
  tasks:
    - name: Run script
      script: /some/local/script.sh -arg1 -arg2
```

  

### ==SERVICE==

```YAML
-
  name: Start services
  hosts: target1
  tasks:
    - name: Start DB
			service:
			  name: postgresql
				state: started
		- name: Start WebServer
      service:
				name: httpd
				state: started
```

  

### ==LINEINFILE==

Ищет линию в файле и заменяет ее, либо добавляет, если ее ранее не было.

```YAML
-
  name: Add DNS Server to resovl.conf
  hosts: target1
  tasks:
	- lineinfile:
      path: /etc/resolv.conf
	  line: 'nameserver 10.1.250.10'
```

FILE
``` yaml
-
  name: "Create file"
  hosts: app_nodes
  tasks:
    -  
      name: "Create file"
      file:
        path: /tmp/data.txt
        state: touch
        mode: '0655'
```