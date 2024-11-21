[easy]
[[#1 error-handling]]
[[#2 two-fer]]
[[#3 raindrops]]
[[#4 Humming]]

[medium]

[hard]
# 1: error-handling
### Problem1
```text
The goal of this exercise is to consider the number of arguments passed to your program. If there is exactly one argument, print a greeting message. Otherwise print an error message and exit with a non-zero status.

---

Цель этого упражнения - подсчитать количество аргументов, переданных вашей программе. Если есть только один аргумент, выведите приветствие. В противном случае выведите сообщение об ошибке и завершите работу с ненулевым статусом.
```

### Solution1
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
### Problem2
```text
Your task is to determine what you will say as you give away the extra cookie.
If you know the person's name (e.g. if they're named Do-yun), then you will say:
>One for Do-yun, one for me.
If you don't know the person's name, you will say _you_ instead.
>One for you, one for me.

Here are some examples:

|Name|Dialogue|
|Alice|One for Alice, one for me.|
|Bohdan|One for Bohdan, one for me.|
|*empty*|One for you, one for me.|
|Zaphod|One for Zaphod, one for me.|

---

Ваша задача - определить, что вы скажете, раздавая лишнее печенье.
Если вы знаете имя человека (например, если его зовут До-юн), то вы скажете:
>One for Do-yun, one for me.
Если вы не знаете имени человека, вы будете говорить "_вы_" вместо этого.
>One for you, one for me.

Вот несколько примеров:

|Имя|Диалог|
|Алиса|Одно для Элис, одно для меня.|
|Богдан|Одно для Богдана, одно для меня.|
|*empty*|Одно для тебя, одно для меня.|
|Зафод|Одно для Зафода, одно для меня.|
```

### Solution2
```bash
#!/usr/bin/env bash

person=$1

if [[ -z $1 ]]
then
  person="you"
fi
```



# 3: raindrops
### Problem3
```text
Your task is to convert a number into its corresponding raindrop sounds.

If a given number:

- is divisible by 3, add "Pling" to the result.
- is divisible by 5, add "Plang" to the result.
- is divisible by 7, add "Plong" to the result.
- **is not** divisible by 3, 5, or 7, the result should be the number as a string.

---

Ваша задача - преобразовать число в соответствующие звуки капель дождя.

Если заданное число:

- делится на 3, добавьте "Pling" к результату.
- делится на 5, добавьте "Plang" к результату.
- делится на 7, добавьте "Plong" к результату.
- **не** делится на 3, 5 или 7, результатом должно быть число в виде строки.
```

### Solution3
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



# 4: Humming
### Problem4
```text
Calculate the Hamming distance between two DNA strands.

Your body is made up of cells that contain DNA. Those cells regularly wear out and need replacing, which they achieve by dividing into daughter cells. In fact, the average human body experiences about 10 quadrillion cell divisions in a lifetime!

When cells divide, their DNA replicates too. Sometimes during this process mistakes happen and single pieces of DNA get encoded with the incorrect information. If we compare two strands of DNA and count the differences between them we can see how many mistakes occurred. This is known as the "Hamming distance".

We read DNA using the letters C, A, G and T. Two strands might look like this:

GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
^ ^ ^  ^ ^    ^^

They have 7 differences, and therefore the Hamming distance is 7.

The Hamming distance is useful for lots of things in science, not just biology, so it's a nice phrase to be familiar with :)
```

### Solution4
```bash
#!/usr/bin/env bash

a="$1"
b="$2"
count=0

if [ $# -ne 2 ]; then
    echo "Usage: hamming.sh <string1> <string2>"
    exit 1
fi

if [ ${#a} -ne ${#b} ]; then
    echo "strands must be of equal length"
    exit 1
fi

for (( i=0;i<${#a};i++ )); do
    if [ ${a:$i:1} != ${b:$i:1} ]; then
        count=$((count + 1))
    fi
done

echo $count

```



# 5
### Problem5
```text
```

### Solution5
```bash
```