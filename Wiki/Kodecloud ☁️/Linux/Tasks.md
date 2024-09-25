
- [[#STRATUS DC:|STRATUS DC - Данные об инфраструктуре]]

- [[#2: Group Creation and User Assignment:|2:Group Creation and User Assignment]]
	- [[#Problem:|Problem]]
	- [[#Solution:|Solution]]
- [[#3: Linux User Setup with Non-Interactive Shell:|3: Linux User Setup with Non-Interactive Shell]]
	- [[#Problem3:|Problem]]
	- [[#Solution3:|Solution]]
- [[#4: Service User Creation without Home Directory:| 4: Service User Creation without Home Directory]]
	- [[#Problem4:|Problem]]
	- [[#Solution4:|Solution]]
- [[#5: Temporary User Setup with Expiry]]
	- [[#Problem5:|Problem]]
	- [[#Solution5:|Solution]]
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
id kano                                     # Узнать, существует ли пользователь
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

# 4: Service User Creation without Home Directory
### Problem4

```text
In response to the latest tool implementation at `xFusionCorp Industries`, the system admins require the creation of a service user account. Here are the specifics:  

Create a user named `anita` in `App Server 3` without a home directory.

---

В связи с последним внедрением инструмента в xFusionCorp Industries системные администраторы требуют создания учетной записи пользователя сервиса. Вот подробности:  

Создайте пользователя с именем anita в App Server 3 без домашнего каталога
```
### Solution4

```bash
ssh user@hostname
sudo adduser --no-create-home anita
```


	# 5: Temporary User Setup with Expiry
### Problem5
```text
As part of the temporary assignment to the `Nautilus` project, a developer named `siva` requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:  
  
Create a user named `siva` on `App Server 2` in Stratos Datacenter. Set the expiry date to `2024-01-28`, ensuring the user is created in lowercase as per standard protocol.

---

В рамках временного назначения в проект Nautilus разработчику по имени siva требуется доступ на ограниченный срок. Для обеспечения бесперебойного управления доступом необходима временная учетная запись пользователя с истекшим сроком действия. Вот что вам нужно сделать:

Создайте пользователя с именем siva на App Server 2 в Stratos Datacenter. Установите дату истечения срока действия на 2024-01-28, гарантируя, что пользователь будет создан в нижнем регистре в соответствии со стандартным протоколом.
```

### Solution5
```bash
ssh username@hostname
sudo adduser -e 2024-01-28 siva
```



# 6: Linux User Data Transfer
### Problem6
```text
Due to an accidental data mix-up, user data was unintentionally mingled on Nautilus `App Server 1` at the `/home/usersdata` location by the Nautilus production support team in Stratos DC. To rectify this, specific user data needs to be filtered and relocated. Here are the details:  
  
Locate all files (excluding directories) owned by user `james` within the `/home/usersdata` directory on `App Server 1`. Copy these files while preserving the directory structure to the `/official` directory.

---

Из-за случайной путаницы пользовательские данные были непреднамеренно перемешаны на сервере приложений Nautilus App Server 1 по адресу /home/usersdata командой технической поддержки Nautilus production в Stratos DC. Чтобы исправить это, необходимо отфильтровать и переместить данные конкретного пользователя. Вот подробности:  
  
Найдите все файлы (за исключением каталогов), принадлежащие пользователю james, в каталоге /home/usersdata на сервере приложений 1. Скопируйте эти файлы, сохранив структуру каталогов, в каталог /official.
```

### Solution6
```bash
find /home/usersdata -type f -user james -exec cp --parents {} /official \;
```

>Команда `find /home/usersdata -type f -user john -exec cp --parents {} /blog \;` выполняет следующие действия:
>1. **Поиск файлов:** Команда `find` используется для поиска файлов в каталоге `/home/usersdata`.
>2. **Отбор файлов по типу и владельцу:** Параметр `-type f` указывает, что должны быть найдены только обычные файлы, а `-user john` выбирает файлы, владельцем которых является пользователь `john`.
>3. **Копирование файлов:** Для каждого найденного файла выполняется команда `cp --parents`, которая копирует файл в каталог `/blog`, сохраняя структуру каталогов. Параметр `--parents` позволяет создать родительские каталоги, если они не существуют.
>4. **Выполнение команды:** После нахождения файла, соответствующего критериям поиска, команда `cp --parents` выполняется с использованием параметра `-exec`. Фигурные скобки `{}` заменяются на путь к файлу, который был найден.
>5. Косая черта в конце команды `find /home/usersdata -type f -user john -exec cp --parents {} /blog \;` служит для завершения выполнения команды `cp --parents`, которая была запущена с использованием параметра `-exec` команды `find`. Без этой косой черты команда `find` будет продолжать искать файлы и пытаться выполнить команду `cp --parents` для каждого найденного файла, что может привести к нежелательным результатам или ошибкам.



# 7
### Problem
```text
```

### Solution
```bash
```