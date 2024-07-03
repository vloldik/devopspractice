## Добавление соединения
```bash
nmcli connection add type ethernet con-name Ethepnet1 ifname eth0 ip4 192.168.0.<АДРЕС>/24
```
## Соединение (после этого можно подключиться по SSH)
```bash
nmcli connection up Ethernet1
```
## Добавление роута для обеспечения интернета, настройка DNS и установка autoconnect
```bash
nmcli connection modify Ethernet1 ipv4.gateway 192.168.0.1
nmcli connection modify Ethernet1 ipv4.dns "8.8.8.8 8.8.4.4"
nmcli connection modify Ethernet1 connection.autoconnect yes
nmcli connection up Ethernet1
```
