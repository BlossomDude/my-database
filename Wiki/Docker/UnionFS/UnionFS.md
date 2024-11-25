---
Owner: Blossom Dude
Last edited time: 2024-07-08T11:20
Created time: 2023-08-07T20:31
---
**UnionFS** - исходная объедененная файловая система. На данный момент не поддерживается.  
  

**aufs -** альтернативная версия исходной файловой системы UnionFS c добавлением множества вспомогательных функций. Использовалась по умолчанию в Docker на Linux, однако со временем была заменена на OverlayFS. По сравнению с другими файловыми системами она имеет ряд преимуществ.

  

**OverlayFS** — была включена в ядро Linux Kernel, начиная с версии 3.18 (26 октября 2014 года). Данная файловая система используется по умолчанию драйвером overlay2 Docker (это можно проверить, запустив команду docker system info | grep Storage). Данная файловая система в целом обеспечивает лучшую, чем aufs, производительность и имеет ряд интересных функциональных особенностей, например функцию разделения страничного кэша.

![[/Untitled 12.png|Untitled 12.png]]

  

```JSON
{
  "storage-driver": "devicemapper"
}
```