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

sudo mv /home/thor/.local/bin/ansible /usr/bin/
```



# 9: Configure Local Yum repos
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



# 10: Linux Services
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



# 11: Linux Configure sudo
### Problem
```text
We have some users on all app servers in `Stratos Datacenter`. Some of them have been assigned some new roles and responsibilities, therefore their users need to be upgraded with sudo access so that they can perform admin level tasks.  
  
a. Provide sudo access to user `kareem` on all app servers.  
  
b. Make sure you have set up password-less sudo for the user.
```

### Solution
```bash
sudo usermod -aG kareem root
sudo vi /etc/sudoers
# kareem  ALL=(ALL) NOPASSWD: ALL
```


# 12: DNS Troubleshooting
### Problem
```text
Настроить днс сервер
```

### Solution
```bash
vi /etc/resolv.conf
#nameserver 8.8.8.8
#nameserver 8.8.4.4
```


# 13: Linux Firewalld Setup
### Problem
```text
To secure our `Nautilus` infrastructure in `Stratos Datacenter` we have decided to install and configure `firewalld` on all app servers. We have Apache and Nginx services running on these apps. Nginx is running as a reverse proxy server for Apache. We might have more robust firewall settings in the future, but for now we have decided to go with the given requirements listed below:  

a. Allow all incoming connections on Nginx port, i.e `80`.  
b. Block all incoming connections on Apache port, i.e `8080`.  
c. All rules must be permanent.  
d. Zone should be public.  
e. If Apache or Nginx services aren't running already, please make sure to start them.

```

### Solution
```bash
sudo yum install -y firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
sudo firewall-cmd --permanent --zone=public --remove-port=8080/tcp
```



# 14:  Linux Postfix Mail Server
### Problem
```text
`xFusionCorp Industries` has planned to set up a common email server in `Stork DC`. After several meetings and recommendations they have decided to use `postfix` as their `mail transfer agent` and `dovecot` as an `IMAP/POP3` server. We would like you to perform the following steps:  
   
1. Install and configure `postfix` on `Stork DC` mail server.  
2. Create an email account `yousuf@stratos.xfusioncorp.com` identified by `ksH85UJjhb`.   
3. Set its mail directory to `/home/yousuf/Maildir`.    
4. Install and configure `dovecot` on the same server.
```

### Solution
```bash
sudo systemctl set-default graphical.target
sudo systemctl get-default
```


# 15: Linux Postfix Troubleshooting
### Problem
```text

Some users of the monitoring app have reported issues with `xFusionCorp Industries` mail server. They have a mail server in `Stork DC` where they are using `postfix` mail transfer agent. `Postfix` service seems to fail. Try to identify the root cause and fix it.

```

### Solution
```bash
https://sachins-kodekloud-tasks.hashnode.dev/task-linux-postfix-mail-server
```


# 16: Install and Configure Ha Proxy LBR
### Problem
```text
There is a static website running in `Stratos Datacenter`. They have already configured the app servers and code is already deployed there. To make it work properly, they need to configure `LBR` server. There are number of options for that, but team has decided to go with `HAproxy`. FYI, apache is running on port `3003` on all app servers. Complete this task as per below details.  
  
a. Install and configure `HAproxy` on `LBR` server using `yum` only and make sure all app servers are added to `HAproxy` load balancer. `HAproxy` must serve on default `http` port (`Note`: Please do not remove `stats socket /var/lib/haproxy/stats` entry from haproxy default config.).  
  
b. Once done, you can access the website using `StaticApp` button on the top bar.
```

### Solution
```bash
yum install haproxy
vi /etc/haproxy/haproxy.cfg
	#frontend main
	#   bind *:80
	#...
	#backend app
	#   balance     roundrobin
	#   server  stapp01 172.16.238.10:3003 check
	#   server  stapp02 172.16.238.11:3003 check
	#   server  stapp03 172.16.238.12:3003 check

haproxy -f /etc/haproxy/haproxy.cfg
syst
```


# 17: Haproxy LBR Troubleshooting
### Problem
```text

`xFusionCorp Industries` has an application running on `Nautlitus` infrastructure in `Stratos Datacenter`. The monitoring tool recognised that there is an issue with the `haproxy` service on `LBR` server. That needs to fixed to make the application work properly.    

Troubleshoot and fix the issue, and make sure `haproxy` service is running on `Nautilus LBR` server. Once fixed, make sure you are able to access the website using `StaticApp` button on the top bar.
```

### Solution
```bash
vi /etc/haproxy/haproxy.cfg
# Fix error 1:
# frontend main
#	bind *:80
# Fix error 2: 
# backend app
#   balance     roundrobin

haproxy -f /etc/haproxy/haproxy.cfg

```


# 18: Maria DB Troubleshooting
### Problem
```text

There is a critical issue going on with the `Nautilus` application in `Stratos DC`. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.  

Look into the issue and fix the same.

```

### Solution
```bash
systemctl status mariadb
journalctl -xeu mariadb
mv /var/lib/mysqld/ /var/lib/mysql/
systemctl start mariadb
```

# 19: Linux Bash Scripts
### Problem
```text

The production support team of `xFusionCorp Industries` is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on `App Server 3` in `Stratos Datacenter`, and they need to create a bash script named `ecommerce_backup.sh` which should accomplish the following tasks. (Also remember to place the script under `/scripts` directory on `App Server 3`).  
  

a. Create a zip archive named `xfusioncorp_ecommerce.zip` of `/var/www/html/ecommerce` directory.    

b. Save the archive in `/backup/` on `App Server 3`. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on `Nautilus Backup Server`.  

c. Copy the created archive to `Nautilus Backup Server` server in `/backup/` location.  

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of `App Server 1`) must be able to run it.

```

### Solution
```bash
sudo yum install sshpass

# write script:
zip /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce
sshpass -p "H@wk3y3" scp /backup/xfusioncorp_ecommerce.zip

```

# 20: Linux Bash Scripts
### Problem
```text

The production support team of `xFusionCorp Industries` is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on `App Server 3` in `Stratos Datacenter`, and they need to create a bash script named `ecommerce_backup.sh` which should accomplish the following tasks. (Also remember to place the script under `/scripts` directory on `App Server 3`).  
  

a. Create a zip archive named `xfusioncorp_ecommerce.zip` of `/var/www/html/ecommerce` directory.    

b. Save the archive in `/backup/` on `App Server 3`. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on `Nautilus Backup Server`.  

c. Copy the created archive to `Nautilus Backup Server` server in `/backup/` location.  

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of `App Server 1`) must be able to run it.

```

### Solution
```bash
sudo yum install sshpass

# write script:
zip /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce
sshpass -p "H@wk3y3" scp /backup/xfusioncorp_ecommerce.zip

```
