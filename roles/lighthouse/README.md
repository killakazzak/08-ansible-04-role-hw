# Ansible Role: Установка и настройка Lighthouse

## Описание

Эта роль Ansible предназначена для установки и настройки [Lighthouse](https://developers.google.com/web/tools/lighthouse) — инструмента для анализа производительности веб-страниц. Роль поддерживает установку зависимостей через `dnf` и `apt`, а также настройку конфигурации Lighthouse.

## Требования

- Ansible 2.9 или выше
- Доступ к интернету для установки пакетов

## Установка

```bash
$ ansible-galaxy install killakazzak.lighthouse
```

## Использование

### Пример плейбука

```yaml
- hosts: all
  become: yes
  vars:
    lighthouse_version: "12.3.0"  # Укажите нужную версию Lighthouse
    lighthouse_install_dir: "/usr/local/bin/lighthouse"  # Укажите директорию установки
    lighthouse_config_template: "path/to/lighthouse-config.json.j2"  # Укажите путь к шаблону конфигурации

  roles:
    - yourusername.lighthouse
```

### Переменные

| Переменная                     | Описание                                      | По умолчанию |
|--------------------------------|-----------------------------------------------|--------------|
| `lighthouse_version`           | Версия Lighthouse для установки               | `latest`     |
| `lighthouse_install_dir`       | Директория для установки Lighthouse           | `/usr/local/bin/lighthouse` |
| `lighthouse_config_template`   | Путь к шаблону конфигурации Lighthouse       | `lighthouse-config.json.j2` |

## Задачи

- Установка зависимостей (Node.js и npm) в зависимости от менеджера пакетов (`dnf` или `apt`).
- Установка Lighthouse глобально через npm.
- Проверка установленной версии Lighthouse.
- Копирование файла конфигурации Lighthouse из шаблона.
- Запуск Lighthouse с указанной конфигурацией.

## Лицензия

Этот проект лицензирован под [MIT License](LICENSE).

## Контрибьюция

Если вы хотите внести свой вклад в проект, пожалуйста, создайте форк репозитория и отправьте пулл-реквест с вашими изменениями.

## Поддержка

Если у вас есть вопросы или проблемы, пожалуйста, откройте issue в репозитории.
