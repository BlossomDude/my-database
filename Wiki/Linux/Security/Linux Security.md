
Тема "Безопасность Линукс" является обширной и включает в себя:
- **Access Controls - Контроль доступа**
- **PAM - Pluggable Authentication Model**
- **Network Security**
- **SSH Hardening**
- **SELinux**
- **Many More**

Типы пользователей:
- User Account - обычные пользователи
- SuperUser Account - root, его UID = 0
- System Accounts - Системные пользователи, <100 или между 500 и 1000
- Service Accounts - Сервисные пользователи - напр. nginx.

файл sudoers
username ALL=(ALL:ALL) ALL
|-------------|--------|-------|
|-------------|--------|-------|>Какую команду может использовать пользователь
|-------------|--------|>От имени какого пользователя и групп можно вводить команды
|-------------|>На каком хосте можно использовать команды
|>Для какого пользователя, или группы (прим. %group, <- начинается с "%")