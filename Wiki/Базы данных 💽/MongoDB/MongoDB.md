---
Owner: Blossom Dude
Last edited time: 2024-01-18T12:02
Created time: 2023-12-18T16:47
---
> ==NoSQL, производительная, масштабируемая база данных.==

> [!important]  
> Для установки на CеntOS нужно добавлять репу.  
  

`/etc/mongod.conf` - Конфигурационный файл БД

## Команды
`mongosh` - подключиться к БД
`show dbs` - вывести БД
`show collections` - вывести коллекции
`use *db_name*` - создать бд и переключится на нее
`db.createCollection(”collection_name”)` - Создать коллекцию и присвоить имя

```Bash
## Данная команда bash добавит данные в коллекцию
db.сollection_name.InsertOne({
“nickname”:”Raphael”,
“age”:”12”,
“location”:”terrarium”
});
```

`db.collection.find("param":example)` - вывести все данные из коллекции и отфильтровать по паре ключ-значения