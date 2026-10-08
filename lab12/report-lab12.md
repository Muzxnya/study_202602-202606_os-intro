# Лабораторная работа № 12

## Программирование в командном процессоре ОС UNIX. Командные файлы

**Студент:** Чжао Синья  
**Учётная запись:** zhaoxinya  
**Имя хоста:** zhaoxinya  
**Номер студенческого билета:** 113225832  
**Дисциплина:** Архитектура компьютеров и операционные системы. Операционные системы

---

## Содержание

1. [Цель работы](#цель-работы)
2. [Задание работы](#задание-работы)
3. [Теоретические сведения](#теоретические-сведения)
4. [Соглашения об именовании](#соглашения-об-именовании)
5. [Ход выполнения работы](#ход-выполнения-работы)
6. [Выводы](#выводы)
7. [Ответы на контрольные вопросы](#ответы-на-контрольные-вопросы)
8. [Библиография](#библиография)

---

## Цель работы

Изучить основы программирования в оболочке ОС UNIX/Linux. Научиться писать и отлаживать командные файлы (shell-скрипты) на языке bash.

---

## Задание работы

1. Написать командный файл, выполняющий резервное копирование самого себя в каталог `~/backup` с использованием архиватора (`tar`, `zip` или `bzip2`).
2. Написать командный файл, обрабатывающий произвольное число аргументов командной строки, в том числе превышающее десять.
3. Написать командный файл — аналог команды `ls` (без использования `ls` и `dir`), выводящий информацию о каталоге и правах доступа к файлам.
4. Написать командный файл, который принимает в качестве аргументов путь к каталогу и расширение файла, и подсчитывает количество файлов с указанным расширением.

---

## Теоретические сведения

### Командные оболочки

В UNIX/Linux наиболее распространены:

- `sh` — Bourne shell, базовая оболочка;
- `csh` — C shell, C-подобный синтаксис, история команд;
- `ksh` — Korn shell, совместим с sh, расширен csh;
- `bash` — Bourne Again Shell, стандартная оболочка большинства Linux-систем.

### POSIX

POSIX (Portable Operating System Interface) — набор стандартов IEEE, обеспечивающих совместимость UNIX/Linux-подобных систем и переносимость прикладных программ на уровне исходного кода.

### Переменные в bash

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

### Арифметические операторы

| Оператор       | Описание   |
| -------------- | ---------- |
| `+ - * / % **` | арифметика |
| `<< >>`        | сдвиги     |
| `& \| ^ ~`     | битовые    |
| `&& \|\| !`    | логические |

### Арифметическая подстановка

```bash
i=$((i + 1))
```

### Стандартные переменные

`HOME`, `PATH`, `PS1`, `PS2`, `IFS`, `TERM`, `LOGNAME`, `MAIL`, `PWD`, `USER`, `SHELL`.

### Метасимволы

- `*` — произвольная строка, в том числе пустая;
- `?` — один произвольный символ;
- `[c1-c2]` — диапазон символов;
- `\` — экранирование;
- `'...'` — буквальное значение;
- `"..."` — с подстановкой переменных.

### Командные файлы

Создание и запуск:

```bash
touch script.sh
chmod +x script.sh
./script.sh
```

### Позиционные параметры

`$0`, `$1`, `$2`, ..., `$#`, `$*`, `$@`, `$?`, `$$`, `$!`.

### Управляющие конструкции

- `if ... then ... elif ... else ... fi`
- `case ... esac`
- `for ... do ... done`
- `while ... do ... done`
- `until ... do ... done`
- `break`, `continue`

### Команда getopts

```bash
while getopts "i:o:p:Cn" opt; do
  case $opt in
    i) infile=$OPTARG;;
    o) outfile=$OPTARG;;
    p) pattern=$OPTARG;;
    C) case_sensitive=1;;
    n) show_line_numbers=1;;
  esac
done
```

---

## Соглашения об именовании

- Пользователь ВМ: `zhaoxinya`
- Имя хоста: `zhaoxinya`
- Имя виртуальной машины: `ZhaoXinya2`

Проверка:

```bash
whoami
hostname
id
```

![屏幕截图 2026-10-06 234112.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-06%20234112.png)

---

## Ход выполнения работы

### Задание 1. Резервное копирование скрипта

Создан файл `~/backup-self.sh` с помощью редактора `nano`:

```bash
nano ~/backup-self.sh
```

Исходный код:

```bash
#!/bin/bash
BACKUP_DIR="$HOME/backup"
mkdir -p "$BACKUP_DIR"
SCRIPT_PATH=$(realpath "$0")
BACKUP_FILE="$BACKUP_DIR/backup-self-$(date +%Y%m%d-%H%M%S).tar.gz"
tar -czf "$BACKUP_FILE" "$SCRIPT_PATH"
echo "Backup completed:$BACKUP_FILE"
```

![屏幕截图 2026-10-08 142422.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20142422.png)

Установлены права на выполнение:

```bash
chmod +x ~/backup-self.sh
```

Скрипт выполнен:

```bash
~/backup-self.sh
```

Первоначально при запуске возникала ошибка:

```
tar: Cowardly refusing to create an empty archive
```

Причина ошибки — в команде `tar` между двумя аргументами `"$BACKUP_FILE"` и `"$SCRIPT_PATH"` отсутствовал пробел, из-за чего они склеивались в одно имя. После добавления пробела и использования `realpath "$0"` для получения абсолютного пути скрипт заработал корректно.

Результат выполнения:

```
tar: Removing leading `/' from member names
Backup completed:/home/zhaoxinya/backup/backup-self-20261008-142508.tar.gz
```

Проверка содержимого каталога резервных копий:

```bash
ls -l ~/backup/
```

Результат:

```
total 4
-rw-r--r--. 1 zhaoxinya zhaoxinya 302 Oct  8 14:25 backup-self-20261008-142508.tar.gz
```

Сообщение `tar: Removing leading '/' from member names` не является ошибкой — это стандартное поведение `tar`, который удаляет ведущий `/` из путей внутри архива, чтобы распаковка не перезаписывала абсолютные пути.

![屏幕截图 2026-10-08 142536.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20142536.png)

### Задание 2. Обработка произвольного числа аргументов

Создан файл `~/args.sh`:

```bash
nano ~/args.sh
```

Исходный код:

```bash
#!/bin/bash
echo "Total arguments: $#"
i=1
for arg in "$@"; do
  echo "Argument $i: $arg"
  i=$((i + 1))
done
```

![屏幕截图 2026-10-08 135854.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20135854.png)

Установлены права и выполнен запуск с 12 аргументами:

```bash
chmod +x ~/args.sh
~/args.sh a b c d e f g h i j k l
```

Результат:

```
Total arguments: 12
Argument 1: a
Argument 2: b
Argument 3: c
Argument 4: d
Argument 5: e
Argument 6: f
Argument 7: g
Argument 8: h
Argument 9: i
Argument 10: j
Argument 11: k
Argument 12: l
```

Использование `"$@"` позволяет корректно обрабатывать любое количество аргументов, включая более десяти, и сохранять аргументы с пробелами как единое целое.

![屏幕截图 2026-10-08 140316.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20140316.png)

### Задание 3. Аналог команды ls

Создан файл `~/myls.sh`:

```bash
nano ~/myls.sh
```

Исходный код:

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

![屏幕截图 2026-10-08 144012.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20144012.png)

![屏幕截图 2026-10-08 144021.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20144021.png)

Установлены права и выполнен запуск:

```bash
chmod +x ~/myls.sh
~/myls.sh ~
```

Результат (фрагмент):

```
Contents of /home/zhaoxinya:
- -rw-rw-r-- abc1
- -rwxr-xr-x args.sh
d drwxr-xr-x backup
- -rwxr-xr-x backup-self.sh
d drwxr-xr-x bin
- -rw-r--r-- conf.txt
d drwxr-xr-x Desktop
d drwxr-xr-x Documents
d drwxr-xr-x Downloads
- -rw-r--r-- err.txt
- -rw-r--r-- file.txt
- -rw-r--r-- lab07.sh
- -rw-r--r-- lab07.sh~
- -rw-r--r-- LICENSE
- -rw-r--r-- may
d drwxr-xr-x monthly
d drwx--x--x monthly_perm
d drwxr-xr-x Music
- -rwxr-xr-x myls.sh
d drwxr-xr-x Pictures
d drwxr-xr-x Public
d drwxr-xr-x reports
d drwxr-xr-x ski.plases
d drwxr-xr-x STUD1
d drwxr-xr-x Templates
- -rw-r--r-- test1.txt
d drwxr-xr-x Videos
d drwxr-xr-x work
- -rw------- .bash_history
- -rw-r--r-- .bash_logout
- -rw-r--r-- .bash_profile
- -rw-r--r-- .bashrc
d drwxr-xr-x .bashrc.d
d drwx------ .cache
d drwxr-xr-x .config
d drwx------ .emacs.d
- -rw-r--r-- .gitconfig
d drwx------ .gnupg
- -rw-r--r-- .gtkrc-2.0
d drwxr-xr-x .local
d drwxr-xr-x .mozilla
d drwx------ .password-store
d drwx------ .ssh
- -rw-r--r-- .vimrc
- -rw-r--r-- .XCompose
```

Скрипт корректно обрабатывает как обычные, так и скрытые файлы (`.` и `..` исключены шаблоном `.[!.]*`), выводит тип (`d` — каталог, `-` — файл), права доступа и имя файла.

![屏幕截图 2026-10-08 144205.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20144205.png)

![屏幕截图 2026-10-08 144219.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20144219.png)

![屏幕截图 2026-10-08 144230.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20144230.png)

### Задание 4. Подсчёт файлов по расширению

Создан файл `~/count-ext.sh`:

```bash
nano ~/count-ext.sh
```

Исходный код:

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

![屏幕截图 2026-10-08 145355.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20145355.png)

![屏幕截图 2026-10-08 145731.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20145731.png)

Установлены права и выполнены проверки:

```bash
chmod +x ~/count-ext.sh
~/count-ext.sh ~ sh
~/count-ext.sh ~ txt
```

Результат:

```
Files with extension .sh in /home/zhaoxinya: 5
Files with extension .txt in /home/zhaoxinya: 4
```

Первоначально в последней строке была ошибка `echo"Files..."` (без пробела между `echo` и `"`), из-за чего bash пытался выполнить команду с именем `echoFiles...`. После добавления пробела скрипт стал работать корректно.

![屏幕截图 2026-10-08 145842.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-08%20145842.png)

---

## Выводы

В ходе лабораторной работы были изучены:

- переменные в bash и оператор `$`;
- арифметические операции через `$(( ))`;
- стандартные переменные оболочки;
- метасимволы и их экранирование;
- создание и запуск командных файлов;
- позиционные параметры и команда `getopts`;
- управляющие конструкции (`if`, `case`, `for`, `while`, `until`).

Написаны и отлажены 4 командных файла: `backup-self.sh`, `args.sh`, `myls.sh`, `count-ext.sh`. В процессе работы были выявлены и устранены две типичные ошибки: отсутствие пробела между аргументами `tar` и отсутствие пробела между `echo` и строкой. Обе ошибки связаны с особенностями разбора командной строки bash.

---

## Ответы на контрольные вопросы

### 1. Понятие командной оболочки

Командная оболочка (shell) — программа, обеспечивающая взаимодействие пользователя с операционной системой через команды. Примеры: `sh`, `csh`, `ksh`, `bash`.

### 2. Что такое POSIX?

POSIX — набор стандартов описания интерфейсов взаимодействия ОС и прикладных программ, разработан IEEE для совместимости UNIX/Linux-подобных систем.

### 3. Как определяются переменные и массивы в bash?

Переменные:

```bash
name=value
```

Массивы:

```bash
arr=(a b c)
echo ${arr[0]}
echo ${arr[@]}
```

### 4. Назначение операторов `let` и `read`

- `let` — выполнение арифметических операций над переменными;
- `read` — чтение строки со стандартного ввода и присваивание её переменной.

### 5. Арифметические операторы в bash

`+ - * / % ** << >> & | ^ ~ && || !`.

### 6. Что означает `$(( ))`?

Арифметическая подстановка: содержимое вычисляется как арифметическое выражение, результат подставляется в командную строку.

### 7. Стандартные имена переменных

`HOME`, `PATH`, `PS1`, `PS2`, `IFS`, `TERM`, `LOGNAME`, `MAIL`, `PWD`, `OLDPWD`, `SHELL`, `USER`.

### 8. Что такое метасимволы?

Символы, имеющие специальное значение для shell: `* ? [ ] \ ' " ` $ ; & | < > ( ) { }`.

### 9. Как экранировать метасимволы?

- `\` перед символом — экранирует один символ;
- `'...'` — экранирует всё содержимое;
- `"..."` — экранирует всё, кроме `$`, `` ` ``, `\`, `!`.

### 10. Как создавать и запускать командные файлы?

```bash
touch script.sh
chmod +x script.sh
./script.sh
```

### 11. Как определяются функции в bash?

```bash
function name {
  commands
}
# или
name() {
  commands
}
```

### 12. Как выяснить, является ли файл каталогом?

```bash
[ -d "$file" ] && echo "directory"
```

### 13. Назначение команд `set`, `typeset`, `unset`

- `set` — показать/установить переменные и опции shell;
- `typeset` — объявить переменную с атрибутами;
- `unset` — удалить переменную.

### 14. Как передаются параметры в командный файл?

Позиционные параметры: `$1`, `$2`, ..., `$#`, `$*`, `$@`.

### 15. Специальные переменные bash

`$?` — код завершения последней команды, `$$` — PID текущего shell, `$!` — PID последнего фонового процесса, `$#` — число аргументов, `$*`/`$@` — все аргументы, `$-` — флаги shell, `$_` — последний аргумент предыдущей команды.

---

## Библиография

1. Курячий Г. В., Маслинский К. А. Операционная система Linux. — М.: Интуит.Ру, 2005. — 392 с.
2. Робачевский А. Операционная система UNIX. — СПб.: БХВ-Петербург, 2010. — 656 сс.
3. Robbins A. Bash Pocket Reference. — O'Reilly Media, 2016. — 156 сс.