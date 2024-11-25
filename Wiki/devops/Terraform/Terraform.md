---
Owner: Blossom Dude
Last edited time: 2024-01-22T12:31
Created time: 2024-01-21T18:18
---
```Shell
resource "local_file" "cat" {       # тип блока "тип ресурса" "имя ресурса"
  filename          = "/root/cats.txt"
	content           = "My cat is a sniper"
# sensitive_content = "My cat is a sniper"   # Данный контент не будет отображаться при apply и plan
  file_persmission  = "0700" 
}
```

`terraform`

`apply` - Отобразит план и выполнит его, создаст ресурс.

`destroy` - удалит полностью ресурс

`init` - проверит файл конфигурации и инициализирует его.

`plan` - Покажет действия которые будут выполнены для создания ресурса.

`show` - отображает сведения о созданном ресурсе.