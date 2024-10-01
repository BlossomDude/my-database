---
Owner: Blossom Dude
Last edited time: 2024-01-24T12:12
Created time: 2024-01-24T11:04
---
==Target== - является очередным юнитом SystemD. Напоминаю что ==SystemD== [[Системы инициализации Linux]] состоит из юнитов (прим. ==service==). Таргеты позволяют объединить различные юниты в группу. Например, можно создать ==**target**== который будет запускать сразу несколько служб.

- Пример объединения служб ssh и apache в таргет-файле.
    
    ```Bash
    [Unit]
    Description=ssh apache target
    Requires=ssh.service
    Requires=apache2.service
    [Install]
    WantedBy=multi-user.target
    ```
    
    В примере выше мы создали **ssh-apache.target** и использовали для этого следующие параметры:
    
    - **Description** — описание юнита.
    - **Requires** — в этом параметре указывается юнит, который должен запуститься при запуске таргета. Если необходимо запускать несколько служб, то нужно использовать несколько таких параметров, как в нашем примере.
    - **WantedBy** — на каком загрузочном таргете запускать этот юнит. Про загрузочные таргеты будет написано чуть ниже.

> [!important]  
> Для автозапуска таргета используй systemctl enable target_name.target  

## ==Boot Targets== - Загрузочные таргеты

  

В линукс есть разные варианты(==таргеты==) и уровни (==runlevel)== загрузки. Каждый уровень ссылается на свой ==таргет==. Посмотреть их можно командой:

> [!important]  
> Именно благодаря таргетам - процесс systemD знает что именно запускать при загрузке системы.  

```Bash
root@dsud-nikolaev:~$ ls -l /lib/systemd/system/runlevel*.target
lrwxrwxrwx 1 root root 15 Nov 21 16:10 /lib/systemd/system/runlevel0.target -> poweroff.target
lrwxrwxrwx 1 root root 13 Nov 21 16:10 /lib/systemd/system/runlevel1.target -> rescue.target
lrwxrwxrwx 1 root root 17 Nov 21 16:10 /lib/systemd/system/runlevel2.target -> multi-user.target
lrwxrwxrwx 1 root root 17 Nov 21 16:10 /lib/systemd/system/runlevel3.target -> multi-user.target
lrwxrwxrwx 1 root root 17 Nov 21 16:10 /lib/systemd/system/runlevel4.target -> multi-user.target
lrwxrwxrwx 1 root root 16 Nov 21 16:10 /lib/systemd/system/runlevel5.target -> graphical.target
lrwxrwxrwx 1 root root 13 Nov 21 16:10 /lib/systemd/system/runlevel6.target -> reboot.target
```

Таким образом можно перейти ==в другой уровень== загрузки командой:  
  
`**systemctl isolate rescue.target**` **-** перейдет в режим восстановления

  

`systemctl get-default` - узнать текущий таргет.

  

- Пример файла ==default.target== - таргета по умолчанию
    
    ```Bash
    [Unit]
    Description=Graphical Interface
    Documentation=man:systemd.special(7)
    Requires=multi-user.target
    Wants=display-manager.service
    Conflicts=rescue.service rescue.target
    After=multi-user.target rescue.service rescue.target display-manager.service
    AllowIsolate=yes
    ```
    
    - ==**Requires=multi-user.target**== — при запуске этого юнита, будет запущен юнит ==**multi-user.target**==.
    - ==**Wants=display-manager.service**== — этот юнит ожидает запуска службы ==**display-manager.service**==, но не требует её запуска. То-есть графику он подождёт, но если она не установлена, то всё равно запустится.
    - ==**Conflicts=rescue.service rescue.target**== — параметр ==**Conflicts**== означает, что если указанный юнит выполняется, то этот не будет запущен. А режим ==**rescue.target**== — это режим восстановления, чем-то похож на Безопасный режим в Windows. Получается что если запущен режим восстановления (==**rescue.target**==), то нельзя запускать графический режим (==**graphical.target**==).
    - ==**After**== — по очерёдности юнит должен запускаться после указанных юнитов, но не требует их запуска.
    - ==**AllowIsolate**== — если этот юнит можно использовать в команде `**systemctl isolate <название таргета>**`, то здесь нужно поставить ==**yes**==. То есть, с помощью этого параметра, мы указываем что это именно ==**boot target**==.
    
      
    

  

### Загрузочные таргеты по умолчанию

|                                 |                                                                                                                                                                               |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ==poweroff====**.**====target== | выключение системы                                                                                                                                                            |
| ==rescue.target==               | режим восстановления. В нем запускается минимум сервисов и не запускается сеть. Необходим для восстановление работы системы. Похож на загрузку в безопасном режиме в Windows. |
| ==multi-user.target==           | многопользовательский режим.                                                                                                                                                  |
| ==graphical.target==            | многопользовательский режим с поддержкой графики. Но если графика не установлена, то этот таргет работает как **multi-user.target**                                           |
| ==reboot.target==               | перезагрузка.                                                                                                                                                                 |
| ==default.target==              | режим, который будет загружаться по умолчанию, является символической ссылкой на один из **boot tatget**.                                                                     |

> [!important]  
> После перезагрузки система загрузится именно в default.target. Если указать в качестве default.target — reboot.target, то получим постоянную перезагрузку. Не нужно так делать!!!