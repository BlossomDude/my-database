
Netplan - это набор утилит для работы с сетью в ОС Linux

/etc/netplan/ - в данной директории находятся файлы с настройками интерфейсов.

Пример yaml файла с натройками интерфейса.
```yaml
network:
  ethernets:
    enp0s1:
      dhcp4: false
      dhcp6: false
      addresses:
        - 10.0.0.1/24
        - ffff:ffaf:abcd:abcd:abcd:abcd/64
	  nameservers: 
	    addresses:
	      - 8.8.8.8
	      - 8.8.4.4
	  routes:
	    - to: 1.1.1.1/24
	      via: 10.0.0.2
	    - to: default
	      via: 2.2.2.2
	    
    version: 2
```

 
##### Команды

`netplan` 
	`try`  - проверить текущую конфиграцию
	`apply` - применить текущую конфигурацию