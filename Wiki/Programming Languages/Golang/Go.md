---
Owner: Blossom Dude
Last edited time: 2023-12-15T15:34
Created time: 2023-12-15T15:31
---
# Basic GoLang

- [[#Command line]]
- [[#Comment]]
- [[#Data types]]
- [[# Declaring Variables]]
- [[#Variables Memory]]
- [[#Converting Types]]
- [[#Input Output]]
- [[#Operators]]
- [[#If-else, switch-case statements]]
- [[#For loop]]
- [[#Arrays]] | [[#Slices]] | [[#Map]]
- [[#Functions]]
- [[]]
- 


---

#### Command line
`go env` - internal variables
`go run main.go` - run main function


---

#### Comment
```go
// one-line comment

/*
multi line
comment
*/
``` 


---

#### Data types

Есть разные типы данных. Разные типы нужны для разных задач и операций. Каждый тип занимает определенное количество памяти.

| Type    | Examples                                         | Desc                                                                 |
| ------- | ------------------------------------------------ | -------------------------------------------------------------------- |
| String  | `"Hello" "1.332 Hello"`                          |                                                                      |
| Integer | `1 -837 8373739`                                 |                                                                      |
| Float   | `1.2 1.0000001 1234.1`                           |                                                                      |
| Boolean | `true or false`                                  | Standart boolean type.                                               |
| Arrays  | `[1, 2, 3, 4] ["foo", "bar"] [1.1, 9.03, 543.5]` | Array will be only one type, for example only string or only integer |
| Slices  |                                                  |                                                                      |
| Maps    | `"key" -> "value" 1 -> 100 "x" -> "87"`          | key-value type                                                       |


---

#### Declaring Variables

Объявить переменные можно разными способами:
```go
func main() {
	var str,str1 = "str","str1" // Объявление переменных одного типа в одной строке
	
	var (                       // Объявление разных переменных в блоке var
	str string = "hello
	num int = 1213454)
	
	num := 67                   // Короткое объявление переменной


}
```

##### Constant

Объявить константу можно следующим образом:
```go
const a string = "Constant"
// Или не указывать тип
const b = "Constant"
``` 

Константу обязательно нужно объявить и присвоить ей значение. Ее нельзя перезаписывать.

---
#### Variables Memory

- Integer - просто `int` занимает 32 бита на 32-битной системе и 64 на 64-битной.
	- `uint8, uint16, uint32, uint64` - unsigned integer - только положительные числа
	- `int8, int16, int32, int64` - signed integer - положительные и отрицательные числа
		- Каждая переменная занимает 8,16,32 и 64 бита 

- Float - `float32, float64` - занимают 32 и 64 бита в памяти.

- String - `string` - строка занимает 16 байт в памяти. т.е. 128 бит.

- Boolean - `bool` - занимает 1 байт в памяти
---

#### Converting Types

Для конвертации типов можно использовать такую конструкцию
```go 
func main() {
	// Из Integer в float
	var d int = 10
	var f float64 = float64(d)
	fmt.Printf("%.2f", f)
	// Output: 10.00
	
	// Из Float в Integer
	var f float64 = 65.32
	var i int = int(f)
	fmt.Printf("%v", i) 
}
```

Так же для конвертации типов можно использовать функции из пакета `strconv`
Функция `strconv.Atoi()` преобразовывает String в Int
Функция `strconv.Itoa()` преобразует Int в String:
```go
func main() {
	// Из Integer в String
	var i int = 1
	var s string = strconv.Itoa(i) 
}
```


---
#### Input Output

##### Output
Для стандартного вывода используется библиотека `fmt`.
```go
import "fmt"

main() {
	fmt.Println("Hello World!")
}
```

Для форматирования строк используется функция `Printf`:
```go
import "fmt"

main() {
	var str string = "Its Variable"
	var integer int = 1 
	fmt.Printf("var: %v", str)         // Подставление переменной. Default format
	fmt.Printf("string: %d", integer)  // Подставление integer
}
```
Еще некоторые спецификаторы:
- `%c` - character
- `%s` - plain string
- `%t` - true or false
- `%f` - floating number
- `%5.2f` - ширина 5, точность 2: `90.98` 
	Пример: `%.2f` - ширина по ум. длина 2. `%2.2` -  ширина и длина 2
- `%T` - Тип переменной
	- Так же тип переменной можно определить функцией  `reflect.TypeOf()`
##### Input

Для ввода используется функция `fmt.Scanf()`
```go
func main() {
	var s string
	var d int
	fmt.Scanf("%s", &s)        // Запишет пользовательский ввод в переменную name 
	fmt.Scanf("%s %d", &s, &d) // Записать две переменные 
}
```

Так же функция Scanf возвращает два значения. 
	`Count` - содержит кол-во переданных значений.
	`Err` - содержит ошибку
```go
count, err := fmt.Scanf("%s %d", &s, &d)
```

---


#### Operators

Comparison operators:
```
== != < <= > >=
```

Arithmetic operators:
```
+ - * / 
% - модуль
++ - инкремент
-- - декремент
```

Logical operators:
```
&& - AND
	((x > y) && (x > z))
|| - OR
	((x > y) || (x > z))
! - NOT
	!(x > y)
```

Assignment operators
```
= *= /= += -= %= 
```

Bitwise operators
```
& - AND - берет равные биты
12 & 25
  0 0 0 0 1 1 0 0
& 0 0 0 1 1 0 0 1 
-----------------
  0 0 0 0 1 0 0 0 = 8
  
| - OR - берет те где есть хотя бы 1 бит
12 | 25
  0 0 0 0 1 1 0 0
| 0 0 0 1 1 0 0 1 
-----------------
  0 0 0 1 1 1 0 1 = 29

^ - XOR - берет только где 1 бит
12 ^ 25
  0 0 0 0 1 1 0 0
^ 0 0 0 1 1 0 0 1 
-----------------
  0 0 0 1 0 1 0 1 = 21
  
<< - left shift
  212 << 1
  11010100 -> 11010100(0) - добавляется ноль справа 212 стало 424 
  

>> - right shift
  212 >> 2
  110101(00) -> (00)1101010 - добавляется два ноля слева 212 стало 53
```
---

#### If-else, switch-case statements

##### `if-elif-else`

```go
if condition {
//  do any
} else if condition {
// do any
} else {
// do any
}
```

##### `switch-case`

Ключевое слово  `falltrough` означает перейти к следующему блоку даже если условие `case` не выполняется. 

```go
switch(var) {
  case "value":
    // do any
  case "other_value", "other":
    //do any
    falltrough    
  case "value_1"
    //do any
  default:
    //do any
}

// Так же не обязательно указывать переменную в switch
// Можно просто в каждом case указывать новое условие
switch {
  case a+b == 10:
    // do any
}
```

#### For loop

Цикл for можно использовать с любым количеством блоков, можно использовать просто `for {}` - это будет бесконечным циклом
```go
for i := 1; i <=3; i++ {
    fmt.Println("Hello World")
    
    if i == 4 {
        break  
    }
    if i < 4 {
        continue
    }
}
```
---

#### Arrays

`Array` - Это коллекция одинаковых элементов, которые хранятся в смежных ячейках памяти.

- **Размер массива не изменяем.** После объявление массива нельзя изменить его размер.
- Обязательно все элементы должны быть одного типа данных

Имеет два свойства:
- `lenght` - длина массива
	`len(some_array)` - функция выдает значение длины массива
- `capacity` - количество элементов которые может содержать массив

Пример объявления массивов:
```go
// Объявление переменной
var fruits [3] string

// Объявление и присвоение значений
var numbers [5] int = [5]int{0, 1, 2, 3, 4}

// Простое определение:
vegetables := [2]string{"tomato", "cucumber"}

// Не указываем кол-во элементов. Компилятор сам определит.
grades := [...]int{5, 4, 5, 3, 5}

// Многомерный массив - 2D массив
grades := [3][2]int{{2, 4}, {5, 4}, {1, 1}} 
```

Проход по массиву:
По мимо обычного перебора массива используя `i < len(array)` можно использовать range:
```go
grades := [5]int{5, 4, 4, 3, 4} 

for index; element := range grades {
	fmt.Println(index, "=", element)
}
```
---

#### Slices
Slice - это массив - ссылка на базовый массив, т.е. его срез.

- В отличии от массива можно добавлять разные типы переменных, они так же могут добавляться и удаляться

Slice имеет три основных компонента:
- `pointer` - указатель, используется для указания на первый элемент массива, который доступен через этот слайс.
- `lenght` - количество элементов которых содержит слайс. Функция `len()`
- `capacity` - это количество элементов в базовом массиве, начиная с первого элемента в базовом массиве. Функция `cap()`.
Объявление slice:
```go
grades := []int{10, 20, 30} //не указываем кол-во элементов
```

Как создать слайс из базового массива:
```go
arr := [10]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
slice := arr[1:8] 
// Возьмет элементы с индекса 1(включительно) по индекс 8 (не включительно).
```

Примеры создания слайса из базового массива:
```go
slice := arr[0:9] // С начала по 8
slice := arr[4:] // C 4 до конца
slice := arr[:7] // C начала по 6
slice := arr[:] // Все
```

Создание(инициализация) пустого слайса используя функцию `make`.
```go
slice := make([]int, 5, 8) // Принимает два числа. Lenght и Capacity(Опционально)
```

Мы можем добавлять новые значения в slice используя метод append:
```go
slice = append(slice, 100, 343, 322) // Добавили три новых значения
slice = append(slice, another_slice...) // Соеденить два слайса
```

>[!important]
> Если мы увеличили длину массива функцией `append()` и привысили `capacity`, то `capacity` будет увеличен и произойдет перераспределение памяти:
> Старый массив скопируется в новый массив с большей ёмкостью

Копирование одного среза в другой:
```go
copy(dest_slice, src_slice)
```
---
#### Map

- Хранит данные в паре ключ- значение.
- Работает по принципу hash-tables
- Нельзя добавить данные в nil-map (пустую мапу)

Определение map:
```go
// Создание мапы с ключом string и значением string
var langs map[string]string 

// Создание не пустой мапы 
langs := map[string]string{"en": "English", "ru": "Russian"}

// Используя make
langs := make(map[string]int)
```

- Для получения длины и capacity так же можно использовать встроенные функции `len()` И `cap()`

При получении значения по индексу возвращается два значения, значение самого индекса и true если ключ-значение есть и false если не нашлось значения по индексу:
```go
langs := map[string]string{"en": "English", "ru": "Russian"}
rus := langs["ru"] // rus = Russian
eng, found := langs["en"] // eng = English, found = true
frn, found := langs["fr"] // frn = "", found = false
```

Для добавления в конец мапы новой пары ключ-значения, просто определите её:
```go
langs := map[string]string{"en": "English", "ru": "Russian"}
langs[it] = "Italian"
// langs = "en": "English", "ru": "Russian", "it": "Italian"
```
Таким же способом можно перезаписать существующее значение.

Для удаления элемента используйте функцию `delete()`:
```go
delete("en", "English")
```

---
#### Functions

```go
// Как объявляется функция
func <func_name>(params) <return_type>{}

//Объявление функции с двумя переменными возврата
//Укажите имена возращаемых переменных, чтобы не объявлять их внутри функции и
// и для того чтобы не указывать их в return как ниже:
func operation(a int, b int) (sum int, div int){
	sum = a + b
	div = a - b
	return
}

// Функция принимающая неопределенное колличество параметров
// Так же можно использовать вместе с другими переменными(a string, b ...bool)
func getSum(numbers ...int) (sum int) {
	for _, value := range numbers {
		sum += value
	} 
	return
}

```

Анонимная функция:
```go
x := func(a int, b int) int {
	return a * b
}
fmt.Println(x(2, 2))
// Output: 4

x := func(a int, b int) int {
	return a * b
}(10, 10)
fmt.Println(x)
// Output: 100
```

Рекурсивная функция:
```go
func factorial(n int) (fac int) {
    if n == 1 {
        return 1
    }
    return n * factorial(n-1)
}
```

High-order функция:
Функция которая либо принимает в качестве параметра функцию, либо возвращает функцию:
```go
func printResult(radius float64, calcFunction func (r float64) float64) {
	result := calcFuntion(radius)
	fmt.Println("Result:", result)
}
```

---

#### Pointers

`Pointers` - это указатель, переменная которая хранит адрес в RAM другой переменной.

Каждый раз во время выполнения программы, при объявлении переменной - переменная получает свой pointer. 

Чтобы получить адрес переменной поставьте перед не амперсанд(&), а чтобы получить значение которое хранится по адресу, то поставьте перед адресов звездочку.
```go
x := 10
fmt.Println(&x)
// Result: 0xc00000a0e8 

y := &x
fmt.Println(*y)
// Result: 10
```

Объявление переменной типа `Pointer`:
```go
var my_pointer *int = &some_var
```

Мы можем поменять значение переменной не используя ее, а просто поменять значение благодаря указателю:
```go
s := 1
ps := &s            //ps = 0xc00000a0e8 
*ps = 2             //0xc00000a0e8 = 2
fmt.Println(s)
// Output: 2
```

Так же благодаря указателям мы можем изменять в функциях значения переменных из других функций, передав им переменную типа `Pointer`:
```go
func modify(s *string){
	*s = "world" 
}

func main() {
	a := "hello"
	fmt.Println(a)
	modify(&a)
	fmt.Println(a)
}
// hello
// world
```

>[! important]
>В случае с slice И map не нужно указывать что мы передаем переменную pointer, так как срезы и map'ы сами по себе являются ссылками. Т.е. при изменении значения индекса их значение поменяется даже если не ссылаться на ссылку в памяти
