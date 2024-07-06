## Install Python `3.11.9`

```bash
yum install wget -y
yum install -y devtoolset-7
yum install openssl-devel bzip2-devel libffi-devel sqlite-devel
yum groupinstall "Development Tools"
wget https://www.python.org/ftp/python/3.11.9/Python-3.11.9.tgz
tar xzf Python-3.11.9.tgz
cd "Python-3.11.9"
./configure
make -j 4 
make altinstall
```
