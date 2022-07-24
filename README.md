# Домашнее задание к занятию "10.03. Grafana"

## Задание повышенной сложности

**В части задания 1** не используйте директорию [help](./help) для сборки проекта, самостоятельно разверните grafana, где в 
роли источника данных будет выступать prometheus, а сборщиком данных node-exporter:
- grafana
- prometheus-server
- prometheus node-exporter

За дополнительными материалами, вы можете обратиться в официальную документацию grafana и prometheus.

В решении к домашнему заданию приведите также все конфигурации/скрипты/манифесты, которые вы 
использовали в процессе решения задания.

**В части задания 3** вы должны самостоятельно завести удобный для вас канал нотификации, например Telegram или Email
и отправить туда тестовые события.

В решении приведите скриншоты тестовых событий из каналов нотификаций.

### Ответ:

Скрипты для запуска мониторинга расположены в директории [project](./project)

---
## Обязательные задания

### Задание 1
Используя директорию [help](./help) внутри данного домашнего задания - запустите связку prometheus-grafana.

Зайдите в веб-интерфейс графана, используя авторизационные данные, указанные в манифесте docker-compose.

Подключите поднятый вами prometheus как источник данных.

Решение домашнего задания - скриншот веб-интерфейса grafana со списком подключенных Datasource.

### Ответ:
<details>
<summary style="font-size:14px">Развернуть</summary>

Источник данных подключается из [datasource.yml](./project/grafana/provisioning/datasources/datasource.yml)

![изображение](https://user-images.githubusercontent.com/93001155/180642626-6818846e-a84e-4bc0-839a-96ddbc15aa96.png)


</details>

---
## Задание 2
Изучите самостоятельно ресурсы:
- [promql-for-humans](https://timber.io/blog/promql-for-humans/#cpu-usage-by-instance)
- [understanding prometheus cpu metrics](https://www.robustperception.io/understanding-machine-cpu-usage)

Создайте Dashboard и в ней создайте следующие Panels:
- Утилизация CPU для nodeexporter (в процентах, 100-idle)
- CPULA 1/5/15
- Количество свободной оперативной памяти
- Количество места на файловой системе

Для решения данного ДЗ приведите promql запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

### Ответ:
<details>
<summary style="font-size:14px">Развернуть</summary>

- Утилизация CPU для nodeexporter (в процентах, 100-idle)
  
  ```
  100 - (avg by (instance) (rate(node_cpu_seconds_total{job="nodeexporter",mode="idle"}[1m])) * 100)
  ```

- CPULA 1/5/15
  
  ```
  node_load1{instance="nodeexporter:9100"}
  node_load5{instance="nodeexporter:9100"}
  node_load15{instance="nodeexporter:9100"}
  ```

- Количество свободной оперативной памяти
  
  ```
  avg by (instance) (node_memory_MemAvailable_bytes{job="nodeexporter"})
  ```

- Количество места на файловой системе
  
  ```
  avg by (instance) (sum(node_filesystem_free_bytes{fstype="ext4"}))
  ```

![изображение](https://user-images.githubusercontent.com/93001155/180642665-3be02404-8b6f-4699-b342-bcb3cdd8aea9.png)



</details>

## Задание 3
Создайте для каждой Dashboard подходящее правило alert (можно обратиться к первой лекции в блоке "Мониторинг").

Для решения ДЗ - приведите скриншот вашей итоговой Dashboard.

### Ответ:

<details>
<summary style="font-size:14px">скрин</summary>

![изображение](https://user-images.githubusercontent.com/93001155/180642758-8e5168f7-6b92-4af1-bd05-41a101cc0d17.png)
![изображение](https://user-images.githubusercontent.com/93001155/180642835-ddd4a509-12fb-4b91-9f8f-051592f7f010.png)

![изображение](https://user-images.githubusercontent.com/93001155/180642801-90386783-3347-45e6-a0d4-41e9f9e875c8.png)



</details>

## Задание 4
Сохраните ваш Dashboard.

Для этого перейдите в настройки Dashboard, выберите в боковом меню "JSON MODEL".

Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.

В решении задания - приведите листинг этого файла.

### Ответ:
<details>
<summary style="font-size:14px">json</summary>

```json
    {
    "annotations": {
        "list": [
        {
            "builtIn": 1,
            "datasource": {
            "type": "grafana",
            "uid": "-- Grafana --"
            },
            "enable": true,
            "hide": true,
            "iconColor": "rgba(0, 211, 255, 1)",
            "name": "Annotations & Alerts",
            "target": {
            "limit": 100,
            "matchAny": false,
            "tags": [],
            "type": "dashboard"
            },
            "type": "dashboard"
        }
        ]
    },
    "editable": true,
    "fiscalYearStartMonth": 0,
    "graphTooltip": 0,
    "id": 6,
    "links": [],
    "liveNow": false,
    "panels": [
        {
        "datasource": {
            "type": "prometheus",
            "uid": "PBFA97CFB590B2093"
        },
        "fieldConfig": {
            "defaults": {
            "color": {
                "mode": "palette-classic"
            },
            "custom": {
                "axisLabel": "",
                "axisPlacement": "auto",
                "barAlignment": 0,
                "drawStyle": "line",
                "fillOpacity": 0,
                "gradientMode": "none",
                "hideFrom": {
                "legend": false,
                "tooltip": false,
                "viz": false
                },
                "lineInterpolation": "linear",
                "lineWidth": 1,
                "pointSize": 5,
                "scaleDistribution": {
                "type": "linear"
                },
                "showPoints": "auto",
                "spanNulls": false,
                "stacking": {
                "group": "A",
                "mode": "none"
                },
                "thresholdsStyle": {
                "mode": "off"
                }
            },
            "mappings": [],
            "thresholds": {
                "mode": "absolute",
                "steps": [
                {
                    "color": "green",
                    "value": null
                },
                {
                    "color": "red",
                    "value": 80
                }
                ]
            }
            },
            "overrides": []
        },
        "gridPos": {
            "h": 9,
            "w": 12,
            "x": 0,
            "y": 0
        },
        "id": 4,
        "options": {
            "legend": {
            "calcs": [],
            "displayMode": "list",
            "placement": "bottom"
            },
            "tooltip": {
            "mode": "single",
            "sort": "none"
            }
        },
        "targets": [
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "expr": "node_load1{instance=\"nodeexporter:9100\"}",
            "legendFormat": "LA 1 - {{instance}}",
            "range": true,
            "refId": "A"
            },
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "expr": "node_load5{instance=\"nodeexporter:9100\"}",
            "hide": false,
            "legendFormat": "LA 5 - {{instance}}",
            "range": true,
            "refId": "B"
            },
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "expr": "node_load15{instance=\"nodeexporter:9100\"}",
            "hide": false,
            "legendFormat": "LA 15 - {{instance}}",
            "range": true,
            "refId": "C"
            }
        ],
        "title": "CPU LA 1/5/15",
        "type": "timeseries"
        },
        {
        "datasource": {
            "type": "prometheus",
            "uid": "PBFA97CFB590B2093"
        },
        "fieldConfig": {
            "defaults": {
            "color": {
                "mode": "palette-classic"
            },
            "custom": {
                "axisLabel": "",
                "axisPlacement": "auto",
                "barAlignment": 0,
                "drawStyle": "line",
                "fillOpacity": 0,
                "gradientMode": "none",
                "hideFrom": {
                "legend": false,
                "tooltip": false,
                "viz": false
                },
                "lineInterpolation": "linear",
                "lineWidth": 1,
                "pointSize": 5,
                "scaleDistribution": {
                "type": "linear"
                },
                "showPoints": "auto",
                "spanNulls": false,
                "stacking": {
                "group": "A",
                "mode": "none"
                },
                "thresholdsStyle": {
                "mode": "off"
                }
            },
            "mappings": [],
            "thresholds": {
                "mode": "absolute",
                "steps": [
                {
                    "color": "green",
                    "value": null
                },
                {
                    "color": "red",
                    "value": 80
                }
                ]
            }
            },
            "overrides": []
        },
        "gridPos": {
            "h": 9,
            "w": 12,
            "x": 12,
            "y": 0
        },
        "id": 2,
        "options": {
            "legend": {
            "calcs": [],
            "displayMode": "list",
            "placement": "bottom"
            },
            "tooltip": {
            "mode": "single",
            "sort": "none"
            }
        },
        "targets": [
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "exemplar": false,
            "expr": "100 - (avg by (instance) (rate(node_cpu_seconds_total{job=\"nodeexporter\",mode=\"idle\"}[1m])) * 100)",
            "interval": "",
            "legendFormat": "CPU usage % - {{instance}}",
            "range": true,
            "refId": "A"
            }
        ],
        "title": "CPU %",
        "type": "timeseries"
        },
        {
        "datasource": {
            "type": "prometheus",
            "uid": "PBFA97CFB590B2093"
        },
        "fieldConfig": {
            "defaults": {
            "color": {
                "mode": "palette-classic"
            },
            "custom": {
                "axisLabel": "",
                "axisPlacement": "auto",
                "barAlignment": 0,
                "drawStyle": "line",
                "fillOpacity": 0,
                "gradientMode": "none",
                "hideFrom": {
                "legend": false,
                "tooltip": false,
                "viz": false
                },
                "lineInterpolation": "linear",
                "lineWidth": 1,
                "pointSize": 5,
                "scaleDistribution": {
                "type": "linear"
                },
                "showPoints": "auto",
                "spanNulls": false,
                "stacking": {
                "group": "A",
                "mode": "none"
                },
                "thresholdsStyle": {
                "mode": "off"
                }
            },
            "mappings": [],
            "thresholds": {
                "mode": "absolute",
                "steps": [
                {
                    "color": "green",
                    "value": null
                },
                {
                    "color": "red",
                    "value": 80
                }
                ]
            },
            "unit": "decbytes"
            },
            "overrides": []
        },
        "gridPos": {
            "h": 8,
            "w": 12,
            "x": 0,
            "y": 9
        },
        "id": 8,
        "options": {
            "legend": {
            "calcs": [],
            "displayMode": "list",
            "placement": "bottom"
            },
            "tooltip": {
            "mode": "single",
            "sort": "none"
            }
        },
        "targets": [
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "expr": "avg by (instance) (node_memory_MemAvailable_bytes{job=\"nodeexporter\"})",
            "legendFormat": "Free mem - {{instance}}",
            "range": true,
            "refId": "A"
            }
        ],
        "title": "Free mem",
        "type": "timeseries"
        },
        {
        "datasource": {
            "type": "prometheus",
            "uid": "PBFA97CFB590B2093"
        },
        "fieldConfig": {
            "defaults": {
            "color": {
                "mode": "palette-classic"
            },
            "custom": {
                "axisLabel": "",
                "axisPlacement": "auto",
                "barAlignment": 0,
                "drawStyle": "line",
                "fillOpacity": 0,
                "gradientMode": "none",
                "hideFrom": {
                "legend": false,
                "tooltip": false,
                "viz": false
                },
                "lineInterpolation": "linear",
                "lineWidth": 1,
                "pointSize": 5,
                "scaleDistribution": {
                "type": "linear"
                },
                "showPoints": "auto",
                "spanNulls": false,
                "stacking": {
                "group": "A",
                "mode": "none"
                },
                "thresholdsStyle": {
                "mode": "off"
                }
            },
            "mappings": [],
            "thresholds": {
                "mode": "absolute",
                "steps": [
                {
                    "color": "green",
                    "value": null
                },
                {
                    "color": "red",
                    "value": 80
                }
                ]
            },
            "unit": "decbytes"
            },
            "overrides": []
        },
        "gridPos": {
            "h": 8,
            "w": 12,
            "x": 12,
            "y": 9
        },
        "id": 6,
        "options": {
            "legend": {
            "calcs": [],
            "displayMode": "list",
            "placement": "bottom"
            },
            "tooltip": {
            "mode": "single",
            "sort": "none"
            }
        },
        "targets": [
            {
            "datasource": {
                "type": "prometheus",
                "uid": "PBFA97CFB590B2093"
            },
            "editorMode": "code",
            "expr": "avg by (instance) (sum(node_filesystem_free_bytes{fstype=\"ext4\"}))",
            "legendFormat": "Free space {{instance}}",
            "range": true,
            "refId": "A"
            }
        ],
        "title": "Free space",
        "type": "timeseries"
        }
    ],
    "refresh": false,
    "schemaVersion": 36,
    "style": "dark",
    "tags": [],
    "templating": {
        "list": []
    },
    "time": {
        "from": "now-5m",
        "to": "now"
    },
    "timepicker": {},
    "timezone": "",
    "title": "Test_dashboard",
    "uid": "gPsRp_g4k",
    "version": 6,
    "weekStart": ""
    }
```



</details>

---
