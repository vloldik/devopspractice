## На своей машине
```bash
docker pull dpage/pgadmin4
docker run -p 80:80 \
    -e 'PGADMIN_DEFAULT_EMAIL=<ЛОГИН>' \
    -e 'PGADMIN_DEFAULT_PASSWORD=<ПАРОЛЬ>' \
    -d dpage/pgadmin4 
```

## Перейдем по адресу 127.0.0.1 и подключимся к HAProxy
- Заходим как `postgres`:`patroni`
- В качестве сервера указываем `192.168.0.124:5432`
- Видим, что все работает нормально
