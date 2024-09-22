
- [[#STRATUS DC:|STRATUS DC - Данные об инфраструктуре]]

- [[#2:|2:Group Creation and User Assignment]]
	- [[#Problem:|Problem]]
	- [[#Solution:|Solution]]
- [[#3:|3]]

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
```

### Solution:

ssh user@hostname
sudo groupadd nautilus_sftp_users
id kano
sudo useradd kano 
sudo usermod -a -G nautilus_sftp_users kano


# 3: Linux User Setup with Non-Interactive Shell

To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell. Here's your task:  
  

  

Create a user named `kareem` with a non-interactive shell on `App Server 2`.