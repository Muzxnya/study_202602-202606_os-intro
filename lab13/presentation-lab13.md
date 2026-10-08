% Лабораторная работа № 13
% Чжао Синья
% Программирование в командном процессоре ОС UNIX. Ветвления и циклы

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

Научиться писать более сложные командные файлы с использованием логических управляющих конструкций и циклов.

# Задание работы

1. Скрипт с `getopts` + `grep` (опции `-i -o -p -C -n`).
2. Программа на C (`exit(n)`) + скрипт, анализирующий `$?`.
3. Скрипт создания/удаления N файлов `1.tmp … N.tmp`.
4. Скрипт архивации `tar` + `find` (только файлы, изменённые за 7 дней).

# Теоретические сведения

- `getopts` — разбор опций командной строки.
- `grep` — поиск по шаблону, опции `-i -n -C -v`.
- Специальные переменные: `$?`, `$#`, `$1`, `$2`, ...
- Управляющие конструкции: `if`, `case`, `for`, `while`, `until`.
- `find -mtime -7` — файлы, изменённые за последние 7 дней.
- `tar -czf archive.tar.gz --null -T -` — приём списка файлов со stdin.

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

# Задание 1. mygrep.sh

```bash
#!/bin/bash
while getopts "i:o:p:Cn" opt; do
case $opt in
i) INFILE="$OPTARG";;
o) OUTFILE="$OPTARG";;
p) PATTERN="$OPTARG";;
C) CASE="-C";;
n) NUMS="-n";;
esac
done
if [ -z "$INFILE" ] || [ -z "$PATTERN" ]; then
echo "Usage: $0 -i input -p pattern [-o output] [-C] [-n]"
exit 1
fi
if [ -z "$OUTFILE" ]; then
grep $CASE $NUMS "$PATTERN" "$INFILE"
else
grep $CASE $NUMS "$PATTERN" "$INFILE" > "$OUTFILE"
echo "Result written to $OUTFILE"
fi
```

# Задание 1. Результат

```bash
~/mygrep.sh -i /etc/passwd -p root -n
```

```
1:root:x:0:0:Super User:/root:/bin/bash
10:operator:x:11:0:operator:/root:/usr/sbin/nologin
```

Предупреждение `[: missing ']'` было устранено добавлением пробела между `]` и `;`.

# Задание 2. checknum.c

```c
#include <stdio.h>
#include <stdlib.h>
int main() {
int n;
printf("Enter number: ");
scanf("%d", &n);
if (n > 0) exit(1);
else if (n < 0) exit(2);
else exit(0);
}
```

Компиляция:

```bash
gcc ~/checknum.c -o ~/checknum
```

# Задание 2. checknum.sh

```bash
#!/bin/bash
~/checknum
R=$?
case $R in
0) echo "Number is zero";;
1) echo "Number is positive";;
2) echo "Number is negative";;
*) echo "Unknown exit code: $R";;
esac
```

# Задание 2. Результат

```
Enter number: 5   → Number is positive
Enter number: -3  → Number is negative
Enter number: 0   → Number is zero
```

Скрипт использует `$?` для получения кода возврата, переданного функцией `exit(n)`.

# Задание 3. mkfiles.sh

```bash
#!/bin/bash
if [ $# -ne 2 ]; then
echo "Usage: $0 <count> <create|delete>"
exit 1
fi
N=$1
ACTION=$2
if [ "$ACTION" = "create" ]; then
for i in $(seq 1 $N); do
touch "$i.tmp"
done
echo "Created $N files"
elif [ "$ACTION" = "delete" ]; then
for i in $(seq 1 $N); do
rm -f "$i.tmp"
done
echo "Deleted $N files"
else
echo "Unknown action: $ACTION"
exit 1
fi
```

# Задание 3. Результат

```
~/mkfiles.sh 5 create
Created 5 files

ls *.tmp
1.tmp  2.tmp  3.tmp  4.tmp  5.tmp

~/mkfiles.sh 5 delete
Deleted 5 files

ls *.tmp
ls: cannot access '*.tmp': No such file or directory
```

# Задание 4. archdir.sh

```bash
#!/bin/bash
if [ $# -ne 2 ]; then
echo "Usage: $0 <directory> <archive.tar.gz>"
exit 1
fi
DIR="$1"
ARCH="$2"
if [ ! -d "$DIR" ]; then
echo "Error: $DIR is not a directory"
exit 1
fi
find "$DIR" -type f -mtime -7 -print0 | tar -czf "$ARCH" --null -T -
echo "Archive created: $ARCH"
```

# Задание 4. Результат

```
tar: Removing leading `/' from member names
tar: /home/zhaoxinya/ski.plases/feathers: Cannot open: Permission denied
Archive created: /tmp/mybackup.tar.gz

-rw-r--r--. 1 zhaoxinya zhaoxinya 540454939 Oct 8 16:32 /tmp/mybackup.tar.gz
```

`Permission denied` — файл `feathers` был умышленно закрыт на чтение в lab07. Архив создан успешно (~515 МБ).

# Выводы

- Изучен `getopts` для разбора ключей командной строки.
- Освоена связь C-программы и shell через `exit(n)` и `$?`.
- Закреплено использование `for`, `if/elif/else`, `case`.
- Освоены `find` и `tar` для выборочной архивации.

Написаны и отлажены 4 файла: `mygrep.sh`, `checknum.c`+`checknum.sh`, `mkfiles.sh`, `archdir.sh`.

# Ответы на контрольные вопросы (1/2)

1. **getopts** — разбор опций и их аргументов в командной строке.
2. **Метасимволы** — генерация имён файлов по шаблону (`* ? [ ]`).
3. **Операторы управления**: `if`, `case`, `&&`, `||`, сравнения.
4. **Прерывание цикла**: `break` (выход), `continue` (следующая итерация).

# Ответы на контрольные вопросы (2/2)

5. **true/false** — для бесконечных циклов и явных кодов возврата.
6. `if test -f man$s/$i.$s` — проверка существования обычного файла по склеенному пути.
7. **while** — пока истина; **until** — пока ложь.

# Библиография

1. Курячий Г. В., Маслинский К. А. Операционная система Linux. — М.: Интуит.Ру, 2005. — 392 с.
2. Робачевский А. Операционная система UNIX. — СПб.: БХВ-Петербург, 2010. — 656 сс.
3. Robbins A. Bash Pocket Reference. — O'Reilly Media, 2016. — 156 сс.

# Спасибо за внимание!