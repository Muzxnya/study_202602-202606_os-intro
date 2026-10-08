---
title: "Анализ файловой системы Linux"
subtitle: "Команды для работы с файлами и каталогами. Лабораторная работа № 7"
author: "Чжао Синья (zhaoxinya)"
date: "2026-10-07"
theme: default
---

# Анализ файловой системы Linux

## Команды для работы с файлами и каталогами

**Лабораторная работа № 7**

**Студент:** Чжао Синья  
**Учётная запись:** zhaoxinya  
**Имя хоста:** zhaoxinya  
**Номер студенческого билета:** 113225832  
**Дисциплина:** Архитектура компьютеров и операционные системы

---

# Цель работы

Ознакомление с файловой системой Linux, её структурой, именами и содержанием каталогов.

Приобретение практических навыков:

- по применению команд для работы с файлами и каталогами
- по управлению процессами (и работами)
- по проверке использования диска
- по обслуживанию файловой системы

---

# Задание работы

1. Ознакомиться с основными командами для работы с файлами и каталогами
2. Освоить команды копирования (`cp`), перемещения и переименования (`mv`)
3. Изучить права доступа и команды изменения прав (`chmod`)
4. Проанализировать файловую систему (`mount`, `/etc/fstab`, `df`, `du`, `fsck`)
5. Выполнить задания по работе с файлами и каталогами
6. Изучить основные опции команд `mount`, `fsck`, `mkfs`, `kill`

---

# Команды для просмотра файлов

### Создание пустого файла

```bash
touch имя_файла
```

### Просмотр небольших файлов

```bash
cat имя_файла
```

### Постраничный просмотр

```bash
less имя_файла
```

Клавиши: `Space` — вперёд, `Enter` — на строку, `b` — назад, `h` — справка, `q` — выход.

### Первые/последние строки

```bash
head [-n] имя_файла
tail [-n] имя_файла
```

---

# Команда cp

Копирование файлов и каталогов:

```bash
cp [-опции] исходный_файл целевой_файл
```

Опции:

- `-i` — запрос подтверждения при перезаписи
- `-r` — рекурсивное копирование каталогов

---

# Команда mv

Перемещение и переименование:

```bash
mv [-опции] старый_файл новый_файл
```

Опция:

- `-i` — запрос подтверждения перезаписи

---

# Права доступа

Каждый файл или каталог имеет права доступа:

- тип файла: `-` — файл, `d` — каталог
- права владельца: `r`, `w`, `x`, `-`
- права группы: то же
- права остальных: то же

### Примеры

```
-rw-r--r--    файл: владелец rw, группа r, остальные r
-rwx------    файл: владелец rwx, остальные без прав
drwxr-xr-x    каталог: владелец rwx, группа и остальные rx
```

---

# Команда chmod

```bash
chmod режим имя_файла
```

### Символьная запись

- `u` (user) — владелец
- `g` (group) — группа
- `o` (others) — остальные
- `+` добавить, `-` убрать, `=` установить
- `r`, `w`, `x` — чтение, запись, выполнение

### Числовая запись (восьмеричная)

| Двоичная | Восьмеричная | Символьная |
|----------|--------------|------------|
| 111 | 7 | rwx |
| 110 | 6 | rw- |
| 101 | 5 | r-x |
| 100 | 4 | r-- |
| 011 | 3 | -wx |
| 010 | 2 | -w- |
| 001 | 1 | --x |
| 000 | 0 | --- |

---

# Файловые системы Linux

Основные файловые системы:

- `ext2fs`, `ext3fs`, `ext4`
- `ReiserFS`, `xfs`
- `fat`, `ntfs`

### Команды анализа

```bash
mount                  # смонтированные ФС
cat /etc/fstab         # таблица монтирования
df -h                  # использование диска
du -sh ~               # объём каталога
fsck имя_устройства    # проверка ФС
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

Результат:

```
zhaoxinya
zhaoxinya
uid=1000(zhaoxinya) ...
```

---

# 1. Подготовка файлов

```bash
cd ~
touch abc1
echo "content of abc1" > abc1
ls -l abc1
```

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya 16 Oct  7 17:42 abc1
```

---

# 2. Команда cp

```bash
cd ~
cp abc1 april
cp abc1 may
ls -l april may

mkdir monthly
cp april may monthly
ls monthly

cp monthly/may monthly/june
ls monthly

mkdir monthly.00
cp -r monthly monthly.00
ls monthly.00

cp -r monthly.00 /tmp
ls /tmp | grep monthly
```

Результат:

- `april`, `may` созданы
- `monthly` содержит `april june may`
- `monthly.00` содержит `monthly`
- `/tmp/monthly.00` создан

---

# 3. Команда mv

```bash
cd ~
mv april july
mv july monthly.00
mv monthly.00 monthly.01
mkdir reports
mv monthly.01 reports
mv reports/monthly.01 reports/monthly
ls reports
```

Результат:

```
monthly
```

---

# 4. Права доступа (chmod)

```bash
cd ~
touch may
chmod u+x may
ls -l may          # -rwxr--r--

chmod u-x may
ls -l may          # -rw-r--r--
```

```bash
mkdir -p monthly_perm
chmod g-r,o-r monthly_perm
ls -ld monthly_perm    # drwx------

chmod g+w abc1
ls -l abc1             # -rw-rw-r--
```

---

# 5. Анализ файловой системы

### Смонтированные ФС

```bash
mount | head -20
```

Ключевые строки:

```
/dev/sda3 on / type btrfs (rw,relatime,...,subvol=/root)
/dev/sda2 on /boot type ext4
tmpfs on /tmp type tmpfs
```

### /etc/fstab

```
UUID=... /      btrfs  subvol=root,compress=zstd:1  0 0
UUID=... /boot  ext4   defaults                      1 2
UUID=... /home  btrfs  subvol=home,compress=zstd:1  0 0
```

---

# Использование диска

```bash
df -h
du -sh ~
```

Результат:

```
/dev/sda3   79G  4.4G  74G   6% /
/dev/sda2  974M  326M 581M  36% /boot
/dev/sda3   79G  4.4G  74G   6% /home

523M  /home/zhaoxinya
```

---

# 6. Задания: работа с файлами

```bash
# 3(a) копирование /etc/hosts в equipment
cp /etc/hosts ~/equipment

# 3(b) создание ski.plases
mkdir ~/ski.plases

# 3(c) перемещение equipment
mv ~/equipment ~/ski.plases/

# 3(d) переименование в equiplist
mv ~/ski.plases/equipment ~/ski.plases/equiplist

# 3(e) копирование abc1 в equiplist2
cp ~/abc1 ~/ski.plases/equiplist2

# 3(f) создание каталога equipment
mkdir ~/ski.plases/equipment

# 3(g) перемещение обоих файлов
mv ~/ski.plases/equiplist ~/ski.plases/equiplist2 ~/ski.plases/equipment/

# 3(h) перемещение newdir в plans
mv ~/newdir ~/ski.plases/plans

ls -R ~/ski.plases/
```

---

# Структура после задания 3

```
/home/zhaoxinya/ski.plases/:
equipment  plans

/home/zhaoxinya/ski.plases/equipment:
equiplist  equiplist2

/home/zhaoxinya/ski.plases/plans:
```

---

# 7. Права доступа (задание 4)

```bash
cd ~/ski.plases
mkdir -p australia play
touch my_os feathers

chmod 744 australia    # drwxr--r--
chmod 711 play         # drwx--x--x
chmod 544 my_os        # -r-xr--r--
chmod 664 feathers     # -rw-rw-r--
```

Проверка:

```
drwxr--r--.  australia
drwx--x--x.  play
-r-xr--r--.  my_os
-rw-rw-r--.  feathers
```

---

# 8. Задание 5

```bash
cd ~/ski.plases
cp feathers file.old
mv file.old play/
cp -r play fun
mv fun play/games
ls -R play/
```

Результат:

```
play/:
file.old  games

play/games:
file.old
```

---

# Отмена и восстановление прав

### Отмена чтения

```bash
chmod u-r feathers
ls -l feathers     # --w-rw-r--

cat feathers       # Permission denied
cp feathers feathers.copy   # Permission denied
```

### Восстановление

```bash
chmod u+r feathers
ls -l feathers     # -rw-rw-r--
```

### Отмена выполнения каталога

```bash
chmod u-x play
ls -ld play        # drw---x--x
cd play            # Permission denied

cd ~
chmod u+x ~/ski.plases/play
ls -ld ~/ski.plases/play   # drwx--x--x
```

---

# 9. Основные опции команд (задание 6)

| Команда | Назначение | Основные опции |
|---------|-----------|----------------|
| `mount` | Монтирование ФС | `-t` — тип, `-o` — опции, `-a` — все |
| `fsck` | Проверка/восстановление ФС | `-y` — все «да», `-f` — принудительно, `-A` — все |
| `mkfs` | Создание ФС | `-t` — тип, `-L` — метка |
| `kill` | Отправка сигнала процессу | `-9` — SIGKILL, `-15` — SIGTERM |

---

# Выводы

В ходе лабораторной работы были изучены:

- файловая система Linux (`btrfs` для `/` и `/home`, `ext4` для `/boot`)
- основные команды работы с файлами и каталогами: `touch`, `cat`, `less`, `head`, `tail`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`
- команды изменения прав доступа `chmod` (символьная и числовая запись)
- команды анализа файловой системы: `mount`, `/etc/fstab`, `df`, `du`, `fsck`
- влияние прав доступа на выполнение операций (`cat`, `cp`, `cd`)
- основные опции команд `mount`, `fsck`, `mkfs`, `kill`

Практически подтверждено влияние прав доступа на возможность чтения файлов, копирования и входа в каталог.

---

# Ответы на контрольные вопросы (1/2)

**1. Что такое файловая система?**

Часть ОС, обеспечивающая организацию, хранение и доступ к файлам. Примеры: `ext4`, `btrfs`, `xfs`, `ntfs`, `fat`.

**2. Команды для просмотра файлов:**

```bash
cat file
less file
head -n file
tail -n file
```

**3. Как создать каталог:**

```bash
mkdir dir
mkdir -p dir1/dir2/dir3
mkdir -m 755 dir
```

**4. Как скопировать файл или каталог:**

```bash
cp file1 file2
cp -r dir1 dir2
cp -i file1 file2
```

---

# Ответы на контрольные вопросы (2/2)

**5. Перемещение/переименование:**

```bash
mv old new
mv file dir/
```

**6. Удаление:**

```bash
rm file
rm -r dir
rmdir dir
```

**7. Изменение прав:**

```bash
chmod 755 file
chmod u+x file
chmod g-r file
```

**8. Права доступа** — определяют действия владельца, группы и остальных. Числовая запись: `r=4`, `w=2`, `x=1`.

**9. Просмотр смонтированных ФС:**

```bash
mount
cat /proc/mounts
df -h
cat /etc/fstab
```

**10. Объём каталога:**

```bash
du -sh ~
du -a ~
```

---

# Спасибо за внимание!

**Репозиторий курса:**  
https://github.com/Muzxnya/study_202602-202606_os-intro