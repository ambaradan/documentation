---
title: Команда tar
author: tianci li
contributors: Ganna Zhyrnova
tested_with: 8.10
tags:
  - tar
  - резервне копіювання
  - archive
---

## Огляд

`tar` — це інструмент для обробки архівних файлів у GNU/Linux та інших операційних системах UNIX. Це розшифровується як «tape archive».

Зручне зберігання файлів на магнітній стрічці було початковим використанням архівів tar. Назва «tar» походить від цього використання. Незважаючи на назву утиліти, `tar` може спрямовувати свій вихід на доступні пристрої, файли чи інші програми (за допомогою каналів) і отримувати доступ до віддалених пристроїв або файлів (як архіви).

`tar`, який зараз використовується в сучасному GNU/Linux, спочатку походить від [проекту GNU](https://www.gnu.org/). Ви можете переглянути та завантажити всі версії `tar` на [GNU's website](https://ftp.gnu.org/gnu/tar/).

!!! note "Примітка"

````
`tar` у різних дистрибутивах може мати різні параметри за замовчуванням. Будьте обережні при їх використанні.

```bash
# RockyLinux 8 and Fedora 41
Shell > tar --show-defaults
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/etc/rmt --rsh-command=/usr/bin/ssh
```
````

## Використання `tar`

Використовуючи `tar`, зауважте, що він має два режими збереження:

- **Відносний режим** (за замовчуванням): видалення початкового символу '/' з файлу. Навіть якщо ви додали файл із абсолютним шляхом, `tar` видалить початковий символ "/" у цьому режимі.
- **Абсолютний режим**: збережіть початковий символ '/' і включіть його в назву файлу. Щоб увімкнути цей режим збереження, потрібно використати опцію `-P`. У цьому режимі ви повинні представити всі файли як абсолютні шляхи. З міркувань безпеки вам не слід використовувати цей режим збереження в більшості випадків, якщо немає спеціальних вимог сценарію.

Коли ви використовуєте tar, ви зустрінете такі суфікси, як `.tar.gz`, `.tar.xz` і `.tar.bz2`, які вказують на те, що ви спочатку створюєте архів (класифікуючи пов’язані файли як один файл), а потім стисніть файл за допомогою відповідного типу стиснення або алгоритму.

Тип або алгоритм стиснення може бути gzip, bzip2, xz, zstd або іншим.

`tar` дозволяє видобувати один файл або каталог із резервної копії, переглядати його вміст або перевіряти його цілісність.

Використання створення архіву та використання стиснення:

- `tar [option] [PATH] [DIR1] ... [FILE1] ...`. Наприклад, `tar -czvf /tmp/Fullbackup-20241201.tar.gz /etc/ /var/log/`

Використання для вилучення файлу з архіву:

- `tar [option] [PATH] -C [dir]`. Наприклад, `tar -xzvf /tmp/Fullbackup-20241201.tar.gz -C /tmp/D1`

!!! tip "antic"

Коли ви видобуваєте файли з архівних файлів, tar автоматично вибирає тип стиснення на основі доданого вручну суфікса. Наприклад, для файлів `.tar.gz` можна напряму використовувати `tar -vxf` без використання `tar -zvxf`.
Ви **повинні** вибрати тип стиснення для створення архівних стиснутих файлів.

!!! Note "Примітка"

```
У GNU/Linux більшість файлів не потребують розширення, за винятком робочого середовища (GUI). Причиною штучного додавання суфіксів є полегшення розпізнавання користувачами. Якщо системний адміністратор бачить, наприклад, розширення файлу `.tar.gz` або `.tgz`, він знає, як поводитися з файлом.
```

### Робочі параметри або типи

|    типи    | опис                                                                                                                                       |
| :--------: | :----------------------------------------------------------------------------------------------------------------------------------------- |
|    `-A`    | Додає всі файли в одному архіві в кінець іншого архіву. Застосовується лише до архівних нестиснутих файлів типу `.tar`     |
|    `-c`    | Створює архів. Дуже часто використовується                                                                                 |
|    `-d`    | Порівнює відмінності між заархівованими та відповідними незаархівованими файлами                                                           |
|    `-r`    | Додає файли або каталоги в кінець архіву. Застосовується лише до архівних нестиснутих файлів типу `.tar`                   |
|    `-t`    | Перелічує вміст архіву                                                                                                                     |
|    `-u`    | До архіву додає лише нові файли. Застосовується лише до архівних нестиснутих файлів типу `.tar`                            |
|    `-x`    | Виписка з архіву. Дуже часто використовується                                                                              |
| `--delete` | Видаляє файли або каталоги з архіву ".tar". Застосовується лише до архівних нестиснутих файлів типу `.tar` |

!!! tip "Порада"

```
Що стосується типів операцій, автор рекомендує залишити префікс «-» для збереження звичок користувача. Звичайно, це не обов'язково. Операційні параметри тут вказують на вашу основну функцію з tar. Іншими словами, вам потрібно вибрати один з перерахованих вище типів.
```

### Поширені допоміжні опції

| опція | опис                                                                                                                   |
| :---: | :--------------------------------------------------------------------------------------------------------------------- |
|  `-z` | Використовуйте `gzip` як тип стиснення. Застосовуються як створення архівів, так і вилучення з архівів |
|  `-v` | Відображає детальні деталі обробки                                                                                     |
|  `-f` | Вказує ім’я файлу для архівування (включаючи суфікс файлу)                                          |
|  `-j` | Використовуйте bzip2 як тип стиснення. Застосовуються як створення архівів, так і вилучення з архівів  |
|  `-J` | Використовуйте "xz" як тип стиснення. Застосовуються як створення архівів, так і вилучення з архівів   |
|  `-C` | Зберігає розташування після вилучення файлів з архіву                                                                  |
|  `-P` | Зберігає в абсолютному режимі                                                                                          |

Для інших допоміжних опцій, не згаданих, див. `man 1 tar`

!!! warning "Різниця версії"

```
У деяких старіших версіях tar параметри називаються «ключами», що означає, що використання параметрів із префіксом «-» може призвести до того, що `tar` не працюватиме належним чином. На цьому етапі вам потрібно видалити префікс "-", щоб він працював належним чином.
```

### Про стилі варіантів

`tar` надає три опційні стилі:

1. Традиційний стиль:

  - `tar {A|c|d|r|t|u|x}[GnSkUWOmpsMBiajJzZhPlRvwo] [ARG...]`.

2. Використання стилю короткого варіанту:

  - `tar -A [OPTIONS] ARCHIVE ARCHIVE`
  - `tar -c [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar -d [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar -t [-f ARCHIVE] [OPTIONS] [MEMBER...]`
  - `tar -r [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar -u [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar -x [-f ARCHIVE] [OPTIONS] [MEMBER...]`

3. Використання стилю довгого варіанту:

  - `tar {--catenate|--concatenate} [OPTIONS] ARCHIVE ARCHIVE`
  - `tar --create [--file ARCHIVE] [OPTIONS] [FILE...]`
  - `tar {--diff|--compare} [--file ARCHIVE] [OPTIONS] [FILE...]`
  - `tar --delete [--file ARCHIVE] [OPTIONS] [MEMBER...]`
  - `tar --append [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar --list [-f ARCHIVE] [OPTIONS] [MEMBER...]`
  - `tar --test-label [--file ARCHIVE] [OPTIONS] [LABEL...]`
  - `tar --update [--file ARCHIVE] [OPTIONS] [FILE...]`
  - `tar --update [-f ARCHIVE] [OPTIONS] [FILE...]`
  - `tar {--extract|--get} [-f ARCHIVE] [OPTIONS] [MEMBER...]`

Другий метод є більш поширеним і відповідає звичкам більшості користувачів GNU/Linux.

### Ефективність компресії та частота використання

`tar` не має можливостей стиснення, тому його потрібно використовувати разом з іншими інструментами стиснення. Стиснення та декомпресія впливатиме на споживання ресурсів.

Ось рейтинг стиснення набору текстових файлів від найменшого до найбільш ефективного:

- compress (`.tar.Z`) - Менш популярний
- gzip (`.tar.gz` or `.tgz`) - Популярний
- bzip2 (`.tar.bz2` or `.tb2` or `.tbz`) - Популярний
- lzip (`.tar.lz`) - Менш популярний
- xz (`.tar.xz`) - Популярний

### Правила іменування для `tar`

Ось приклади іменування архівів `tar`:

| Основна функція та допоміжні опції | Файли   | Суфікс           | Функціональність                                |
| ---------------------------------- | ------- | ---------------- | ----------------------------------------------- |
| `-cvf`                             | `home`  | `home.tar`       | `/home` у відносному режимі, нестиснена форма   |
| `-cvfP`                            | `/etc`  | `etc.A.tar`      | `/etc` в абсолютному режимі, без стиснення      |
| `-cvfz`                            | `usr`   | `usr.tar.gz`     | `/usr` у відносному режимі, стиснення _gzip_    |
| `-cvfj`                            | `usr`   | `usr.tar.bz2`    | `/usr` у відносному режимі, стиснення _bzip2_   |
| `-cvfPz`                           | `/home` | `home.A.tar.gz`  | `/home` в абсолютному режимі, стиснення _gzip_  |
| `-cvfPj`                           | `/home` | `home.A.tar.bz2` | `/home` в абсолютному режимі, стиснення _bzip2_ |

Ви також можете додати дату до імені файлу.

### Приклад використання

#### Тип `-c`

1. Архівуйте та стискайте **/etc/** у відносному режимі з суфіксом `.tar.gz`:

  ```bash
  Shell > tar -czvf /tmp/etc-20241207.tar.gz /etc/
  ```

  Через те, що `tar` за замовчуванням працює у відносному режимі, перший рядок виводу команди відображатиме наступне:

  ```bash
  tar: Removing leading '/' from member names
  ```

2. Архівуйте **/var/log/** і виберіть тип xz для стиснення:

  ```bash
  Shell > tar -cJvf /tmp/log-20241207.tar.xz /var/log/

  Shell > du -sh /var/log/ ; ls -lh /tmp/log-20241207.tar.xz
  18M     /var/log/
  -rw-r--r-- 1 root root 744K Dec  7 14:40 /tmp/log-20241207.tar.xz
  ```

3. Оцініть розмір файлу без створення архіву:

  ```bash
  Shell > tar -cJf - /etc | wc -c
  tar: Removing leading `/' from member names
  3721884
  ```

  Одиницею виведення команди `wc -c` є байти.

4. Виріжте великі файли `.tar.gz`:

  ```bash
  Shell > cd /tmp/ ; tar -czf - /etc/  | split -d -b 2M - etc-backup20241207.tar.gz.

  Shell > ls -lh /tmp/
  -rw-r--r-- 1 root root 2.0M Dec  7 20:46 etc-backup20241207.tar.gz.00
  -rw-r--r-- 1 root root 2.0M Dec  7 20:46 etc-backup20241207.tar.gz.01
  -rw-r--r-- 1 root root 2.0M Dec  7 20:46 etc-backup20241207.tar.gz.02
  -rw-r--r-- 1 root root  70K Dec  7 20:46 etc-backup20241207.tar.gz.03
  ```

  Перший «-» представляє вхідні параметри `tar`, тоді як другий «-» повідомляє `tar` перенаправити вихід на `stdout`.

  Щоб витягнути ці вирізані невеликі файли, ви можете вказати таку операцію:

  ```bash
  Shell > cd /tmp/ ; cat etc-backup20241207.tar.gz.* >> /tmp/etc-backup-20241207.tar.gz

  Shell > cd /tmp/ ; tar -xvf etc-backup-20241207.tar.gz -C /tmp/dir1/
  ```

#### Тип `-x`

1. Завантажте вихідний код Redis і розпакуйте його в каталог `/usr/local/src/`：

  ```bash
  Shell > wget -c https://github.com/redis/redis/archive/refs/tags/7.4.1.tar.gz

  Shell > tar -xvf 7.4.1.tar.gz -C /usr/local/src/
  ```

2. Розпакуйте лише один файл із zip-архіву

  ```bash
  Shell > tar -xvf /tmp/etc-20241207.tar.gz etc/chrony.conf
  ```

#### Тип -A або -r

1. Додайте один файл `.tar` до іншого файлу `.tar`:

  ```bash
  Shell > tar -cvf /tmp/etc.tar /etc/

  Shell > tar -cvf /tmp/log.tar /var/log/

  Shell > tar -Avf /tmp/etc.tar /tmp/log.tar
  ```

  Це означає, що всі файли в "log.tar" будуть додані в кінець "etc.tar".

2. Додайте файли або каталоги до файлу `.tar`:

  ```bash
  Shell > tar -rvf /tmp/log.tar /etc/chrony.conf
  tar: Removing leading `/' from member names
  /etc/chrony.conf
  tar: Removing leading `/' from hard link targets

  Shell > tar -rvf /tmp/log.tar /tmp/dir1
  ```

!!! warning "Важливо"

```
Незалежно від того, чи використовуєте ви опцію «-A» чи «-r», враховуйте режим збереження відповідних архівних файлів.
```

!!! warning "Важливо"

```
`-A` і `-r` не застосовуються до заархівованих стиснених файлів.
```

#### тип `-t`

1. Перегляньте вміст архіву:

  ```bash
  Shell > tar -tvf /tmp/log.tar

  Shell > tar -tvf /tmp/etc-20241207.tar.gz | less
  ```

#### тип `-d`

1. Порівняйте відмінності файлів:

  ```bash
  Shell > cd / ; tar -dvf /tmp/etc.tar etc/chrony.conf
  etc/chrony.conf

  Shell > cd / ; tar -dvf /tmp/etc-20241207.tar.gz etc/
  ```

  Для методів зберігання, які використовують відносний режим, при використанні типу `-d` змініть шлях до файлу на '/'.

#### тип `-u`

1. Якщо існує кілька версій одного файлу, ви можете використовувати тип `-u`:

  ```bash
  Shell > touch /tmp/tmpfile1

  Shell > tar -rvf /tmp/log.tar /tmp/tmpfile1

  Shell > echo "File Name" >> /tmp/tmpfile1

  Shell > tar -uvf /tmp/log.tar /tmp/tmpfile1

  Shell > tar -tvf /tmp/log.tar
  ...
  -rw-r--r-- root/root         0 2024-12-07 18:53 tmp/tmpfile1
  -rw-r--r-- root/root        10 2024-12-07 18:54 tmp/tmpfile1
  ```

#### Тип `--delete`

1. Ви також можете використовувати `--delete`, щоб видалити файли всередині `.tar` файлу.

  ```bash
  Shell > tar --delete -vf /tmp/log.tar tmp/tmpfile1

  Shell > tar --delete -vf /tmp/etc.tar etc/motd.d/
  ```

  При видаленні ви видаляєте з архіву всі файли з однаковою назвою.

## Загальноприйнята термінологія

Деякі веб-сайти згадують два терміни:

- **tarfile** - Відноситься до нестиснутих архівних файлів, таких як файли `.tar`
- **tarball** - Відноситься до стислих архівних файлів, таких як `.tar.gz` і `.tar.xz`
