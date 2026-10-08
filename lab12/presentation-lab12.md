% Лабораторная работа № 12
% Чжао Синья
% Программирование в командном процессоре ОС UNIX. Командные файлы

# Содержание

1. Цель работы
2. Задание работы
3. Теоретические сведения
4. Соглашения об именовании
5. Ход выполнения работы
6. Выводы
7. Ответы на контрольные вопросы
8. Библиография

# Цель работы

Изучить основы программирования в оболочке ОС UNIX/Linux.

Научиться писать и отлаживать командные файлы (shell-скрипты) на языке **bash**.

# Задание работы

1. Резервное копирование скрипта самого себя в `~/backup` (архиватор `tar`/`zip`/`bzip2`).
2. Обработка произвольного числа аргументов командной строки (в т.ч. > 10).
3. Аналог команды `ls` — без использования `ls` и `dir`.
4. Подсчёт файлов с заданным расширением в указанном каталоге.

# Теоретические сведения: оболочки

- `sh` — Bourne shell, базовая оболочка
- `csh` — C shell, C-подобный синтаксис
- `ksh` — Korn shell, совместим с sh
- `bash` — Bourne Again Shell, стандарт в Linux

**POSIX** — стандарт IEEE для совместимости UNIX/Linux-систем.

# Переменные и арифметика в bash

Переменные:

```bash
name=value
echo $name
```

Массивы:

```bash
arr=(a b c)
echo ${arr[0]}
echo ${arr[@]}
```

Арифметическая подстановка:

```bash
i=$((i + 1))
```

# Метасимволы и экранирование

- `*` — произвольная строка
- `?` — один символ
- `[c1-c2]` — диапазон
- `\` — экранирование одного символа
- `'...'` — буквально
- `"..."` — с подстановкой переменных

# Командные файлы: создание и запуск

```bash
touch script.sh
chmod +x script.sh
./script.sh
```

Позиционные параметры:

`$0 $1 $2 … $# $* $@ $? $$ $!`

# Управляющие конструкции

- `if ... then ... elif ... else ... fi`
- `case ... esac`
- `for ... do ... done`
- `while ... do ... done`
- `until ... do ... done`
- `break`, `continue`

# Команда getopts

```bash
while getopts "i:o:p:cn" opt; do
  case $opt in
    i) infile=$OPTARG;;
    o) outfile=$OPTARG;;
    p) pattern=$OPTARG;;
    C) case_sensitive=1;;
    n) show_line_numbers=1;;
  esac
done
```

# Соглашения об именовании

- Пользователь ВМ: `zhaoxinya`
- Имя хоста: `zhaoxinya`
- Имя виртуальной машины: `ZhaoXinya2`

Проверка:

```bash
whoami      # zhaoxinya
hostname    # zhaoxinya
id          # uid=1000(zhaoxinya) gid=1000(zhaoxinya) groups=1000(zhaoxinya),10(wheel)
```

# Задание 1. Резервное копирование

Создан файл `~/backup-self.sh`:

```bash
#!/bin/bash
BACKUP_DIR="$HOME/backup"
mkdir -p "$BACKUP_DIR"
SCRIPT_PATH=$(realpath "$0")
BACKUP_FILE="$BACKUP_DIR/backup-self-$(date +%Y%m%d-%H%M%S).tar.gz"
tar -czf "$BACKUP_FILE" "$SCRIPT_PATH"
echo "Backup completed:$BACKUP_FILE"
```

# Задание 1. Ошибка и её устранение

Возникла ошибка:

```
tar: Cowardly refusing to create an empty archive
```

**Причина:** между `"$BACKUP_FILE"` и `"$SCRIPT_PATH"` не было пробела.

**Решение:** добавить пробел и использовать `realpath "$0"`.

# Задание 1. Результат

```
tar: Removing leading `/' from member names
Backup completed:/home/zhaoxinya/backup/backup-self-20261008-142508.tar.gz
```

Проверка:

```
total 4
-rw-r--r--. 1 zhaoxinya zhaoxinya 302 Oct  8 14:25 backup-self-20261008-142508.tar.gz
```

# Задание 2. Произвольное число аргументов

```bash
#!/bin/bash
echo "Total arguments: $#"
i=1
for arg in "$@"; do
  echo "Argument $i: $arg"
  i=$((i + 1))
done
```

Запуск:

```bash
~/args.sh a b c d e f g h i j k l
```

# Задание 2. Результат

```
Total arguments: 12
Argument 1: a
Argument 2: b
...
Argument 11: k
Argument 12: l
```

`"$@"` корректно обрабатывает любое число аргументов, включая > 10.

# Задание 3. Аналог `ls`

```bash
#!/bin/bash
DIR="${1:-.}"
if [ ! -d "$DIR" ]; then
  echo "Error: $DIR is not a directory."
  exit 1
fi
echo "Contents of $DIR:"
for f in "$DIR"/* "$DIR"/.[!.]*; do
  [ -e "$f" ] || continue
  if [ -d "$f" ]; then type="d"
  elif [ -f "$f" ]; then type="-"
  else type="?"
  fi
  perms=$(stat -c %A "$f")
  echo "$type $perms $(basename "$f")"
done
```

# Задание 3. Результат (фрагмент)

```
Contents of /home/zhaoxinya:
- -rw-rw-r-- abc1
- -rwxr-xr-x args.sh
d drwxr-xr-x backup
- -rwxr-xr-x backup-self.sh
d drwxr-xr-x bin
- -rw-r--r-- conf.txt
d drwxr-xr-x Desktop
...
- -rw------- .bash_history
d drwx------ .ssh
```

Скрытые файлы тоже выводятся, `.` и `..` исключены.

# Задание 4. Подсчёт файлов по расширению

```bash
#!/bin/bash
if [ $# -ne 2 ]; then
  echo "Usage: $0 <directory> <extension>"
  exit 1
fi
DIR="$1"
EXT="$2"
if [ ! -d "$DIR" ]; then
  echo "Error: Not a directory: $DIR"
  exit 1
fi
COUNT=0
for f in "$DIR"/*."$EXT"; do
  [ -e "$f" ] || continue
  COUNT=$((COUNT + 1))
done
echo "Files with extension .$EXT in $DIR: $COUNT"
```

# Задание 4. Результат

```
Files with extension .sh in /home/zhaoxinya: 5
Files with extension .txt in /home/zhaoxinya: 4
```

Ошибка `echo"Files..."` (без пробела) была исправлена на `echo "Files..."`.

# Выводы

- Изучены переменные, арифметика, метасимволы, позиционные параметры.
- Освоены `if`, `case`, `for`, `while`, `until`.
- Написаны и отлажены 4 скрипта: `backup-self.sh`, `args.sh`, `myls.sh`, `count-ext.sh`.
- Устранены две типичные ошибки: пробел в `tar` и пробел между `echo` и строкой.

# Ответы на контрольные вопросы (1/4)

1. **Shell** — программа для взаимодействия с ОС. Примеры: `sh`, `csh`, `ksh`, `bash`.
2. **POSIX** — стандарт IEEE для совместимости UNIX/Linux.
3. **Переменные**: `name=value`. **Массивы**: `arr=(a b c)`.
4. **`let`** — арифметика, **`read`** — чтение ввода.
5. **Арифметические операторы**: `+ - * / % ** << >> & | ^ ~ && || !`

# Ответы на контрольные вопросы (2/4)

6. `$(( ))` — арифметическая подстановка.
7. **Стандартные переменные**: `HOME`, `PATH`, `PS1`, `PS2`, `IFS`, `TERM`, `LOGNAME`, `MAIL`, `PWD`, `USER`, `SHELL`.
8. **Метасимволы**: `* ? [ ] \ ' " ` $ ; & | < > ( ) { }`
9. **Экранирование**: `\`, `'...'`, `"..."`.

# Ответы на контрольные вопросы (3/4)

10. **Создание скрипта**: `touch`, `chmod +x`, `./`.
11. **Функции**: `name() { ... }` или `function name { ... }`.
12. **Проверка каталога**: `[ -d "$file" ]`.
13. **`set`** — переменные shell, **`typeset`** — атрибуты, **`unset`** — удаление.

# Ответы на контрольные вопросы (4/4)

14. **Параметры**: `$1`, `$2`, ..., `$#`, `$*`, `$@`.
15. **Специальные переменные**: `$?`, `$$`, `$!`, `$#`, `$*`, `$@`, `$-`, `$_`.

# Библиография

1. Курячий Г. В., Маслинский К. А. Операционная система Linux. — М.: Интуит.Ру, 2005. — 392 с.
2. Робачевский А. Операционная система UNIX. — СПб.: БХВ-Петербург, 2010. — 656 сс.
3. Robbins A. Bash Pocket Reference. — O'Reilly Media, 2016. — 156 сс.

# Спасибо за внимание!