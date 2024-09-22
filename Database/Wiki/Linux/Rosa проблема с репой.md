залить образ в /dev/cdrom я по этой статье создал репозиторий с диска и все взлетело  
  
[https://linux-notes.org/kak-sozdat-yum-repozitorij-iz-cd-rom-diska-v-redhat-centos-fedora/](https://linux-notes.org/kak-sozdat-yum-repozitorij-iz-cd-rom-diska-v-redhat-centos-fedora/)

---

  

1) Я отключил стандартные репозитории Росы:  
Два файла в директории /etc/yum.repos.d -> reld.repo и reld-source.repo  
В каждом файле в строках со значением enabled нужно проставить 0:  
enabled=0  
2) Потом я создал новый файл CentOS-Base.repo, скопировал в него стандатную репу  
[отсюда](https://github.com/cloudrouter/centos-repo/blob/master/CentOS-Base.repo) и отключил в каждой строке этого файла проверку gpg ключа, потому-что была какаято беда с ними и я просто решил отрубить проверку ключей. Вот так в каждой строке:  
gpgcheck=0  
3) И на всякий случай подключил популярный центосовский репозиторий чтобы все пакеты находились:  
yum install epel-realese