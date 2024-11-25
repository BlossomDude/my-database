---
Owner: Blossom Dude
Last edited time: 2024-02-18T13:27
Created time: 2023-11-17T14:43
---
### Playbook - отдельный YAML-файл

- Play - определяет набор действий для выполнения на хостах
    - Task - действие, которое надо выполнить на хосте, например:
        - Запустить скрипт
        - Установить пакет
        - Ребут

### Примеры playbook.yml с задания

```YAML
-
    name: 'Stop the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'
            
-
    name: "Stop db on db-nodes"
    hosts: db_nodes
    tasks:
        -
            name: "Stop db on db nodes"
            command: service mysql stop

-
    name: "reboot all servers"
    hosts: all_nodes
    tasks:
        -
            name: "reboot all servers"
            command: /sbin/shutdown -r

-
    name: "start db"
    hosts: db_nodes
    tasks:
        -
            name: "start db"
            command: service mysql start

-
    name: "start web"
    hosts: web_nodes
    tasks:
        -
            name: "start web"
            command: service httpd start
```

```YAML
-            #ПРИМЕР PLAYBOOK КОТОРЫЙ ВЫПОЛНЯЕТ КОМАНДУ DATE НА REMOTEHOST
    name: Execute a date command on remote host             #Это play
    hosts: remotehost                                       
    tasks:                                                  
	      -                                                   # Это Таск      \
            name: 'Execute a date command'                  # имя(описание)  }
            command: date                                   # команды       /
```

`ansible-playbook playbook.yml` - Запустить playbook с именем **playbook.yml**

Параметры:

`become: true` - предоставляет права рут если работаеш не под рутом.

- Loops
    
    ```YAML
    - name: Install Web
    	hosts: all
    	var:
    		packages:
    			- name: nginx
    				required: True
    			- name: mysql
    				required: True
    			- name: httpd
    				required: False
    	tasks:
    		- name: Install "{{ item.name }}" on Ubuntu
    			apt:
    				name: "{{ item.name }}"
    				state: present
    			when: item.required == True
    			loop: "{{ packages }}"
    ```
    
    ```YAML
    -
      name: Create users
    	hosts: localhost
    	tasks:
    		- user: name='{{ item.name }}' state=present uid='{{ item.uid }}'
    			loop:
    				- name: Boris
    					uid: 1010
    				- name: Dima
    					uid: 1011
    				- name: Grisha
    					uid: 1012
    				- name: Gogi
    					uid: 1013
    				- name: Vadim
    					uid: 1014
    				- name: Kesha
    					uid: 1015
    ```