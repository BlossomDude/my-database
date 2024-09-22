---
Owner: Blossom Dude
Last edited time: 2024-01-17T17:53
Created time: 2023-12-18T16:02
---
Установка, настройка, базово

`systemctl start mysqld` - запустить службу БД

`cat /var/log/mysqld.log` - логи БД, при создании можно узнать по логам временный пароль.

`mysql -u root -p` - зайди в субд

`ALTER USER “root”@”localhost” IDENTIFIED BY “newpass”;` - Создать новый пароль для рута. Рут сможет подключится только из под

`CREATE USER “username”@”host” IDENTIFIED BY “password”` - создать юзера

`localhost` как указано в команде. **_% - для любых хостов  
  
_**

`CREATE DATABASE *name*;` - создать БД

`USE *name*;` - использовать базу *name*

`INSERT INTO animals values ( “Toothless”,11,”terrarium” );` - вставить в таблицу animals 3 значения в столбцы по порялку как указано.

`mysqldump *db_name* > /dir/dest.txt` - создать дамп и направить вывод в файл

`GRANT` ==`(PERMISSION)`== `ON` ==`(DB.TABLE)`== `TO (user@hostname)` - выдать права

- ==Список прав==
    
    - ==CREATE== - Создание таблиц, баз, юзеров и тд.
    - ==DROP== - Удаление таблиц, баз, юзеров и тд.
    - ==INDEX== - Создание Индекса таблиц.
    - ==INSERT== - Вставить новую строку в таблицу.
    - ==UPDATE== - Обновить строку в таблице.
    - ==DELETE== - Удалить строку из таблицы.
    - ==SELECT== - Получить данные из таблицы.
    - ==ALTER== - Изменить столбцы таблицы.
    - ==GRANT== - Аозможномть раздавть привелегии.
    - ==ALL PRIVILEGES== - Полнуый доступ.
    - ==. - точка означает доступ ко всем базам==
    
      
    

`SHOW GRANTS` - вывести список разрешений