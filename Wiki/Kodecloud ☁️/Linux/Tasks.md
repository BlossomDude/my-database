
- [[#STRATUS DC:|STRATUS DC - Данные об инфраструктуре]]

- [[#2:|2:Group Creation and User Assignment]]
	- [[#Problem:|Problem]]
	- [[#Solution:|Solution]]
- [[#3: Linux User Setup with Non-Interactive Shell:|3: Linux User Setup with Non-Interactive Shell]]
	- [[#Problem3:|Problem]]
	- [[#Solution3:|Solution]]
# STRATUS DC:

| stapp01   | 172.16.238.10 | stapp01.stratos.xfusioncorp.com   | tony    | Ir0nM@n    | Nautilus App 1                 |
| --------- | ------------- | --------------------------------- | ------- | ---------- | ------------------------------ |
| stapp02   | 172.16.238.11 | stapp02.stratos.xfusioncorp.com   | steve   | Am3ric@    | Nautilus App 2                 |
| stapp03   | 172.16.238.12 | stapp03.stratos.xfusioncorp.com   | banner  | BigGr33n   | Nautilus App 3                 |
| stlb01    | 172.16.238.14 | stlb01.stratos.xfusioncorp.com    | loki    | Mischi3f   | Nautilus HTTP LBR              |
| stdb01    | 172.16.239.10 | stdb01.stratos.xfusioncorp.com    | peter   | Sp!dy      | Nautilus DB Server             |
| ststor01  | 172.16.238.15 | ststor01.stratos.xfusioncorp.com  | natasha | Bl@kW      | Nautilus Storage Server        |
| stbkp01   | 172.16.238.16 | stbkp01.stratos.xfusioncorp.com   | clint   | H@wk3y3    | Nautilus Backup Server         |
| stmail01  | 172.16.238.17 | stmail01.stratos.xfusioncorp.com  | groot   | Gr00T123   | Nautilus Mail Server           |
| jump_host | Dynamic       | jump_host.stratos.xfusioncorp.com | thor    | mjolnir123 | Jump Server to Access Stork DC |
| jenkins   | 172.16.238.19 | jenkins.stratos.xfusioncorp.com   | jenkins | j@rv!s     | Jenkins Server for CI/CD       |

# 2: Group Creation and User Assignment
### Problem

```text
The system admin team at `xFusionCorp Industries` has streamlined access management by implementing group-based access control. Here's what you need to do:  

a. Create a group named `nautilus_sftp_users` across all App servers within the `Stratos Datacenter`.  
  
b. Add the user `kano` into the `nautilus_sftp_users` group on all App servers. If the user doesn't exist, create it as well.

---

Команда системных администраторов "xFusionCorp Industries" упростила управление доступом, внедрив групповой контроль доступа. Вот что вам нужно сделать: 


a. Создайте группу с именем "nautilus_sftp_users" на всех серверах приложений в "Центре обработки данных Stratos`. 

б. Добавьте пользователя "kano" в группу "nautilus_sftp_users" на всех серверах приложений. Если пользователь не существует, создайте его.
```

### Solution:

``` bash
ssh user@hostname
sudo groupadd nautilus_sftp_users
id kano                                     # Узнает существует ли пользователь
sudo useradd kano 
sudo usermod -a -G nautilus_sftp_users kano
```



# 3: Linux User Setup with Non-Interactive Shell
### Problem3
``` text
To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell. Here's your task:  

Create a user named `kareem` with a non-interactive shell on `App Server 2`.

___


Чтобы соответствовать спецификациям инструмента backup agent, команде системных администраторов `xFusionCorp Industries` требуется создать пользователя с неинтерактивной оболочкой. Вот ваша задача: 

Создайте пользователя с именем "Карим" с неинтерактивной оболочкой на "Сервере приложений 2`.

```

>**Non-interactive shell** – это оболочка, которая используется для выполнения автоматизированных сценариев или команд, не требующих взаимодействия с пользователем. В отличие от интерактивной оболочки, где пользователь вводит команды напрямую, неинтерактивная оболочка работает без обычной среды и настроек интерактивного сеанса, позволяя выполнять команды без прерываний. Это особенно полезно при автоматизации задач, таких как ежедневное резервное копирование данных через cron-задания, где скрипт запускается автоматически в заданное время без участия человека.
### Solution3:

``` bash
ssh user@hostname 
sudo adduser kareem -s /sbin/login   #выбрать оболчку новому пользователю
```

# 4
### Problem4
```text
```

### Solution4
```bash
```
