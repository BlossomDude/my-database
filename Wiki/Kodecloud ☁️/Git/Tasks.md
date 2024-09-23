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