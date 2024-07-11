Подготовка на машине с интернетом
```bash
yum update -y
sudo yum install yum-utils
mkdir /root/clone-repo
```
Комманда для установки пакета в репозиторий
```
sudo yumdownloader --resolve --destdir=/root/clone-repo <пакет>
```
```
createrepo /root/clone-repo
```
