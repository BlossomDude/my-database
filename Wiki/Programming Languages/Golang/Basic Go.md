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
- [[#Memory]]
- [[#Input Output]]



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
|         |                                                  |                                                                      |
|         |                                                  |                                                                      |

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