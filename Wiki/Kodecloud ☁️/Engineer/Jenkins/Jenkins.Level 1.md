# 1: 
### Problem
```text
The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:  
  

  

1. Install `jenkins` on jenkins server using `yum` utility only, and start its `service`. You might face timeout issue while starting the Jenkins service, please refer [this](https://www.jenkins.io/doc/book/system-administration/systemd-services/#starting-services) link for help.  
  

2. Jenkin's admin user name should be `theadmin`, password should be `Adm!n321`, full name should be `John` and email should be `john@jenkins.stratos.xfusioncorp.com`.  
  
---

Команда DevOps в xFusionCorp Industries приступает к настройке конвейеров CI / CD и решила использовать Jenkins в качестве своего сервера. Выполните задачу в соответствии с предоставленными требованиями.:


1. Установите jenkins на сервер jenkins, используя только утилиту yum, и запустите ее службу. При запуске службы Jenkins может возникнуть проблема с таймаутом, пожалуйста, обратитесь по этой ссылке за помощью.
2. Имя пользователя администратора Дженкина должно быть theadmin, пароль должен быть Adm! n321, полное имя должно быть John и адрес электронной почты должен быть john@jenkins.stratos.xfusioncorp.com.

Примечание:
1. Для выполнения этой задачи получите доступ к серверу Jenkins по SSH, используя пользователя root и пароль S3curePass от jump host.
2. После установки сервера Jenkins нажмите кнопку Jenkins на верхней панели, чтобы получить доступ к пользовательскому интерфейсу Jenkins, и следуйте инструкциям на экране, чтобы создать пользователя-администратора.
```

### Solution
```bash
# Следовал стандартной инструкции по установке Jenkins
```



# 2: Install plugins
### Problem
```text
The Nautilus DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task  
  
1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.  
2. Once logged in, install the `Git` and `GitLab` plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`.  
  
---

Команда разработчиков Nautilus недавно настроила сервер Jenkins, который они хотят использовать для некоторых заданий CI / CD. Перед этим они хотят установить некоторые плагины, которые будут использоваться в большинстве заданий. Пожалуйста, ознакомьтесь ниже с более подробной информацией о задаче

1. Нажмите на кнопку Jenkins на верхней панели, чтобы получить доступ к пользовательскому интерфейсу Jenkins. Войдите в систему, используя имя пользователя admin и пароль Adm! n321.
2. После входа в систему установите плагины Git и GitLab. Обратите внимание, что вам может потребоваться перезапустить службу Jenkins для завершения установки плагинов, при необходимости выберите перезапуск Jenkins, когда установка завершена и никакие задания не выполняются на странице установки / обновления плагина, т. е. в центре обновлений.
Примечание:
1. После перезапуска службы Jenkins дождитесь повторного появления страницы входа в Jenkins, прежде чем продолжить.
2. Для задач, связанных с изменениями веб-интерфейса, делайте снимки экрана, чтобы поделиться ими для ознакомления, или рассмотрите возможность использования программного обеспечения для записи экрана, такого как loom.com для документирования и совместного использования.
```

### Solution
```bash
# Просто установил два плагина
```



# 3: 
### Problem
```text
The Nautilus team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:  

1. Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login with username `admin` and password `Adm!n321`.
2. Create a jenkins user named `javed` with the password`LQfKeWWxWD`. Their full nme should match `Javed`.   
3. Utilize the `Project-based Matrix Authorization Strategy` to assign `overall read` permission to the `javed` user.   
4. Remove all permissions for `Anonymous` users (if any) ensuring that the `admin` user retains overall `Administer` permissions.   
5. For the existing job, grant `javed` user only `read` permissions, disregarding other permissions such as Agent, SCM etc.

---

Команда Nautilus интегрирует Jenkins в свои конвейеры CI/CD. После настройки нового сервера Jenkins команда разработчиков приступает к настройке доступа пользователей, выполните следующие действия: 

1. Нажмите на кнопку Jenkins на верхней панели, чтобы получить доступ к пользовательскому интерфейсу Jenkins. Войдите под именем admin и паролем Adm!n321. 
2. Создайте пользователя jenkins по имени javed с паролем Qfkewwxwd. Его полное имя должно совпадать с Javed. 
3. Используйте стратегию авторизации Matrix, основанную на проекте, для назначения общего разрешения на чтение пользователю javed. 
4. Удалите все разрешения для анонимных пользователей (если таковые имеются), гарантируя, что пользователь admin сохранит за собой общие права администрирования. 
5. Для существующего задания предоставьте пользователю javed только права на чтение, игнорируя другие разрешения, такие как Agent, SCM и т.д.
```

### Solution
```bash
-
```



# 4: 
### Problem
```text
-
```

### Solution
```bash
-
```



# 5: 
### Problem
```text

```

### Solution
```bash

```
