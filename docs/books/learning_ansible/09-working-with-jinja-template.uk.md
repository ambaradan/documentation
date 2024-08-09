---
title: Робота з шаблоном Jinja
author: Srinivas Nishant Viswanadha
contributors: Steven Spencer, Antoine Le Morvan, Ganna Zhyrnova
---

## Вступ

Ansible забезпечує потужний і простий спосіб керування конфігураціями за допомогою шаблонів Jinja через вбудований модуль `template`. Цей документ досліджує два важливі способи використання шаблонів Jinja в Ansible:

- додавання змінних до файлу конфігурації
- створення складних файлів із циклами та складними структурами даних.

## Додавання змінних до файлу конфігурації

### Крок 1: Створення шаблону Jinja

Створіть файл шаблону Jinja, наприклад, `sshd_config.j2`, із заповнювачами для змінних:

```jinja
# /path/to/sshd_config.j2

Port {{ ssh_port }}
PermitRootLogin {{ permit_root_login }}
# Add more variables as needed
```

### Крок 2: Використання модуля шаблону Ansible

У Ansible playbook використовуйте модуль `template`, щоб відобразити шаблон Jinja з певними значеннями:

```yaml
---
- name: Generate sshd_config
  hosts: your_target_hosts
  tasks:
    - name: Template sshd_config
      template:
        src: /path/to/sshd_config.j2
        dest: /etc/ssh/sshd_config
      vars:
        ssh_port: 22
        permit_root_login: no
      # Add more variables as needed
```

### Крок 3: Застосування змін конфігурації

Виконайте Ansible playbook, щоб застосувати зміни до цільових хостів:

```bash
ansible-playbook your_playbook.yml
```

Цей крок забезпечує послідовне застосування змін конфігурації до вашої інфраструктури.

## Створення повного файлу з циклами та складними структурами даних

### Крок 1: Вдосконалення шаблону Jinja

Розширте свій шаблон Jinja для обробки циклів і складних структур даних. Ось приклад налаштування гіпотетичної програми з кількома компонентами:

```jinja
# /path/to/app_config.j2

{% for component in components %}
[{{ component.name }}]
    Path = {{ component.path }}
    Port = {{ component.port }}
    # Add more configurations as needed
{% endfor %}
```

### Крок 2: Інтеграція модуля шаблону Ansible

У ваш Ansible playbook інтегруйте модуль `template`, щоб створити повний файл конфігурації:

```yaml
---
- name: Generate Application Configuration
  hosts: your_target_hosts
  vars:
    components:
      - name: web_server
        path: /var/www/html
        port: 80
      - name: database
        path: /var/lib/db
        port: 3306
      # Add more components as needed
  tasks:
    - name: Template Application Configuration
      template:
        src: /path/to/app_config.j2
        dest: /etc/app/config.ini
```

Виконайте Ansible playbook, щоб застосувати зміни до цільових хостів:

```bash
ansible-playbook your_playbook.yml
```

Цей крок забезпечує послідовне застосування змін конфігурації до вашої інфраструктури.

Модуль Ansible `template` дозволяє використовувати шаблони Jinja для динамічного створення конфігураційних файлів під час виконання playbook. Цей модуль дозволяє розділяти логіку конфігурації та дані, роблячи ваші підручники Ansible більш гнучкими та зручними в обслуговуванні.

### Ключові особливості

1. **Візуалізація шаблону:**
   - Модуль рендерить шаблони Jinja для створення файлів конфігурації з динамічним вмістом.
   - Змінні, визначені в посібнику чи інвентарі, можна вставляти в шаблони, уможливлюючи динамічні конфігурації.

2. **Використання Jinja2:**
   - Модуль `template` використовує механізм створення шаблонів Jinja2, надаючи такі потужні функції, як умови, цикли та фільтри для розширеної роботи з шаблонами.

3. **Шляхи джерела та призначення:**
   - Визначає вихідний файл шаблону Jinja та шлях призначення для створеного файлу конфігурації.

4. **Передача змінних:**
   - Змінні можна передати безпосередньо в завданні playbook або завантажити із зовнішніх файлів, що забезпечує гнучку та динамічну генерацію конфігурації.

5. **Ідемпотентне виконання:**
   - Модуль шаблону підтримує ідемпотентне виконання, забезпечуючи застосування шаблону лише у разі виявлення змін.

### Приклад фрагмента playbook

```yaml
---
- name: Generate Configuration File
  hosts: your_target_hosts
  tasks:
    - name: Template Configuration File
      template:
        src: /path/to/template.j2
        dest: /etc/config/config_file
      vars:
        variable1: value1
        variable2: value2
```

### Випадки використання

1. **Управління конфігурацією:**
   - Ідеально підходить для керування конфігураціями системи шляхом динамічного створення файлів на основі певних параметрів.

2. **Налаштування програми:**
   - Корисно для створення конфігураційних файлів програми з різними параметрами.

3. **Інфраструктура як код:**
   - Спрощує практику «Інфраструктура як код», дозволяючи динамічно коригувати конфігурації на основі змінних.

### Кращі практики

1. **Відокремлення інтересів:**
   - Зберігайте логіку конфігурації в шаблонах Jinja, відокремлюючи її від основної структури посібника.

2. **Контроль версій:**
   - Зберігайте шаблони Jinja в репозиторіях з контрольованими версіями для кращого відстеження та співпраці.

3. **Перевіряемість:**
   - Тестуйте шаблони незалежно, щоб переконатися, що вони дають очікуваний результат конфігурації.

Використовуючи модуль `template`, користувачі Ansible можуть підвищити керованість і гнучкість завдань конфігурації, сприяючи більш спрощеному та ефективному підходу до налаштування системи та програм.

### Посилання

[Документація модуля Ansible Template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html).