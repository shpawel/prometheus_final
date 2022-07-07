# Node1

Запуск базы
```bash
docker compose up -d db
```
Подключение к контейнеру с базой для создания пользователя для mysql_exporter
```bash
sudo docker compose exec db mysql -u root -p
```
Создание пользователя и назначение разрешений (см. документацию по запуску mysql_exporter)
```mysql
CREATE USER 'prometheus'@'%' IDENTIFIED BY 'prometheus' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'prometheus'@'%';
```
Запуск остальных сервисов (wordpress, cadvisor, mysql_exporter, node_exporter)
```bash
docker compose up -d
```


# Prometheus


Создание отдельного пользователя
```bash
useradd -s /sbin/nologin --system prometheus
```
Создание директории в которой будут храниться данные Prometheus и остальных связных сервисов
```bash
mkdir -p /opt/monitoring
chown prometheus:prometheus /opt/monitoring
```
Т.к. все приложения указанные в задании будут запущены в контейнерах требуется получить UID, GID
созданного пользователя Prometheus
```bash
id prometheus
```
Запуск всех сервисов
```bash
docker compose up -d
```