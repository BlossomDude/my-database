# 1: Troubleshoot and Create Ansible Playbook

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



# 2: Create Ansible Inventory for App Server Testing

### Problem
```
The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under `/home/thor/playbook/` directory on the `jump host` and now intend to test them on `app server 3` in `Stratos DC`. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:

a. Create an ini type Ansible inventory file `/home/thor/playbook/inventory` on `jump host`.  
b. Include `App Server 3` in this inventory along with necessary variables for proper functionality.   
c. Ensure the inventory hostname corresponds to the `server name` as per the wiki, for example `stapp01` for `app server 1` in `Stratos DC`.  
  
`Note:` Validation will execute the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook functions properly without any extra arguments.

---

Команда разработчиков Nautilus тестирует сборники игр Ansible на различных серверах в своем стеке. Они разместили несколько сборников игр в каталоге /home/thor/ playbook / на хостинге jump и теперь намерены протестировать их на app server 3 в Stratos DC. Однако для подключения Ansible к соответствующему приложению необходимо создать файл инвентаря. Вот требования.

a. Создайте файл инвентаря Ansible ini типа / home / thor / playbook / inventory на jump host.
б. Включите App Server 3 в этот список вместе с необходимыми переменными для надлежащей функциональности.
c. Убедитесь, что имя хоста инвентаря соответствует имени сервера в соответствии с wiki, например stapp01 для сервера приложений 1 в Stratos DC.

Примечание: Проверка выполнит playbook с помощью команды ansible-playbook -i inventory playbook.yml. Убедитесь, что playbook функционирует должным образом без каких-либо дополнительных аргументов.
```

### Solution
```bash
# add inventory file
```



# 3: Configure Default SSH User for Ansible

### Problem
```
The Nautilus DevOps team aims to manage all servers within the stack using Ansible, utilizing a common sudo user across all servers. They plan to use this user for various tasks on each server. While this isn't finalized, they're starting with testing. Ansible is already installed on the `jump host` via yum. Here's the requirement:

On the `jump host`, modify the default configuration of Ansible to enable the use of `mariyam` as the default SSH user for all hosts. Ensure to make changes within Ansible's default configuration without creating a new one.

---

Команда Nautilus DevOps стремится управлять всеми серверами в стеке с помощью Ansible, используя общего пользователя sudo для всех серверов. Они планируют использовать этого пользователя для выполнения различных задач на каждом сервере. Пока это не завершено, они приступают к тестированию. Ansible уже установлен на хосте jump через yum. Вот требования:  
  
На хосте jump измените конфигурацию Ansible по умолчанию, чтобы разрешить использование mariyam в качестве пользователя SSH по умолчанию для всех хостов. Убедитесь, что вы можете вносить изменения в конфигурацию Ansible по умолчанию, не создавая новую.
```

### Solution
```bash
# Add "remote_user = mariyam" to ansible.cfg
```



# 4: Copy Data to App Servers using Ansible

### Problem
```
The Nautilus DevOps team needs to copy data from the `jump host` to all `application servers` in `Stratos DC` using Ansible. Execute the task with the following details:

a. Create an inventory file `/home/thor/ansible/inventory` on `jump_host` and add all application servers as managed nodes.    
b. Create a playbook `/home/thor/ansible/playbook.yml` on the `jump host` to copy the `/usr/src/data/index.html` file to all application servers, placing it at `/opt/data`.  

`Note:` Validation will run the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook functions properly without any extra arguments.

---

Команде разработчиков Nautilus необходимо скопировать данные с узла jump на все серверы приложений в Stratos DC с помощью Ansible. Выполните задачу, указав следующие сведения:

a. Создайте файл инвентаризации /home /thor / ansible /inventory на jump_host и добавьте все серверы приложений в качестве управляемых узлов.
б. Создайте playbook /home/thor/ansible/playbook.yml на узле перехода, чтобы скопировать файл /usr/src/data/index.html на все серверы приложений, разместив его по адресу /opt/data.

Примечание: Проверка запустит playbook с помощью команды ansible-playbook -i inventory playbook.yml. Убедитесь, что playbook функционирует должным образом без каких-либо дополнительных аргументов.
```

### Solution
```bash
-
  name: "Copy file"
  hosts: app_nodes
  tasks:
    -
      name: "Copy file"
      copy:
        src: /usr/src/data/index.html
        dest: /opt/data/index.html

stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n

[app_nodes]
stapp01
stapp02
stapp03
```



# 5: Create Files on App Servers using Ansible

### Problem
```


---


```

### Solution
```bash

```
