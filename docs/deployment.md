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
