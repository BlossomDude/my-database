---
Owner: Blossom Dude
Last edited time: 2024-01-22T12:31
Created time: 2024-01-21T18:18
---
`Terraform` - IaC инструмент с открытым исходным кодом.
	Terraform позволяет описывать и управлять инфраструктурными ресурсами, такими как виртуальные машины, сети, хранилища данных и другие, используя декларативный язык конфигурации - HCL(HashiCorp Configuration Language).

- Каждый объект которым управляет `terraform` называется ресурсом.
- `Terraform` записывает состояние инфраструктуры в файл `terraform.tfstate` так, как оно выглядит на текущий момент в реальном мире. И на основании этого файла он определяет какие изменения и для какого ресурса нужно выполнить.
- Он может импортировать созданные ресурсы.


---
## Фазы работы

Terraform Работает в трех фазах:
- `Init` - Инициализация проекта и определения провайдеров.
- `Plan` - Разработка плана для достижения целевого состояния. 
- `Apply` - Применение изменений в окружение.
	Если по какой-то причине окружение изменит состояние, то при следующей фазе `apply` - terraform приведет все в порядок.

Простой рабочий процесс terraform состоит из 4 шагов:
- Создайте файл конфигурации
- `terraform init` - Проверит текущую конфигурацию и наличие изменений.
	Команда проверит используемые провайдеры и загрузит необходимые плагины для успешной работы. 
- `terraform plan` - Покажет что terraform выполнит при применении текущей конфигурации. Аналог команды `diff`.
- `terraform apply` - Применит текущую конфигурацию.

---
## HCL Basics

Пример создания локального файла:
```
resource "local_file" "hello_resource" {
	filename = "/home/test.txt"
	content  = "Hello terraform!"
}
```

Имя блока:
	`resource` - означает что далее будет определение какого-либо ресурса.
Тип ресурса:
	`local_file`
	- `local` - provider - по сути "поставщик" ресурса.
	- `file` - тип ресурса
	`hello_resource` - имя ресурса
Аргументы/Параметры:
	`filename` -  имя создаваемого файла
	`content` - содержимое файла

---

## Plugins

`registry.terraform.io` - registry с плагинами по умолчанию

По умолчанию устанавливаются последние версии плагинов.


---

## Configuration directory

Configuration directory -  директория с нашими конфигурационными файлами.

Существует список рекомендуемых названий файлов в нашей главной директории:

| file name      | purpose                                                   |
| -------------- | --------------------------------------------------------- |
| `main.tf`      | Главный файл конфигурации содержащий определение ресурсов |
| `variables.tf` | Определения переменных                                    |
| `outputs.tf`   | Содержит вывод ресурсов                                   |
| `provider.tf`  | Содержит в себе определение провайдеров                   |

---

## Variables

Для хранения переменных используется файл variables:
```tf
variable "filename" {
	default     = "/root/file.txt"
	description = "Переменная с именем файла"
	type        = string
}

variable "content" {
	default     = "/root/file.txt"
	description = "Переменная с содержимым файла"
	type        = string
}
``` 

А вот так мы можем их использовать в ресурсе:
```tf
resource "local_file" "file" {
	filename = var.filename
	content  = var.content
}
```

`default` - значение переменной
`description` - описание переменной (опц.)
`type` - тип переменной - (опц.)  По умолчанию переменная типа `any`
	`string` - строка
	`number` - число, положительное или отрицательное
	`bool` 
	`map` - мапа - `["a": "b", "c": "d"]`
	`list` - список, массив - `[1, 2, 3, 4]`
	`set` - как list но нельзя иметь повторяющиеся значения 
>[!info]
>Мы можем указывать тип переменных явно в списках:
>`list(number)`/`map(bool)`/`map(string)`

###### Переменная типа `list`:
Например у нас есть такая переменная:
```tf
variable "num" {
	default     = ["zero", "one", "two"]
	type        = list
}
```

То мы можем обратится к индексу переменной в нашей конфигурации:
```tf
resource "local_file" "file" {
	filename = "file_with_zero"
	content  = var.num[0]
}
```

###### Переменная типа `map`:
```
variable "strings" {
	type        = map
	default     = {
		"value1" = "Hello from value1"
		"value2" = "Hello from value2" 
	}
}
```

Теперь мы можем использовать значение по индексу:
```
resource "local_file" "file" {
	filename = "file_with_value"
	content  = var.strings["value2"]
}
```


###### Переменная типа `object`:

Данный тип переменной может содержать в себе множества значений:
```tf
variable "human" {
	type = object({
		name = string
		age  = number
		skin = string
		sex  = string
		docs = set(string)
	})

	defaulr = {
		name = "Nik"
		age  = 25
		skin = "black"
		sex  = "yes"
		docs = ["passport", "drive_license"]
	}
}
```

###### Переменная типа `tuple`:

Данный тип переменной аналогичен `list` но может содержать в себе значения разных типов:
```
variable {
	type = tuple([string bool number])
	default = ["hello", true, 228]
}
```
Должен содержать в себе ровное количество определенных элементов и в указанной последовательности