---
Owner: Blossom Dude
Last edited time: 2023-12-15T12:59
Created time: 2023-12-14T17:30
---
### Команды в bash

- `javac *MyClass.java*` - скомпилировать исходный код и создает MyClass.class
- `jar cfe <app.jar> <some_class> <Myclass.class> <Parser.Class>` - Команда для создания jar архива, указывается класс который запустится самый первый и все остальные входячие в архив файлы
- `javadoc -d` `doc` `MyClass.java` - создает html с документацией кода.

  

JDK - набор иснтрументов для разработке на java включает в себя:

- JDB - отладка приложений
- Javadoc - документоравние кода
- Javac - компилятор
- jar - архивация кода и связанных библиотек
- JRE Java Runtime Enviroment) - Среда для запуска кода
    - java - загрузчик для запуска приложений
`jdk_ver_directory/bin` - посмотреть все бинарные файлы JDK


`.WAR` - веб-архив
  

> [!important]  
> JAR.jar - в данном архиве содержаться файлы джава классов - готовое приложение, используется для сжатия и распространения единым файлом.jar cfe app.jar MyClass Myclass.class Parser.Class Worker.Class - Команда для создания jar архива, указывается класс который запустится самый первый и все остальные входячие в архив файлы.Так же при создании создается файл манифест где указываются метаданные jar архива.  

  
`mvn package` - сбилдить
`gradlew build` - сбилдить
`gradlew run` - запустить
`ant compile jar` - сбилдить
`ant` - команда для запуска файла конфигурации ant