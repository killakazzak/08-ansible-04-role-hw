# Домашнее задание к занятию 4 «Работа с roles»

## Подготовка к выполнению

1. * Необязательно. Познакомьтесь с [LightHouse](https://youtu.be/ymlrNlaHzIY?t=929).
2. Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
3. Добавьте публичную часть своего ключа к своему профилю на GitHub.

## Основная часть

Ваша цель — разбить ваш playbook на отдельные roles. 

Задача — сделать roles для ClickHouse, Vector и LightHouse и написать playbook для использования этих ролей. 

Ожидаемый результат — существуют три ваших репозитория: два с roles и один с playbook.

**Что нужно сделать**

1. Создайте в старой версии playbook файл `requirements.yml` и заполните его содержимым:

   ```yaml
   ---
     - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
       scm: git
       version: "1.13"
       name: clickhouse 
   ```

Файл создан requirements.yml

2. При помощи `ansible-galaxy` скачайте себе эту роль.

```bash
ansible-galaxy install -r requirements.yml -p roles
```
![image](https://github.com/user-attachments/assets/dea4e9cc-f851-4c37-833c-cc089bdc4460)

3. Создайте новый каталог с ролью при помощи `ansible-galaxy role init vector-role`.

```bash
ansible-galaxy role init --init-path ./roles/ vector-role
```
![image](https://github.com/user-attachments/assets/aac4c4dd-43fd-467f-b82a-5f47589dcf04)
 
4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`. 

roles/vector_role/vars/main.yml

```yml
---
# Variables specific to the Vector role
vector_role_version: "0.43.1" # Set the desired version of Vector
vector_role_arch: "x86_64"
```
roles/vector_role/defaults/main.yml
```yml
---
# Default variables for the Vector role
vector_role_install_dir: "/opt/vector"
vector_role_config_template: "vector_config.j2"
```
roles/vector_role/tasks/main.yml
```yaml
---
- name: Установить и настроить Vector
  block:
    # - name: Получить последнюю версию Vector
    # Раскомментируйте следующие строки, чтобы динамически получить последнюю версию
    # ansible.builtin.uri:
    #   url: "https://api.github.com/repos/vectordotdev/vector/releases/latest"
    #   return_content: true
    # register: vector_release

    # Убедитесь, что вы не оставили пустую строку или не закомментировали всю задачу
    # Если вы не используете эту задачу, удалите её или закомментируйте

    - name: Установить переменную vector_version
      ansible.builtin.set_fact:
        # v_version: "{{ vector_release.json.tag_name | replace('v', '') }}"
        v_version: "{{ vector_role_version }}"

    - name: Скачать дистрибутив Vector
      ansible.builtin.get_url:
        url: "https://github.com/vectordotdev/vector/releases/download/v{{ v_version }}/vector-{{ v_version }}-{{ vector_role_arch }}-unknown-linux-gnu.tar.gz"
        dest: "/tmp/vector-{{ v_version }}-{{ vector_role_arch }}.tar.gz"
        mode: "0644"

    - name: Создать директорию для установки Vector
      ansible.builtin.file:
        path: "{{ vector_role_install_dir }}"
        state: directory
        mode: "0755"

    - name: Извлечь дистрибутив Vector
      ansible.builtin.unarchive:
        src: "/tmp/vector-{{ v_version }}-{{ vector_role_arch }}.tar.gz"
        dest: "{{ vector_role_install_dir }}"
        remote_src: true

    - name: Проверить содержимое директории установки Vector
      ansible.builtin.command: ls -l {{ vector_role_install_dir }}
      register: vector_dir_contents
      changed_when: false

    - name: Скопировать файл конфигурации Vector
      ansible.builtin.template:
        src: "{{ vector_role_config_template }}"
        dest: "{{ vector_role_install_dir }}/vector.toml"
        mode: "0644"
```

5. Перенести нужные шаблоны конфигов в `templates`.

Файл перенесен в roles/vector_role/templates/vector_config.j2

6. Опишите в `README.md` обе роли и их параметры. Пример качественной документации ansible role [по ссылке](https://github.com/cloudalchemy/ansible-prometheus).

В файл README.md добавлено описание роли https://github.com/killakazzak/08-ansible-04-role-hw/blob/master/roles/vector_role/README.md

7. Повторите шаги 3–6 для LightHouse. Помните, что одна роль должна настраивать один продукт.

Роль для установки Ligthhouse - создана

https://github.com/killakazzak/lighthouse

8. Выложите все roles в репозитории. Проставьте теги, используя семантическую нумерацию. Добавьте roles в `requirements.yml` в playbook.

Все роли загружены в репозитории. Тэги проставлены.

https://github.com/killakazzak/vector_role.git
https://github.com/killakazzak/lighthouse.git

10. Переработайте playbook на использование roles. Не забудьте про зависимости LightHouse и возможности совмещения `roles` с `tasks`.
11. Выложите playbook в репозиторий.

Ansible-playbook загружен
https://github.com/killakazzak/08-ansible-04-role-hw/blob/master/playbook.yml

12. В ответе дайте ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

https://github.com/killakazzak/vector_role.git
https://github.com/killakazzak/lighthouse.git
https://github.com/killakazzak/08-ansible-04-role-hw/blob/master/playbook.yml

Роли загрузил на ansible-galaxy

https://galaxy.ansible.com/ui/standalone/roles/killakazzak/vector_role/

https://galaxy.ansible.com/ui/standalone/roles/killakazzak/lighthouse/

![image](https://github.com/user-attachments/assets/18e3b35e-c70d-4982-8bfb-96c99bf0061e)

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
