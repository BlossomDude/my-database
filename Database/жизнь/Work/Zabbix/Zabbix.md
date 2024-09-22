|   |   |   |   |
|---|---|---|---|
||**Container**|**Port**|ip|
|1|`zabbix-postgres`|———————|——————|
|2|`zabbix-server`|`10051:10051`|172.18.0.3|
|3|`zabbix-web`|`80:8080 443:8443`|172.18.0.4|

dsud-zab-mon | **172.19.90.202**

Docker network - **zabbix-net  
ZABBIX HOSTNAME  
**= dsud-zab-mon

**POSTGRES_PASSWORD** = zabbix  
  
**POSTGRES_USER** = zabbix