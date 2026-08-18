WAF + Веб-приложение (DVWA)

## Краткое описание
Развёрнут WAF на базе Nginx с ModSecurity и набором правил OWASP Core Rule Set. За WAF настроено проксирование трафика на уязвимое веб-приложение DVWA. Логи срабатываний WAF собираются с помощью Filebeat и отправляются на Logstash (ВМ1, `192.168.56.10:5044`). Это позволяет централизованно анализировать попытки атак на веб-приложение.

## Технические детали
- **WAF:** Nginx + ModSecurity + OWASP CRS (Docker-образ `owasp/modsecurity-crs:nginx`).
- **Веб-приложение:** DVWA (Docker-образ `vulnerables/web-dvwa`).
- **Сбор логов:** Filebeat (читает `/var/log/nginx/modsec_audit.log`).
- **Отправка:** Logstash на ВМ1 (порт 5044).

## Архитектура
Схема и потоки данных описаны в `/docs/architecture.md`.

## Результаты
- ✅ WAF блокирует SQL-инъекции (ответ 403 Forbidden).
- ✅ Логи сработавших правил доставляются в Elasticsearch и видны в Kibana.
- ✅ DVWA доступно через WAF по адресу `http://192.168.56.11`.

## Структура репозитория
elk-lab-waf-source/
├── README.md
├── docs/
│ ├── architecture.md
│ └── deployment.md
├── config/
│ ├── docker-compose.yml
│ ├── filebeat.yml
│ └── 50-waf-forward.conf
├── samples/
│ └── modsec_audit_sample.log
└── screenshots/
├── 01_dvwa_login.png
└── 02_kibana_waf_events.png
