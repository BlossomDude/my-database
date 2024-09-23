


# 1: error-handling
### Problem
```text
The goal of this exercise is to consider the number of arguments passed to your program. If there is exactly one argument, print a greeting message. Otherwise print an error message and exit with a non-zero status.

---

Цель этого упражнения - подсчитать количество аргументов, переданных вашей программе. Если есть только один аргумент, выведите приветствие. В противном случае выведите сообщение об ошибке и завершите работу с ненулевым статусом.
```

### Solution
```bash
#!/usr/bin/env bash

if [[ $# == 1 ]]
then
  echo "Hello, $1"
else
  echo "Usage: error_handling.sh <person>"
exit 1
fi
```



# 2: two-fer
### Problem
```text
```

### Solution
```bash
#!/usr/bin/env bash

person=$1

if [[ -z $1 ]]
then
  person="you"
fi
```



# 3: raindrops
### Problem
```text
```

### Solution
```bash
#!/usr/bin/env bash

main () {
  (( $# == 1 )) || return 1
  (( $1 % 3 )) || sound+='Pling'
  (( $1 % 5 )) || sound+='Plang'
  (( $1 % 7 )) || sound+='Plong'
  echo "${sound:-$1}"
}

main "$@"
```



# 4
### Problem
```text
```

### Solution
```bash
```



# 5
### Problem
```text
```

### Solution
```bash
```