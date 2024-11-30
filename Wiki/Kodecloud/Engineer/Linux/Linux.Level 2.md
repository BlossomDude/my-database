# 1: Add CronJob
### Problem

```text
...
```

### Solution:

``` bash
ssh...
sudo -s

yum install cronie
systemctl start crond
crontab -e
```



# 3: Linux Collaborative Directories
### Problem3

``` text
The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the `dbadmin` group of the team.  
  
Setup a collaborative directory `/dbadmin/data` on `app server 2` in `Stratos Datacenter`.   

The directory should be group owned by the group `dbadmin` and the group should own the files inside the directory. The directory should be `read/write/execute` to the user and group owners, and `others` should not have any access.

```
### Solution:

``` shell
chgrp -R dbadmin /dbadmin/data
chmod -R 2770 /dbadmin/data
```

# 4: Linux String Substitute (sed)
### Problem

```text
There is some data on `Nautilus App Server 3` in `Stratos DC`. Data needs to be altered in several of the files. On `Nautilus App Server 3`, alter the `/home/BSD.txt` file as per details given below:  
  
a. Delete all lines containing word `software` and save results in `/home/BSD_DELETE.txt` file. (Please be aware of case sensitivity)  
   
b. Replace all occurrence of word `and` to `them` and save results in `/home/BSD_REPLACE.txt` file.```
```

### Solution
```bash
sudo sed '/software/d' /home/BSD.txt > /home/BSD_DELETE.txt
sudo sed 's/\bto\b/from/g' /home/BSD.txt > /home/BSD_REPLACE.txt
```


# 5: Linux SSH Authentication
### Problem
```text
The system admins team of `xFusionCorp Industries` has set up some scripts on `jump host` that run on regular intervals and perform operations on all app servers in `Stratos Datacenter`. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1). Based on the requirements, perform the following:  
  
Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.
```

### Solution
```bash
ssh-keygen 
ssh-copy-id banner@stapp03
```



# 6: Linux Find Command
### Problem6
```text
During a routine security audit, the team identified an issue on the Nautilus App Server. Some malicious content was identified within the website code. After digging into the issue they found that there might be more infected files. Before doing a cleanup they would like to find all similar files and copy them to a safe location for further investigation. Accomplish the task as per the following requirements:  
  
a. On `App Server 3` at location `/var/www/html/ecommerce` find out all files (not directories) having `.php` extension.  
b. Copy all those files along with their `parent directory structure` to location `/ecommerce` on same server.  
c. Please make sure not to copy the entire `/var/www/html/ecommerce` directory content.
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



# 7: Install a package
### Problem
```text
As per new application requirements shared by the `Nautilus` project development team, serveral new packages need to be installed on all app servers in `Stratos Datacenter`. Most of them are completed except for `telnet`.  

Therefore, install the `telnet` package on all `app-servers`.

```


### Solution7
```bash
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config 
sudo cat /etc/ssh/sshd_config | grep PermitRootLogin
sudo systemctl stop sshd 
sudo systemctl start sshd
sudo systemctl status sshd
```

# 8: Install Ansible
### Problem
```text
During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with `Ansible` for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use `jump host` as an Ansible controller to test different kind of tasks on rest of the servers.  
  
Install `ansible` version `4.7.0` on `Jump host` using `pip3` only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.
```

### Solution
```bash
# Обновляем pip3
python3 -m pip install --upgrade pip 

# Устанавливаем ansible
python3 -m pip install --user ansible==4.7.0

sudo mv ~/.local/bin/ansible /usr/bin/
```



# 9: 
### Problem
```text
The `Nautilus` production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers. For now they have decided to configure a local yum repo on `Nautilus Backup Server`. This is one of the pending items from last month, so please configure a local yum repository on `Nautilus Backup Server` as per details given below.  
  
a. We have some packages already present at location `/packages/downloaded_rpms/` on `Nautilus Backup Server`.  
  
b. Create a yum repo named `epel_local` and make sure to set `Repository ID` to `epel_local`. Configure it to use package's location `/packages/downloaded_rpms/`.  
  
c. Install package `wget` from this newly created repo.
```

### Solution
```bash
createrepo --distro='epel_local,epel_local' /packages/downloaded_rpms/
sudo vi /etc/yum.repos.d/epel_local

#[epel_local]
#name=epel_local
#baseurl=file:///packages/downloaded_rpms/
#enabled=1

yum install -y wget
```



# 10: 
### Problem
```text
As per details shared by the development team, the new application release has some dependencies on the back end. There are some packages/services that need to be installed on all app servers under `Stratos Datacenter`. As per requirements please perform the following steps:  

a. Install `cups` package on all the application servers.  

b. Once installed, make sure it is enabled to start during boot.
```

### Solution
```bash
ssh user@stapp
sudo yum install -y cups
sudo systemctl enable cups
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
yum install selinux-policy-devel policycoreutils
yum install selinux-policy-targeted
vi /etc/selinux/config # add SELINUX: disabled
```


