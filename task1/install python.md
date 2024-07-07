## Установим Python `3.11.9`

```bash
yum install wget -y
yum install -y devtoolset-7
yum install openssl-devel bzip2-devel libffi-devel sqlite-devel
yum groupinstall "Development Tools"
wget https://www.python.org/ftp/python/3.11.9/Python-3.11.9.tgz
tar xzf Python-3.11.9.tgz
cd "Python-3.11.9"
```
Так как openssl, которая поставляется вместе с centos 7 устарела и не работает с питоном, которому очень нужно использовать его для pip установим openssl последней версии
```bash
yum install perl-IPC-Cmd
wget https://www.openssl.org/source/openssl-3.0.14.tar.gz
tar -xvzf openssl-3.0.14.tar.gz
cd  openssl-3.0.14
./Configure --prefix=/usr/local/ssl --openssldir=/usr/local/ssl     '-Wl,-rpath,$(LIBRPATH)'
make
make install
```
Теперь добавим библиотеку в конфиг
```bash
vi /etc/ld.so.conf.d/usr-lib.conf
```
вставим следующие настройки
```
/usr/local/lib
/usr/local/lib64
```
перезагрузка конфига
```
ldconfig -v
```
Теперь комманда `/usr/local/ssl/bin/openssl version` должна показывать 3.0.14

Создадим symlink для openssl3 (необязательно)
```
ln -sf /usr/local/ssl/bin/openssl /bin/openssl3
```
В папку `/usr/local/lib/pkgconfig` добавим файл `openssl3.pc` [со слудующим содержимым](https://github.com/vloldik/devopspractice/blob/main/task1/external/openssl3.pc)
Внутри папки `Python-3.11.9` изменим файл на [совместимый с костылем configure файл](https://github.com/vloldik/devopspractice/blob/main/task1/external/configure)

*Пришлось изменить файл для добавления установленной библиотеки. Можно было ее перезаписать вместо стандартной, но возможны проблемы с сервисами, которые используют предыдущую версию*

Добавим библиотеку в LD_LIBRARY и продолжим установку
```bash
vi /etc/profile
```
Добавим следующие линии
```bash
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
export LD_LIBRARY_PATH=/usr/local/ssl/lib64:$LD_LIBRARY_PATH
```
```bash
source /etc/profile
./configure
make -j 4 
make altinstall
```
