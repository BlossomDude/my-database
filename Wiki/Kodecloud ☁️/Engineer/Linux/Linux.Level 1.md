
- [[#STRATOS DC:|STRATOS]]

- [[#2 Group Creation and User Assignment]]
- [[#3 Linux User Setup with Non-Interactive Shell]]
- [[#4 Service User Creation without Home Directory]]
- [[#5 Temporary User Setup with Expiry]]
- [[#6 Linux User Data Transfer]]
- [[#7 Secure Root SSH Access]]
- [[#8 Data Backup for Developer]]
- [[#9 Script Execution Permissions]]
- [[#10 File Permission Correction]]
- [[#11 String Replacement]]
- [[#12 Secure Data Transfer]]
- 
# STRATOS DC



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



# 7: Secure Root SSH Access
### Problem7
```text
Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.  
  
Your task is to disable direct SSH root login on all app servers within the `Stratos Datacenter`.

---

После аудита безопасности команда безопасности xFusionCorp Industries внедрила новые протоколы, включая ограничение прямого входа по SSH с правами root.

Ваша задача - отключить прямой вход по SSH root на всех серверах приложений в Центре обработки данных Stratos.
```


### Solution7
```bash
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config 
sudo cat /etc/ssh/sshd_config | grep PermitRootLogin
sudo systemctl stop sshd 
sudo systemctl start sshd
sudo systemctl status sshd
```

# 8: Data Backup for Developer
### Problem8
```text
Within the Stratos DC, the Nautilus storage server hosts a directory named `/data`, serving as a repository for various developers non-confidential data. Developer `john` has requested a copy of their data stored in `/data/john`. The System Admin team has provided the following steps to fulfill this request:  
  
a. Create a compressed archive named `john.tar.gz` of the `/data/john` directory.
b. Transfer the archive to the `/home` directory on the Storage Server.```

---

В Stratos DC на сервере Nautilus storage server размещен каталог с именем /data, который служит хранилищем различных неконфиденциальных данных разработчиков. Разработчик john запросил копию своих данных, хранящихся в /data/john. Команда системных администраторов выполнила следующие действия для выполнения этого запроса:  
  
a. Создайте сжатый архив с именем john.tar.gz из каталога /data/john.  
b. Перенесите архив в каталог /home на сервере хранения.
```

### Solution8
```bash
tar -czf john.tar.gz ./john
mv john.tar.gz /home
```



# 9: Script Execution Permissions
### Problem9
```text
In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on `App Server 1` within the Stratos Datacenter.  
  
Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 1`. Additionally, ensure that all users have the capability to execute it.

---

В попытке автоматизировать процессы резервного копирования команда системных администраторов xFusionCorp Industries разработала новый скрипт bash под названием xfusioncorp.sh. Хотя этот скрипт был распространен на все необходимые серверы, ему не хватает разрешений на выполнение на сервере приложений 1 в центре обработки данных Stratos.  
  
Ваша задача - предоставить разрешения на выполнение скрипту /tmp/xfusioncorp.sh на сервере приложений 1. Кроме того, убедитесь, что все пользователи имеют возможность его выполнить.
```

### Solution9
```bash
chmod 777 xfusioncorp.sh
```



# 10: File Permission Correction
### Problem
```text
After conducting a security audit within the `Stratos DC`, the Nautilus security team discovered misconfigured permissions on critical files. To address this, corrective actions are being taken by the production support team. Specifically, the file named `/etc/resolv.conf` on `Nautilus App 1` server requires adjustments to its Access Control Lists (ACLs) as follows:  
  
1. The file's user owner and group owner should be set to `root`.  
2. `Others` should possess `read only` permissions on the file.  
3. User `yousuf` must not have any permissions on the file.  
4. User `jerome` should be granted `read only` permission on the file.

---

После проведения аудита безопасности в Stratos DC команда безопасности Nautilus обнаружила неправильно настроенные права доступа к критически важным файлам. Для устранения этой проблемы команда технической поддержки предпринимает корректирующие действия. В частности, для файла с именем /etc/resolv.conf на сервере Nautilus App 1 требуется внести изменения в списки контроля доступа (ACL) следующим образом: 

1. Пользователь-владелец файла и владелец группы должны иметь права root. 
2. Другие пользователи должны обладать правами доступа к файлу только для чтения. 
3. Пользователь yousuf не должен иметь никаких прав доступа к файлу. 
4. Пользователю jerome должно быть предоставлено разрешение на доступ к файлу только для чтения.
```

### Solution
```bash
```



# 11: String Replacement
### Problem
```text
Within the Stratos DC, the backup server holds template XML files crucial for the Nautilus application. Before utilization, these files require valid data insertion. As part of routine maintenance, system admins at `xFusionCorp Industries` employ string and file manipulation commands.  
  
Your task is to substitute all occurrences of the string `About` with `Echo-Location` within the XML file located at `/root/nautilus.xml` on the backup server.

---

На сервере резервного копирования Stratos DC хранятся XML-файлы шаблонов, необходимые для работы приложения Nautilus. Перед использованием в эти файлы необходимо вставить корректные данные. В рамках планового технического обслуживания системные администраторы xFusionCorp Industries используют команды управления строками и файлами.  
  
Ваша задача - заменить все вхождения строки About на Echo-Location в XML-файле, расположенном по адресу /root/nautilus.xml на сервере резервного копирования.
```

### Solution
```bash
sudo sed -i 's/About/Echo-Location/g' /root/nautilus.xml
```


# 12: Secure Data Transfer
### Problem
```text
A `Nautilus` developer has stored confidential data on the jump host within `Stratos DC`. To ensure security and compliance, this data must be transferred to one of the app servers. Given developers lack direct access to these servers, the system admin team has been enlisted for assistance.  
  
Copy `/tmp/nautilus.txt.gpg` file from jump server to `App Server 2` placing it in the directory `/home/appdata`.

---

Разработчик Nautilus сохранил конфиденциальные данные на сервере jump в Stratos DC. Для обеспечения безопасности и соответствия требованиям эти данные необходимо перенести на один из серверов приложений. Поскольку разработчики не имеют прямого доступа к этим серверам, была привлечена команда системных администраторов для оказания помощи.  
  
Скопируйте файл /tmp/nautilus.txt.gpg с jump server на App Server 2, поместив его в каталог /home/appdata.

```

### Solution
```bash
sudo scp /tmp/nautilus.txt.gpg steve@stapp02:/home/appdata
```


# 13: Restrict Cron Access
### Problem
```text
In alignment with security compliance standards, the Nautilus project team has opted to impose restrictions on crontab access. Specifically, only designated users will be permitted to create or update cron jobs.  
  

  

Configure crontab access on App Server 2 as follows: Allow crontab access to `kirsty` user while denying access to the `ryan` user.

---

В соответствии со стандартами безопасности команда Nautilus project решила ввести ограничения на доступ к crontab. В частности, только назначенным пользователям будет разрешено создавать или обновлять задания cron.    
  
Настройте доступ к crontab на App Server 2 следующим образом: Разрешите доступ к crontab пользователю kirsty, отказав в доступе пользователю ryan.

```

### Solution
```bash
sudo touch /etc/cron.allow
sudo touch /etc/cron.deny

sudo echo "kirsty" >> /etc/cron.allow
sudo echo "ryan" >> /etc/cron.deny
```



# 14: Default GUI Boot Configuration
### Problem
```text

With the installation of new tools on the app servers within the `Stratos Datacenter`, certain functionalities now necessitate graphical user interface (GUI) access.  
  
Adjust the `default runlevel` on all App servers in `Stratos Datacenter` to enable GUI booting by default. It's imperative not to initiate a server reboot after completing this task.

---

После установки новых инструментов на серверах приложений в центре обработки данных Stratos для некоторых функций теперь требуется доступ к графическому интерфейсу пользователя (GUI).  
  
Настройте уровень запуска по умолчанию на всех серверах приложений в центре обработки данных Stratos, чтобы включить загрузку с графическим интерфейсом пользователя по умолчанию. Крайне важно не инициировать перезагрузку сервера после выполнения этой задачи.

```

### Solution
```bash
sudo systemctl set-default graphical.target
sudo systemctl get-default
```


# 15: Timezone Alignment
### Problem
```text

In the daily standup, it was noted that the timezone settings across the `Nautilus Application Servers` in the `Stratos Datacenter` are inconsistent with the local datacenter's timezone, currently set to `Asia/Hovd`.  
  
Synchronize the timezone settings to match the local datacenter's timezone (`Asia/Hovd`).

--

В ежедневном обзоре было отмечено, что настройки часового пояса на серверах приложений Nautilus в Центре обработки данных Stratos не соответствуют часовому поясу местного центра обработки данных, который в настоящее время установлен на Asia / Hovd.

Синхронизируйте настройки часового пояса в соответствии с часовым поясом локального центра обработки данных (Asia / Hovd).

```

### Solution
```bash
sudo ln -sf /usr/share/zoneinfo/Asia/Hovd /etc/localtime
```


# 16: Firewall Configuration
### Problem
```text

The `Nautilus` system admins team has rolled out a web UI application for their backup utility on the `Nautilus backup server` within the `Stratos Datacenter`. This application operates on port `3000`, and `firewalld` is active on the server. To meet operational needs, the following requirements have been identified:  
  
Allow all incoming connections on port `3000/tcp`. Ensure the zone is set to `public`.

---

Команда системных администраторов Nautilus внедрила приложение с веб-интерфейсом для своей утилиты резервного копирования на сервере резервного копирования Nautilus в центре обработки данных Stratos. Это приложение работает через порт 3000, а firewalld активен на сервере. Для удовлетворения оперативных потребностей были определены следующие требования:  
  
Разрешить все входящие соединения через порт 3000/tcp. Убедитесь, что для зоны установлено значение public.

```

### Solution
```bash
firewall-cmd --permanent --zone=public --add-port=3000/tcp
```


# 17: Process Limit Adjustment
### Problem
```text

In the `Stratos Datacenter`, our `Storage server` is encountering performance degradation due to excessive processes held by the `nfsuser` user. To mitigate this issue, we need to enforce limitations on its maximum processes. Please set the maximum process limits as specified below:   

a. Set the soft limit to `1026`  
b. Set the hard limit to `2024`

---

В центре обработки данных Stratos на нашем сервере хранения данных наблюдается снижение производительности из-за чрезмерного количества процессов, выполняемых пользователем nfsuser. Чтобы устранить эту проблему, нам необходимо ввести ограничения на максимальное количество процессов. Пожалуйста, установите максимальные ограничения на процессы, как указано ниже:  
    
a. Установите мягкое ограничение на 1026    
b. Установите жесткое ограничение на 2024

```

### Solution
```bash
# Добавил две строки в /etc/security/limits.conf
nfsuser soft nproc 1026
nfsuser hard nproc 2024
```


# 18: SElinux Installation and Configuration
### Problem
```text

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 2` in the `Stratos Datacenter:`

1. Install the required `SELinux` packages.
2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.

---

После аудита безопасности команда безопасности x Fusion Corp Industries приняла решение повысить безопасность приложений и серверов с помощью SELinux. Чтобы начать тестирование, для сервера приложений App server 2 в центре обработки данных Stratos были установлены следующие требования:

1.Установите необходимые пакеты SELinux. 
2.Временно отключите SELinux окончательно; он будет снова включен после внесения необходимых изменений в конфигурацию. 
3.Перезагружать сервер не нужно, так как на сегодняшний вечер уже запланирована плановая перезагрузка для обслуживания. 
4.Не обращайте внимания на текущее состояние SELinux через командную строку; окончательное состояние после перезагрузки должно быть отключено.

```

### Solution
```bash
sudo -s
dnf install policycoreutils-python-utils setools setools-console setroubleshoot

```


