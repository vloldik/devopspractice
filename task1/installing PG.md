*Running on vm with ip 192.168.0.124*

## Update yum domains, because of mirrorlist.centos.org doesn't exists anymore.
```bash
sed -i s/mirror.centos.org/vault.centos.org/g /etc/yum.repos.d/*.repo
sed -i s/^#.*baseurl=http/baseurl=http/g /etc/yum.repos.d/*.repo
sed -i s/^mirrorlist=http/#mirrorlist=http/g /etc/yum.repos.d/*.repo
```
- Update yum `yum -y update`
- Reload server `reboot now`
## To install lastest version make repo changes
```bash
vi /etc/yum.repos.d/CentOS-Base.repo
```
Add line `exclude=postgresql*`
```bash
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```
## Install postres 14
```bash
sudo yum install postgresql14-server
```
click y anywhere
## initialize database, start service and enable it
```bash
sudo /usr/pgsql-14/bin/postgresql-14-setup initdb
sudo systemctl start postgresql-14
sudo systemctl enable postgresql-14
```


