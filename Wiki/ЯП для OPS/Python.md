---
Owner: Blossom Dude
Last edited time: 2023-12-15T15:18
Created time: 2023-12-15T14:33
---
==Стандартные утилиты python==

==pyenv== - версии python

==virtualenv== - виртуальные среды

==pip== - зависимости

  

==PIP==

Является менеджером пакетов для python. pip2 или pip3 для каждой версии python.

`pip -V` - версия

`pip3 install *pkg_name*==1.1.1` - установить пакет с версией

`pip3 show *pkg_name*` - информация о пакете, где он установлен

- `pip3 install -r requirments.txt` - установить все пакеты которые указаны в файлe requirments.txt
    
    ```Bash
    Flask
    Jinja2
    PyLibName
    PyDependenciesName
    ```
    

`pip3 uninstall *pkg_name*` - удаление пакета.