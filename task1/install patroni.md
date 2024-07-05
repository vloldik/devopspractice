# Загрузим библиотеку, используя питон, установленный во время [настройки pgadmin](https://github.com/vloldik/devopspractice/blob/main/task1/installing%20pgadmin.md)
```bash
pip3.9 install patroni[etcd3]
```
## Схема установки
![Схема установки](https://github.com/vloldik/devopspractice/blob/main/task1/scheme.png?raw=true)
- Имеется заранее установленный postgres на 192.168.0.124, проделаем [тот же процесс](https://github.com/vloldik/devopspractice/blob/main/task1/installing%20PG.md) для 192.168.0.123
