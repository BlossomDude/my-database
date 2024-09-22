---
Owner: Blossom Dude
Last edited time: 2024-02-20T20:24
Created time: 2023-06-30T11:26
---
[[Wiki/devops/Ansible/Команды|Команды]]

[[Roles]]

[[Встроенные переменные Ansible]]

[[YAML]]

[[Playbook]]

[[inventory]]

[[Modules]]

[[Условия]]

> [!important]  
> Для работы не нужно устанавливать агенты.Ansible подключается к удаленным хостам по SSH, а к win машинам по Powershell Remoting  

  

==Playbook== - используется для описания сценариев, язык YAML.

==Модули== - Ansible запускает модули на узлах и удаляет их по завершении.

==Файл hosts== - файл для группировки всех узлов.

==Конфиг== - /etc/ansible/ansible.cfg

  

### ==Переменные==

**Jinja 2 Templating**

```YAML
-
  name: Ass DNS
  hosts: target
	vars_files:
    - variables.yml
	tasks:
		- lineinfile:
				path: /etc/resolv.conf
				line: 'nameserver {{ dns_server }}'
```

Желательно использовать файл переменных к каждому плейбуку:

```YAML
dns_server: 10.1.20.90
```