## Replication Controller & Replica Set

Replica Set - это контроллер, основная задача которого заключается в поддержании заданного количества реплик (параллельно работающих экземпляров) подов. Он автоматически создаёт или удаляет поды, чтобы соответствовать указанному в конфигурации количеству.

Данные объекты нужны для автоматического создания и поддержания требуемого количество реплик подов в кластере, что обеспечивает высокую доступность и отказоустойчивость.

- Replication Controller на данный момент считается старой версией Replica Set.

- Главное отличие в них, это то что Replica Set требует наличие блока selector. В то время как в RC вы можете указывать, а можете не указывать этот блок. 

- Selector позволяет благодаря сопоставлению меток создавать не только реплики пода который указан в блоке spec, но и уже существующих и будущих подов.   
