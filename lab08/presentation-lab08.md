---
title: "Поиск файлов. Перенаправление ввода-вывода. Просмотр запущенных процессов"
subtitle: "Лабораторная работа № 8"
author: "Чжао Синья (zhaoxinya)"
date: "2026-10-08"
theme: default
---

# Поиск файлов. Перенаправление ввода-вывода. Просмотр запущенных процессов

## Лабораторная работа № 8

**Студент:** Чжао Синья  
**Учётная запись:** zhaoxinya  
**Имя хоста:** zhaoxinya  
**Номер студенческого билета:** 113225832  
**Дисциплина:** Архитектура компьютеров и операционные системы

---

# Цель работы

Ознакомление с инструментами поиска файлов и фильтрации текстовых данных.

Приобретение практических навыков:

- по управлению процессами (и заданиями)
- по проверке использования диска
- по обслуживанию файловых систем

---

# Задание работы

1. Ознакомиться с перенаправлением ввода-вывода
2. Освоить работу с конвейерами (pipe)
3. Освоить команды поиска файлов (`find`) и фильтрации (`grep`)
4. Проверить использование диска (`df`, `du`)
5. Освоить управление задачами и процессами (`jobs`, `ps`, `kill`)

---

# Три стандартных потока

| Поток | Дескриптор | По умолчанию |
|-------|-----------|--------------|
| stdin | 0 | клавиатура |
| stdout | 1 | терминал |
| stderr | 2 | терминал |

---

# Перенаправление ввода-вывода

```bash
1>filename        # stdout → файл (перезапись)
1>>filename       # stdout → файл (добавление)
2>filename        # stderr → файл
2>>filename       # stderr → файл (добавление)
&>filename        # stdout и stderr → файл
```

### Пример

```bash
echo "hello" > test1.txt
echo "world" >> test1.txt
ls /etc/nonexistent 2> err.txt
ls /etc 2>&1 | head -3
```

---

# Конвейер (pipe)

Конвейер передаёт вывод одной команды на вход другой:

```bash
ls -la | sort > sorting_list
```

Разберём:

- `ls -la` — вывод
- `|` — конвейер
- `sort` — сортировка
- `> sorting_list` — запись в файл

---

# Команда find

```bash
find путь [-опции]
```

### Примеры

```bash
find ~ -name "f*" -print
find ~ -name "*.tmp" -exec rm {} \;
find ~ -type d | head -30
```

### Основные опции

- `-name шаблон` — по имени
- `-type f` — только файлы
- `-type d` — только директории
- `-exec ... {} \;` — выполнить команду

---

# Команда grep

```bash
grep строка имя_файла
```

### Примеры

```bash
grep begin f*
ls -l | grep lab
grep "\.conf" file.txt > conf.txt
```

### Основные опции

- `-i` — игнорировать регистр
- `-r` — рекурсивно
- `-v` — инвертировать результат

---

# Проверка использования диска

```bash
df -h
du -a ~
du -sh ~
```

### Результат

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        79G  4.4G   74G   6% /
/dev/sda2       974M  326M  581M  36% /boot
/dev/sda3        79G  4.4G   74G   6% /home

527M    /home/zhaoxinya
```

---

# Управление задачами

```bash
gedit &                          # запуск в фоне
jobs                             # список задач
kill %номер_задачи               # завершение задачи
```

### Пример

```bash
gnome-text-editor &
# [1] 13500
```

---

# Управление процессами

```bash
ps aux                           # список процессов
ps aux | grep gedit              # найти gedit
pgrep -a -f gnome-text-editor    # найти PID
kill -9 PID                      # завершить по PID
pkill -f имя                     # завершить по имени
```

### Пример

```bash
ps aux | grep gnome-text-editor | grep -v grep
# zhaoxin+  13500  ...  gnome-text-editor
```

---

# Соглашения об именовании

- Пользователь ВМ: `zhaoxinya`
- Имя хоста: `zhaoxinya`
- Имя виртуальной машины: `ZhaoXinya2`

Проверка:

```bash
whoami
hostname
id
```

---

# 2. Файл file.txt

```bash
cd ~
ls /etc > file.txt
ls ~ >> file.txt
head -30 file.txt
```

Результат: файл `file.txt` содержит:

- содержимое `/etc`
- содержимое домашнего каталога

---

# 3. Файл conf.txt

```bash
grep "\.conf" file.txt > conf.txt
cat conf.txt
```

Результат:

```
anthy-unicode.conf
asound.conf
brltty.conf
chrony.conf
...
xattr.conf
```

---

# 4. Файлы, начинающиеся с c

```bash
ls ~ | grep "^c"
find ~ -maxdepth 1 -name "c*"
```

Результат:

```
conf.txt
/home/zhaoxinya/conf.txt
```

---

# 5. Файлы /etc, начинающиеся с h

```bash
ls /etc | grep "^h" | less
```

Результат:

```
host.conf
hostname
hosts
hp
httpd
```

---

# 6. Фоновый поиск log

```bash
find / -name "log*" -print > ~/logfile 2>/dev/null &
```

Проверка:

```bash
jobs
ls -l ~/logfile
head -20 ~/logfile
```

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya 57546 ... logfile

/dev/log
/home/zhaoxinya/.local/share/keyrings/login.keyring
/home/zhaoxinya/.local/share/pnpm/...
```

---

# 7. Удаление ~/logfile

```bash
rm ~/logfile
ls ~/logfile
```

Результат:

```
ls: cannot access '/home/zhaoxinya/logfile': No such file or directory
```

---

# 8. Запуск gnome-text-editor в фоне

```bash
gnome-text-editor &
```

Результат:

```
[1] 13500
```

---

# 9. Определение PID

```bash
ps aux | grep gnome-text-editor | grep -v grep
pgrep -a -f gnome-text-editor
```

Результат:

```
zhaoxin+ 13500 ... gnome-text-editor
13500 gnome-text-editor
```

---

# 10. Завершение процесса

```bash
kill -9 13500
```

Результат:

```
[2]-  Killed              gnome-text-editor
```

Проверка:

```bash
pgrep -a -f gnome-text-editor
```

Вывод пуст — процесс завершён.

---

# 11. df и du

```bash
df -h
du -sh ~
```

Результат:

```
/dev/sda3   79G  4.4G  74G   6% /
/dev/sda2  974M  326M 581M  36% /boot
/dev/sda3   79G  4.4G  74G   6% /home

527M  /home/zhaoxinya
```

---

# 12. Поиск директорий

```bash
find ~ -type d | head -30
```

Результат:

```
/home/zhaoxinya
/home/zhaoxinya/.mozilla
/home/zhaoxinya/.mozilla/extensions
/home/zhaoxinya/.mozilla/plugins
/home/zhaoxinya/.mozilla/firefox
...
```

---

# Выводы

- Изучены перенаправление стандартных потоков и конвейеры
- Освоены команды поиска `find` и фильтрации `grep`
- Освоена проверка использования диска (`df`, `du`)
- Изучено управление задачами (`jobs`, `&`) и процессами (`ps`, `kill`)
- Практически выполнено удаление процесса по PID и по имени

---

# Ответы на контрольные вопросы (1/2)

**1. Какие потоки ввода-вывода вы знаете?**

`stdin` (0), `stdout` (1), `stderr` (2)

**2. Разница между > и >>?**

`>` — перезапись; `>>` — добавление.

**3. Что такое конвейер?**

Передаёт вывод одной команды на вход другой:

```bash
ls -la | sort
```

**4. Что такое процесс?**

Выполняющийся экземпляр программы.

**5. Что такое PID и GID?**

- PID — идентификатор процесса
- GID — идентификатор группы

---

# Ответы на контрольные вопросы (2/2)

**6. Что такое задачи?**

Фоновые процессы текущей оболочки. Управление: `jobs`, `kill %N`.

**7. top и http**

- `top` — интерактивный монитор процессов
- `httpd` — HTTP-сервер

**8. Команды поиска файлов**

`find` (по атрибутам) и `grep` (по содержимому).

**9. Поиск по контексту**

```bash
grep "текст" файл
```

**10. Объём свободной памяти**

```bash
df -h
```

**11. Объём домашнего каталога**

```bash
du -sh ~
```

**12. Удаление зависшего процесса**

```bash
kill -9 PID
pkill -9 имя
```

---

# Спасибо за внимание!

**Репозиторий курса:**  
https://github.com/Muzxnya/study_202602-202606_os-intro