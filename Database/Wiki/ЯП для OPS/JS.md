---
Owner: Blossom Dude
Last edited time: 2024-02-21T17:46
Created time: 2023-12-15T13:04
---
==NodeJS==

Серверная среда для разработки на JS

  

`node app.js` - Запуск кода.

  

==NPM==

> [!important]  
> npm -это менеджер пакетов, который управляет модулями и зависимостями проекта.  

`npm search *``_template_``*` - cписок всех доступных пакетов

`npm install *pkg_name*` - установить пакет в /node_modules

`npm install *pkg_name*` -g - установить пакет глобально для всех проектов js.

> [!important]  
> node_modeles /any_package/package.json - информация о пакете  

  

Cоществуют три типа модулей:

- **модули ядра и встроенные**
    - ядро `node -e “console.log(require(’repl’)._builtinLibs)”`
    - встроенные `ls /usr/lib/node_modules/npm/node_modules`
- **сторонние** `npm list -g`
- **локальные (собственно разработанные)**