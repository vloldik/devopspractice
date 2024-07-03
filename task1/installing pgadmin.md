*using vm with ip 192.168.0.124*
## Add repo
```bash
rpm -i https://ftp.postgresql.org/pub/pgadmin/pgadmin4/yum/pgadmin4-redhat-repo-2-1.noarch.rpm
```
## looking again at site and see 
IMPORTANT NOTE: Packages for distributions that are no longer supported will be archived to https://pgadmin-archive.postgresql.org/, from where they can be manually downloaded. The archive site cannot be used as a repository!
## removing broken repo and installing manually 
```bash
yum remove pgadmin4-redhat-repo-2-1.noarch
```
- updating python (python > 3.12.2) *may be not needed*
```bash
yum install wget -y
yum install -y devtoolset-7
yum install openssl-devel bzip2-devel libffi-devel sqlite-devel 
wget https://www.python.org/ftp/python/3.12.4/Python-3.9.18.tgz
tar xzf Python-3.9.18.tgz
cd "Python-3.9.18"
./configure
make -j 4 
make altinstall
```
- installing pgadmin from [archive](https://pgadmin-archive.postgresql.org/pgadmin4/yum/redhat/rhel-7Server-x86_64/index.html)
- we need 3 packages:
  - [server](https://pgadmin-archive.postgresql.org/pgadmin4/yum/redhat/rhel-7Server-x86_64/pgadmin4-server-6.9-1.el7.x86_64.rpm)
  - [wsgi](https://pgadmin-archive.postgresql.org/pgadmin4/yum/redhat/rhel-7Server-x86_64/pgadmin4-python3-mod_wsgi-4.9.0-1.el7.x86_64.rpm)
  - [web](https://pgadmin-archive.postgresql.org/pgadmin4/yum/redhat/rhel-7Server-x86_64/pgadmin4-web-6.9-1.el7.noarch.rpm)
- installing packages
```bash
rpm -i pgadmin.rpm
rpm -i pgwsgi.rpm
yum install httpd
rpm -i pgadminweb.rpm
```
inside `config_local.py` write 
```
vi config_local.py
```
```python
LOG_FILE = '/var/log/pgadmin4/pgadmin4.log'
SQLITE_PATH = '/var/lib/pgadmin4/pgadmin4.db'
SESSION_DB_PATH = '/var/lib/pgadmin4/sessions'
STORAGE_DIR = '/var/lib/pgadmin4/storage'
AZURE_CREDENTIAL_CACHE_DIR = '/var/lib/pgadmin4/azurecredentialcache'
KERBEROS_CCACHE_DIR = '/var/lib/pgadmin4/kerberoscache'
```
### create config system and override some settoshs 
```bash
touch /etc/pgadmin/config_system.py
vi /etc/pgadmin/config_system.py
```
```python
MASTER_PASSWORD_REQUIRED=False
```
- configure installation
```bash
/usr/pgadmin4/bin/setup-web.sh
```
then run
```bash
chown -R apache:apache /var/lib/pgadmin4/
sudo chmod 740 "/var/lib/pgadmin4"
chown -R apache:apache /var/log/pgadmin4/
sudo chmod 740 "/var/log/pgadmin"
sudo semanage fcontext -a -t httpd_sys_rw_content_t '/var/lib/pgadmin4(/.*)?'
```
LESS GO
now we can visit http://192.168.0.124/pgadmin4
## Create postgres user
```bash
sudo -u postgres psql
createuser admin --pwprompt --interactive
```
We can authorize now
