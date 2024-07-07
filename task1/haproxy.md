## Установим прокси сервер как точку входа для patroni кластера
```bash
yum install haproxy22
firewall-cmd --permanent --add-port=7000/tcp --add-port=5432/tcp
firewall-cmd --reload
vi /etc/haproxy/haproxy.cfg
```
Вставим следующую конфигурацию
```cfg
global

        maxconn 100
        log     127.0.0.1 local2

defaults
        log global
        mode tcp
        retries 2
        timeout client 30m
        timeout connect 5s
        timeout server 30m
        timeout check 5s

listen stats
    mode http
    bind 192.168.0.124:7000
    stats enable
    stats uri /stats
    stats auth admin:admin

listen postgres
    bind 192.168.0.124:5000
    option httpchk
    http-check expect status 200
    default-server inter 3s fall 3 rise 2 on-marked-down shutdown-sessions
    server node1 192.168.0.123:5432 maxconn 200 check port 8008
    server node2 192.168.0.125:5432 maxconn 200 check port 8008
```
Затем включим haproxy в SELinux и запустим
```
setsebool -P haproxy_connect_any=1
systemctl enable haproxy
systemctl start haproxy
```
