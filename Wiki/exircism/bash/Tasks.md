


# 1: error-handling
### Problem
```text
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