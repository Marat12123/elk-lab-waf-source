markdown
# Алгоритм установки WAF + DVWA

## Предварительные требования
- ВМ2 с Kali Linux / Ubuntu Server.
- Установленные Docker и Docker Compose.
- Сетевой доступ к ВМ1 (192.168.56.10) по порту 5044.

## Шаг 1. Развёртывание WAF + DVWA
```bash
# Создание проекта
mkdir ~/waf-dvwa && cd ~/waf-dvwa

# Создание docker-compose.yml (см. /config)
nano docker-compose.yml

# Запуск контейнеров
docker-compose up -d

# Проверка
docker-compose ps
Шаг 2. Настройка Filebeat
bash
# Установка Filebeat
sudo apt install filebeat -y

# Настройка (см. /config/filebeat.yml)
sudo nano /etc/filebeat/filebeat.yml

# Запуск
sudo systemctl enable filebeat
sudo systemctl start filebeat
Шаг 3. Настройка Rsyslog (опционально, если не используется Filebeat)
bash
# Создать конфиг (см. /config/50-waf-forward.conf)
sudo nano /etc/rsyslog.d/50-waf-forward.conf

# Перезапустить rsyslog
sudo systemctl restart rsyslog
Шаг 4. Генерация тестовой атаки
bash
# SQL-инъекция
curl "http://192.168.56.11/?id=1%27%20OR%20%271%27=%271"
Ожидаемый ответ: 403 Forbidden.

Шаг 5. Проверка логов
bash
# Локальный лог WAF
sudo tail -f /var/log/nginx/modsec_audit.log

# Логи Filebeat
sudo journalctl -u filebeat -f
Шаг 6. Проверка в Kibana
Открыть http://192.168.56.10:5601.

Перейти в Discover.

Поиск: program:modsecurity или modsecurity.

Установить временной диапазон Today.
