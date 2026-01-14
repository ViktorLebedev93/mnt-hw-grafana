# Домашнее задание к занятию 14 «Средство визуализации Grafana»

## Обязательные задания

### Задание 1

1. Используя директорию [help](https://github.com/netology-code/mnt-homeworks/tree/MNT-video/10-monitoring-03-grafana/help) внутри этого домашнего задания, запустите связку prometheus-grafana.
1. Зайдите в веб-интерфейс grafana, используя авторизационные данные, указанные в манифесте docker-compose.
1. Подключите поднятый вами prometheus, как источник данных.
1. Решение домашнего задания — скриншот веб-интерфейса grafana со списком подключенных Datasource.

### Решение 1

![img1](img/img1.jpg)
![img2](img/img2.jpg)

------

## Задание 2

Изучите самостоятельно ресурсы:

1. [PromQL tutorial for beginners and humans](https://valyala.medium.com/promql-tutorial-for-beginners-9ab455142085).
1. [Understanding Machine CPU usage](https://www.robustperception.io/understanding-machine-cpu-usage).
1. [Introduction to PromQL, the Prometheus query language](https://grafana.com/blog/2020/02/04/introduction-to-promql-the-prometheus-query-language/).

Создайте Dashboard и в ней создайте Panels:

- утилизация CPU для nodeexporter (в процентах, 100-idle);
- CPULA 1/5/15;
- количество свободной оперативной памяти;
- количество места на файловой системе.

Для решения этого задания приведите promql-запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

### Решение 2

Мои запросы

Утилизация CPU (в процентах, 100-idle)

```
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100)
```

Средняя загрузка системы (Load Average) 1/5/15 минут

```
node_load1 or node_load5 or node_load15
```

Количество свободной оперативной памяти

```
node_memory_MemFree_bytes
```

Количество свободного места на файловой системе
```
node_filesystem_free_bytes{mountpoint="/"}
```

![img3](img/img3.jpg)

------

## Задание 3

1. Создайте для каждой Dashboard подходящее правило alert — можно обратиться к первой лекции в блоке «Мониторинг».
1. В качестве решения задания приведите скриншот вашей итоговой Dashboard.

### Решение 3

Были созданы алерты на каждый дашборд

### Алерт 1: Утилизация CPU
**Метрика:** `100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100)`
- **Порог:** > 90%
- **Время срабатывания:** 3 минуты
- **Severity:** Warning
- **Condition в Grafana:**
  ```
  WHEN: last()
  OF: query(A, 1m, now)
  IS ABOVE: 90
  ```
  
###  Алерт 2: Средняя загрузка системы (Load Average)
- **Метрика:** `node_load1`
- **Тип алерта:** Warning
- **Порог:** > 3.5
- **Время срабатывания:** 5 минут
```
WHEN:   last()
OF:     query(A, 1m, now)
IS ABOVE: 3.5
```

### Алерт 3: Свободная оперативная память
- **Метрика:** `node_memory_MemFree_bytes`
- **Тип алерта:** Critical
- **Порог:** < 150 МБ (157286400 байт)
- **Время срабатывания:** 2 минуты
```
WHEN:   last()
OF:     query(A, 1m, now)
IS BELOW: 157286400
```

### Алерт 4: Свободное место на файловой системе
- **Метрика:** `node_filesystem_free_bytes{mountpoint="/"}`
- **Тип алерта:** Critical
- **Порог:** < 1 ГБ (1073741824 байт)
- **Время срабатывания:** 5 минут
```
WHEN:   last()
OF:     query(A, 2m, now)
IS BELOW: 1073741824
```

![img4](img/img4.jpg)
![img4](img/img4.jpg)

------

## Задание 4

1. Сохраните ваш Dashboard.Для этого перейдите в настройки Dashboard, выберите в боковом меню «JSON MODEL». Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.
2. В качестве решения задания приведите листинг этого файла.

### Решение 4

------

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---