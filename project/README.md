# prometheus + grafana + nodeexporter

Для запуска набора образов можно воспользоваться командой:

```bash
./sanbox up
```

## grafana
---
Datasource - подключается в [datasource.yml](./grafana/provisioning/datasources/datasource.yml)  
Dashboard - настройки панели сохранены в [general_test_dashboard.json](./grafana/provisioning/dashboards/general_test_dashboard.json)  
Логин и пароль для доступа устанавливаются из [.env](./.env)

## prometeus
---
Конфигурация prometheus - [prometheus.yml](prometheus/prometheus.yml)