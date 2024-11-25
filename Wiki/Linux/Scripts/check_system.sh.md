``` bash
#Данная простая конструкция может использоваться в разных целях.
#В данном случае скрипт выводит список команд для проверки и траблшутинга системы.

#!/bin/bash
echo ""
echo "Select the command number to check the system:"
echo ""

select number in Htop Uptime Free Iotop Sar Ps stop
do case $number in
	Htop) htop ;;
        Uptime) uptime ;;
	Free) free -m ;;
        Iotop) iotop ;;
        Sar) sar ;;
        Ps) ps -aux | less ;;
	stop) break ;;
	*) echo Такой команды не существует ;;
esac
done
```
