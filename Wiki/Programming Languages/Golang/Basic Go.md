---
Owner: Blossom Dude
Last edited time: 2023-12-15T15:34
Created time: 2023-12-15T15:31
---
# Basic GoLang

### Command line
`go env` - internal variables
`go run main.go` - run main function
##### Comment
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
##### Memory

- Integer - просто `int` занимает 32 бита на 32-битной системе и 64 на 64-битной.
	- `uint8, uint16, uint32, uint64` - unsigned integer - только положительные числа
	- `int8, int16, int32, int64` - signed integer - положительные и отрицательные числа
		- Каждая переменная занимает 8,16,32 и 64 бита 

- Float
	- `float32, float64` - занимают 32 и 64 бита в памяти.

- String - строка занимает 16 байт в памяти. т.е. 128 бит.

- Boolean - `bool` - занимает 1 байт в памяти

---
#### Input Output

Для стандартного вывода используется библиотека fmt.
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
	fmt.Printf("var: %v", str)         // Подставление переменной
	fmt.Printf("string: %d", integer)  // Подставление integer
}
```
