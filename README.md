# Ansible роли для развертывания Prometheus, Node Exporter, ElasticSearch, Kibana и FluentBit

Этот репозиторий содержит Ansible роли для автоматизации развертывания следующих компонентов:

1. **Prometheus** - система мониторинга и оповещений.
2. **Node Exporter** - экспортер метрик для мониторинга хостов.
3. **ElasticSearch** - поисковая и аналитическая система для работы с логами.
4. **Kibana** - веб-интерфейс для визуализации данных из ElasticSearch.
5. **FluentBit** - легковесный сборщик и пересыльщик логов.

Все роли разработаны для работы на операционной системе Ubuntu.

## Роли

### 1. Роль `prometheus`
Эта роль устанавливает и настраивает Prometheus для мониторинга внешних хостов.

**Основные задачи:**
- Установка Prometheus.
- Создание systemd service файла для запуска Prometheus.
- Создание пользователей и директорий с необходимыми правами.
- Настройка конфигурации Prometheus для мониторинга самого себя.
- Запуск и перезагрузка сервиса при изменении конфигурации.

**Переменные:**
- `prometheus_version` - версия Prometheus.
- `prometheus_config_dir` - директория для конфигурационных файлов.
- `prometheus_data_dir` - директория для хранения данных.
- `prometheus_user` - пользователь для запуска Prometheus.
- `prometheus_group` - группа для запуска Prometheus.

### 2. Роль `node_exporter`
Эта роль устанавливает и настраивает Node Exporter для сбора метрик с хостов.

**Основные задачи:**
- Установка Node Exporter.
- Создание systemd service файла для запуска Node Exporter.
- Создание пользователей и директорий с необходимыми правами.
- Добавление нового хоста в конфигурацию Prometheus через static config.
- Запуск сервиса Node Exporter и перезагрузка Prometheus при добавлении нового хоста.

**Переменные:**
- `node_exporter_version` - версия Node Exporter.
- `node_exporter_user` - пользователь для запуска Node Exporter.
- `node_exporter_group` - группа для запуска Node Exporter.
- `prometheus_server` - адрес сервера Prometheus для добавления нового хоста.

### 3. Роль `elasticsearch_kibana`
Эта роль устанавливает и настраивает ElasticSearch и Kibana.

**Основные задачи:**
- Установка ElasticSearch.
- Установка Kibana.
- Создание systemd service файлов для запуска ElasticSearch и Kibana.
- Создание пользователей и директорий с необходимыми правами.
- Запуск сервисов ElasticSearch и Kibana.

**Переменные:**
- `elasticsearch_version` - версия ElasticSearch.
- `kibana_version` - версия Kibana.
- `elasticsearch_user` - пользователь для запуска ElasticSearch.
- `elasticsearch_group` - группа для запуска ElasticSearch.
- `kibana_user` - пользователь для запуска Kibana.
- `kibana_group` - группа для запуска Kibana.

### 4. Роль `fluentbit`
Эта роль устанавливает и настраивает FluentBit для сбора системных логов и их пересылки в ElasticSearch.

**Основные задачи:**
- Установка FluentBit.
- Создание systemd service файла для запуска FluentBit.
- Создание пользователей и директорий с необходимыми правами.
- Настройка конфигурации FluentBit для сбора логов из journald и пересылки их в ElasticSearch.
- Запуск сервиса FluentBit.

## Использование

1. Клонируйте репозиторий:
   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```

2. Создайте инвентарный файл `inventory.yml` с описанием ваших хостов:
   ```yaml
   all:
     hosts:
       your_vm:
         ansible_host: <vm_ip>
         ansible_user: <vm_user>
         ansible_ssh_private_key_file: <path_to_private_key>
   ```

3. Создайте playbook `site.yml` для применения ролей:
   ```yaml
   - hosts: your_vm
     roles:
       - prometheus
       - node_exporter
       - elasticsearch_kibana
       - fluentbit
   ```

4. Запустите playbook:
   ```bash
   ansible-playbook -i inventory.yml site.yml
   ```

## Лучшие практики

- **Идемпотентность**: Роли разработаны таким образом, что их повторное применение не приведет к изменениям на уже настроенном хосте.
- **Хэндлеры**: Используются для перезагрузки конфигурации и сервисов при изменении конфигурационных файлов.
- **Переменные**: Все настраиваемые параметры вынесены в `defaults`, что позволяет легко изменять конфигурацию без редактирования самой роли.

## Заключение

Эти роли позволяют автоматизировать процесс развертывания и настройки Prometheus, Node Exporter, ElasticSearch, Kibana и FluentBit на Ubuntu. Использование Ansible обеспечивает повторяемость и масштабируемость процесса настройки.

Для дополнительной информации по настройке и использованию каждого компонента, обратитесь к официальной документации:

- [Prometheus](https://prometheus.io/docs/)
- [Node Exporter](https://github.com/prometheus/node_exporter)
- [ElasticSearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html)
- [FluentBit](https://docs.fluentbit.io/manual/)
