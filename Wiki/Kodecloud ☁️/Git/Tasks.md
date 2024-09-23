[[#1 Set Up Git Repository on Storage Server]]
	[[#Problem1]]
	[[#Solution1]]

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


# 1: Set Up Git Repository on Storage Server
### Problem1
```text
The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the `Storage server` in the Stratos DC:  
  

1. Utilize `yum` to install the `git` package on the `Storage Server`.    
    
2. Create a bare repository named `/opt/ecommerce.git` (ensure exact name usage).

---

Команда разработчиков Nautilus предоставила команде DevOps требования к новому проекту разработки приложений, в частности, запрос на создание репозитория Git. Следуйте приведенным ниже инструкциям, чтобы создать репозиторий Git на сервере хранения в Stratos DC.:  
  
Используйте yum для установки пакета git на сервер хранения.  
  
Создайте простой репозиторий с именем /opt/ecommerce.git (убедитесь, что используется точное название).
```

### Solution1
```bash
sudo git init --bare ecommerce.git
```



# 2: Set Up Git Repository on Storage Server
### Problem2
```text
The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the `Storage Server` in the Stratos DC. Follow the provided details to clone the repository:  

1. The repository to be cloned is located at `/opt/official.git`      
2. Clone this Git repository to the `/usr/src/kodekloudrepos` directory. Ensure no modifications are made to the repository during the cloning process.

---

На прошлой неделе команда DevOps создала новый репозиторий Git, который в настоящее время не используется. Однако команде разработчиков приложений Nautilus теперь требуется копия этого репозитория на сервере хранения в Stratos DC. Следуйте приведенным инструкциям, чтобы клонировать репозиторий:  
  
1) Репозиторий, который нужно клонировать, находится по адресу /opt/official.git
  
2) Скопируйте этот репозиторий Git в каталог /usr/src/kodekloudrepos. Убедитесь, что в процессе клонирования в репозиторий не вносились изменения.
```

### Solution2
```bash
cd /usr/src/kodekloudrepos
git clone /opt/official.git
```



# 3: Fork a Git Repository
### Problem3
```text
There is a Git server utilized by the Nautilus project teams. Recently, a new developer named Jon joined the team and needs to begin working on a project. To begin, he must fork an existing Git repository. Follow the steps below:   

1. Click on the `Gitea UI` button located on the top bar to access the Gitea page.
2. Login to `Gitea` server using username `jon` and password `Jon_pass123`.        
3. Once logged in, locate the Git repository named `sarah/story-blog` and `fork` it under the `jon` user.  

---

Команды проекта Nautilus используют Git-сервер. Недавно к команде присоединился новый разработчик по имени Джон, которому необходимо начать работу над проектом. Для начала он должен разветвить существующий репозиторий Git. Выполните следующие действия.:  
  
1)Нажмите на кнопку пользовательского интерфейса Gitea, расположенную на верхней панели, чтобы получить доступ к странице Gitea.  
2)Войдите на сервер Gitea, используя имя пользователя jon и пароль Jon_pass123.  
3)После входа в систему найдите репозиторий Git с именем sarah / story-blog и создайте его под именем пользователя jon.
```
### Solution3
```bash
-
```



# 4: 
### Problem2
```text
```

### Solution2
```bash

```



# 5: 
### Problem2
```text
```

### Solution2
```bash

```
