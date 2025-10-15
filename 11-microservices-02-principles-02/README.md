# Домашнее задание к занятию «Микросервисы: принципы»
Вы работаете в крупной компании, которая строит систему на основе микросервисной архитектуры. Вам как DevOps-специалисту необходимо выдвинуть предложение по организации инфраструктуры для разработки и эксплуатации.

## Задача 1: API Gateway
Предложите решение для обеспечения реализации API Gateway. Составьте сравнительную таблицу возможностей различных программных решений. На основе таблицы сделайте выбор решения.

Решение должно соответствовать следующим требованиям:

маршрутизация запросов к нужному сервису на основе конфигурации,
возможность проверки аутентификационной информации в запросах,
обеспечение терминации HTTPS.
Обоснуйте свой выбор.

## Ответ:
В качестве расматриваемых API geteway были выбраны 3 программных продукта: NGINX, Traefik, Kong.
Сравнительная таблица решений API Gateway:
| Критерий | Kong |	NGINX |	Traefik |
|----------|---------|--------|---------|
Маршрутизация       | Динамическая <br> Load Balancing <br> Canary релизы <br> |  Статическая <br> Load <br> Balancing | Динамическая <br> Load Balancing <br> Canary релизы |
Аутентификация      | JWT <br> OAuth2 <br> Key Auth <br> ACL  | Basic Auth <br> JWT (с модулями) | JWT <br> Basic Auth <br> OAuth2 (Middleware)  |
HTTPS Termination | Самоподписанные <br> Let's Encrypt <br> Custom Certs| Самоподписанные <br> Let's Encrypt <br> Custom Certs | Самоподписанные <br> Let's Encrypt <br> Custom Certs  |
Мониторинг          | Prometheus <br> Datadog <br> Custom metrics  | Prometheus <br> Статус страницы  | Prometheus <br> Custom metrics  |
Производительность  | 15-20k RPS | 50-100k RPS | 0-15k RPS |
Лицензирование и стоймойсть  | Enterprise <br> Free OSS  | Free OSS <br> Plus ($2500+/год)  | 100% Free OSS |

### Рекомендуемое решение: Kong API Gateway
Обоснование выбора:
Соответствие требованиям:
Маршрутизация - динамическая конфигурация через Admin API.
Аутентификация - встроенные плагины JWT, OAuth2, Key Authentication.
HTTPS Termination - автоматическое управление сертификатами через Let's Encrypt.

Преимущества для компании:
1. Гибкость развертывания - Kong можно развернуть как на земле так и в облаке.
2. Расширяемость - более 300 готовых плагинов для различных сценариев.
3. Производительность - основан на NGINX, обеспечивает высокую пропускную способность.
4. Открытый исходный код.


## Задача 2: Брокер сообщений
Составьте таблицу возможностей различных брокеров сообщений. На основе таблицы сделайте обоснованный выбор решения.
Решение должно соответствовать следующим требованиям:
- поддержка кластеризации для обеспечения надёжности,
- хранение сообщений на диске в процессе доставки,
- высокая скорость работы,
- поддержка различных форматов сообщений,
- разделение прав доступа к различным потокам сообщений,
- простота эксплуатации.
Обоснуйте свой выбор.

## Ответ:
| Критерий | RabbitMQ |	Apache Kafka |	NATS | Redis |
|----------|---------|--------|---------|---------|
| Кластеризация и надежность | Высокая <br> Mirroring queues <br> HA политики | Высокая <br> Replication <br> Partition tolerance | Высокая <br> RAFT консенсус <br> Super cluster | Базовая <br> Sentinel <br> Cluster mode |
| Хранение на диске | Durable queues <br> Persistent messages <br> Transaction support |  Сегментированное <br> Retention policies <br> Compact topics | JetStream <br> File storage <br> Retention limits |  AOF (append only) |
| Форматы сообщений | Любые binary <br> Headers <br> Properties | Binary data <br> Headers <br> Schema Registry | Любые binary <br> Headers <br> Metadata | String/binary |
| Разделение прав доступа | VHosts <br> Users/Permissions <br> Topic permissions | SSL/SASL <br> ACLs <br> Role-based | Accounts <br> Users <br> Permissions | Password auth |
| Простота эксплуатации | Простая настройка <br> Web UI <br> Мониторинг | Management tools <br> Monitoring | Простая настройка <br> Lightweight <br> Minimal config | Очень простая <br> Minimal setup |
| Скорость работы | 40-50k msg/sec | 100-500k msg/sec | 1M+ msg/sec | 1M+ msg/sec |
| Протоколы | AMQP 0.9.1, MQTT, STOMP | Custom binary, HTTP | Custom binary, WebSocket | RESP (Redis |
| Мониторинг | Prometheus <br> Management API <br> Grafana dashboards | JMX <br> Prometheus <br> Burrow | Prometheus <br> Monitoring API <br> Grafana | INFO command <br> Monitoring <br> RedisInsight |

### Рекомендуемое решение: RabbitMQ. Данное решение полностью соотвествутет требованиям
1. Кластеризация для надежности
- Mirroring queues с автоматической репликацией
- HA политики для отказоустойчивости
- Простое добавление/удаление узлов кластера

2. Хранение сообщений на диске
- Durable queues с persistence
- Transaction support для гарантий доставки
- Надежное хранение даже при перезагрузках

3. Высокая скорость работы
- 40-50k сообщений/сек достаточно для большинства enterprise-систем
- Оптимизированная работа с памятью и диском

4. Поддержка форматов сообщений
- Любые binary данные
- Headers и properties для метаданных
- Поддержка JSON, XML, Protobuf через плагины

5. Разделение прав доступа
- Virtual Hosts для изоляции окружений
- Детальные permissions на чтение/запись
- Поддержка LDAP/AD интеграции

6. Простота эксплуатации
- Простая настройка и развертывание
- Web UI для мониторинга и управления
- Широкая документация и сообщество

## Задача 3:

![Скриншот задание 3](https://github.com/Sem20071/hw-microservices/blob/main/11-microservices-02-principles-02/images/dz_microservices-02-01.png)
