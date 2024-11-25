---
Owner: Blossom Dude
Last edited time: 2023-08-24T14:50
Created time: 2023-08-22T19:46
---
- `netstat -aptu`
    - -a - список всех портов
    - -p - укажет пид и имя процесса
    - -t - только tcp
    - -u - только udp
- `netstat -s` - статистика всех портов
- `netstat -i` - список интерфейсов

  

`netstat -lnptux`

- l все открытые порты (LISTEN)
- -t по протоколу TCP
- -u по протоколу UDP
- -x по протоколу UNIX Socket
- -n без резолва IP/имён
- -p но с названиями процессов и PID-ами