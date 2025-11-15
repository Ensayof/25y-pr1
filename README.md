>*Человек может сделать великим путь, которым идёт, но путь не может сделать человека великим*
>Конфуций

# Что такое Alertmanager?
***Alertmanager** — компонент системы мониторинга Prometheus, который обрабатывает оповещения, группирует их и отправляет получателю (например, на электронную почту, в Telegram или Slack). Конфигурация Alertmanager задаётся в файле alertmanager.yml в формате YAML.*
Оновные назначения:
- Получать алерты от Prometheus и других источников.
- Группировать похожие оповещения, чтобы уменьшить шум.
- Отфильтровывать и подавлять алерты, которые не требуют действия.
- Отправлять уведомления в нужные каналы (*Email, Slack, Telegram, PagerDuty и т.д.*).
- Управлять маршрутизацией — какие алерты куда и при каких условиях отправлять.
**Роль в мониторинге:** Alertmanager обеспечивает надежную и умную доставку оповещений, помогает избежать “alert fatigue” и делает систему мониторинга более управляемой и эффективной.

# Таблица с основными правилами оповещений и действиями
Ниже рпдеставлена таблица с основными правилами опевещений и действиями
| Правило | Что делает | 
|-----------|-----------|
| Routing  | Направляет алерты в нужный канал  | 
| Grouping  | Объединяет похожие алерты  | 
| Inhibition  | Подавляет второстепенные алерты  |
| Silences  | Временно отключает алерты  |
| Retrying  | Повторяет доставку при ошибках  |

# Файлик
&#9658;Ниже представлен пример конфигурационного файла alertmanager.yml в блоке кода.

```
global:
  resolve_timeout: 5m
route:
  receiver: "default"
  group_by: ["alertname"]
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h
receivers:
  - name: "default"
    email_configs:
      - to: "alerts@example.com"
inhibit_rules:
  - source_match:
      severity: "critical"
    target_match:
      severity: "warning"
    equal: ["alertname"]
```

Если кратко: файл задаёт, как Alertmanager принимает, группирует, подавляет и отправляет алерты — в данном примере через email.

# Инструкция по запуску и интеграции 

1. Запуск Alertmanager
```
alertmanager --config.file=alertmanager.yml
```
2. Проверить веб-интерфейс
Открыть в браузере: [Название ссылки](http://localhost:9093)
3. Интеграция с Prometheus `prometheus.yml` добавить:
```
alerting:
  alertmanagers:
    - static_configs:
        - targets: ["localhost:9093"]
```
4. Перезапустить Prometheus
`prometheus --config.file=prometheus.yml`

<div style="background: #007bff; color: white; padding: 12px; border-radius: 5px; border: 1px solid #0056b3; font-family: Arial, sans-serif;">
  <strong>Готово:</strong> Prometheus отправляет алерты в Alertmanager.
</div>

