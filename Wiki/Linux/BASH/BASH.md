---
Owner: Blossom Dude
Last edited time: 2024-04-05T10:24
Created time: 2023-08-11T17:27
---
> [https://habr.com/ru/companies/ruvds/articles/325522/](https://habr.com/ru/companies/ruvds/articles/325522/)

- [[#Арифметические действия:|Арифметические действия:]]
- [[#Циклы|Циклы]]
- [[#Условия IF &&|Условия IF && ]]
- [[#Операторы файлов|Операторы файлов]]
- [[#Параметры|Параметры]]

### Арифметические действия:

`bc` - встроенный калькулятор

---
Можно использовать команду `echo` :

- `echo $((X+Y))`
- `echo $((X-Y))`
- `echo $((X*Y))`
- `echo $((X/Y))`

Инкремент и декремент:

- `echo $((X++))`
- `echo $((X--))`
- `echo $((++X))`
- `echo $((--X))`
- `var=$((var + 1))`

> [!important]  
> X+=$Y - не арефмитическое действие а конкатенация строк  

  

## Циклы

### Цикл for

Стандартная запись:

```Bash
#!/bin/bash
for mission in $(cat list_of_missions.txt)
do
echo create_and_launch_rocket $mission
done
```

Запись как в java:

```Bash
#!/bin/bash
num=0

for ((i=0;i<5:i++))
do
echo $i
done
```

  

### Цикл while

```Bash
#!/bin/bash
to_number=$1
number=0

while [ $number -lt $to_number ]
do
echo $(( number++ ))
done
```

## Условия IF && ||

### AND OR

`[x] && [y]` - х и у

`[x] || [y]` - x или у

  

Использование двойных скобок открывает более сложные конструкции.

`[[x -gt 5 && x -lt 10]]` - х больше 5 и меньше 10

`[[x -gt 5 || x -lt 10]]` - х больше 10 или меньше 5

### IF ELSE etc

  

**if-then**

```Bash
#!/bin/bash

if [ $rocket_status = "failed" ] 
then
	echo "failed"
else
	echo "good"
fi
```

  

**if-elif**

```Bash
#!/bin/bas

user=dimm

if grep $user /etc/passwd; then
	echo "The user $user exists"
elif ls /home; then
	echo "The user doesn't exist, but there is a directory under /home"
else 
	echo "The user $user doesn't exist"
fi
```


**Case**

```bash

while true
do
  echo "1. Add"
  echo "2. Subtract"
  echo "3. Multiply"
  echo "4. Divide"
  echo "5. Average"
  echo "6. Quit"
  read -p "Enter your choice: " choice

  case $choice in
    1)
        read -p "Enter Number1: " number1
        read -p "Enter Number2: " number2
        echo Answer=$(( $number1 + $number2 ))
        ;;
    2)
        read -p "Enter Number1: " number1sum / 2" | bc -l
        read -p "Enter Number2: " number2
        echo Answer=$(( $number1 - $number2 ))
        ;;

    3)
        read -p "Enter Number1: " number1
        read -p "Enter Number2: " number2
        echo Answer=$(( $number1 * $number2 ))
        ;;
    4)
        read -p "Enter Number1: " number1
        read -p "Enter Number2: " number2
        echo Answer=$(( $number1 / $number2 ))
        ;;
    5)
        read -p "Enter Number1: " number1
        read -p "Enter Number2: " number2
        sum=$(( $number1 + $number2 ))
        echo Answer=$(echo "sum / 2" | bc -l)
    6)
        break
        ;;
  esac

done

```

> Если используются операторы сравнения то выражение нужно заключать в кв. скобки.  
> Т  
> **ак же обязательно соблюдать пробелы.  
>   
>   
> `[` `“x”` `=` `“y”` `]` - х равен у  
>   
> `[` `“x”` `!=` `“y”` `]` - х не равен у  
>   
> `[` `“x”` `==-eq==` `“y”` `]` - х равен у  
>   
> `[` `“x”` ``` `` `==-ne==` `` ``` `“y”` `]` - х не равен у  
>   
> `[` `“x”` ``` `` `==-gt==` `` ``` `“y”` `]` - х больше чем у  
>   
> `[` `“x”` ``` `` `==-lt==` `` ``` `“y”` `]` - x меньше чем у

- Двойные кв. скобки
    
    ![[/Untitled 5.png|Untitled 5.png]]
    

  

### Операторы файлов

`[ -f FILENAME ]` - true если файл существует

`[ -e FILENAME ]` - true если файл существует

`[ -d FILENAME ]` - true если файл существует и он каталог

`[ -s FILENAME ]` - true если файл существует и разрер больше 0.

`[ -x FILENAME ]` - true если файл исполняемый

`[ -w FILENAME ]` - true если в файл можно писАть

`[ -z STRING ]` - True если строка не существует

`[ -n STRING ]` - true если строка существует

```Shell
https://exercism.org/tracks/bash/exercises/raindrops#:~:text=a%20factor%2C%20add%20%27-,Plong,-%27%20to%20the%20resul
```

## Параметры

Передача параметров `./script param1 param2 …`

`$0` - имя скрипта

`$1`- первый параметр

`$2`- второй параметр - и так далее, вплоть до переменной `$9`, в которую попадает девятый параметр.

`$#` - содержит в себе колличество параметров переданных в скрипт.

`${!#}` - содержит в себе последнюю переменную.

`$*` - содержит все параметры, введённые в командной строке, в виде единого «слова».

`$@` - параметры разбиты на отдельные «слова». Эти параметры можно перебирать в циклах.

  

> Если нужно больше 9 параметров, то в скрипте необходимо обращаться к переменной вот так `${10}`

  

Запустим `./script 5 10 15`

```Bash
#!/bin/bash
echo $0
echo $1
echo $2
echo $3
```

Вывод:

![[Untitled 1.png]]

  

> Если необходимо чтобы один параметр содержал например предложение или набор цифр то необходимо параметр заключать в кавычки:  
>   
> `./script “hello 1 2 3 4 5” $1 = hello 1 2 3 4 5`

- Передача параметров в команду - ==Heredog==
    
    ```Bash
    #Любое слово
    cat << PARAMS      
    file_string.txt     
    filw.txt
    fail.txt
    ...
    PARAMS              #Обозначает окончание параметров
    ```
    
- Передача параметра в виде одной строки - ==Herestring==
    
    ```Bash
    \#herestring
    cat <<< "Передача строги с помощью трех знаков меньше"
    ```
    

  
```Bash
cd /var/log/apps/

echo -e " Log name \tGET \tPOST \tPATCH \tHEAD \tPUT \tDELETE "
echo -e "---------------------------------------------------------------"

for i in $(cat /home/moon/apps.txt)
do
get=$(cat ${i}_app.log | grep "GET" | wc -l)
post=$(cat ${i}_app.log | grep "POST" | wc -l)
patch=$(cat ${i}_app.log | grep "PATCH" | wc -l)
head=$(cat ${i}_app.log | grep "HEAD" | wc -l)
put=$(cat ${i}_app.log | grep "PUT" | wc -l)
delete=$(cat ${i}_app.log | grep "DELETE" | wc -l)

echo -e " manager \t${get} \t${post} \t${patch} \t ${head} \t${put} \t${delete}"
done
```