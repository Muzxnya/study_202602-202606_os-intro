# Лабораторная работа № 7

## Анализ файловой системы Linux. Команды для работы с файлами и каталогами

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

Ознакомление с файловой системой Linux, её структурой, именами и содержанием каталогов. Приобретение практических навыков по применению команд для работы с файлами и каталогами, по управлению процессами (и работами), по проверке использования диска и обслуживанию файловой системы.

---

## Задание работы

1. Ознакомиться с основными командами для работы с файлами и каталогами.
2. Освоить команды копирования (`cp`), перемещения и переименования (`mv`).
3. Изучить права доступа и команды изменения прав (`chmod`).
4. Проанализировать файловую систему (`mount`, `/etc/fstab`, `df`, `du`, `fsck`).
5. Выполнить задания по работе с файлами и каталогами.
6. Изучить основные опции команд `mount`, `fsck`, `mkfs`, `kill`.

---

## Теоретические сведения

### Команда touch

Создание пустого файла:

```bash
touch имя_файла
```

### Команда cat

Просмотр небольших файлов:

```bash
cat имя_файла
```

### Команда less

Постраничный просмотр файлов:

```bash
less имя_файла
```

Клавиши: `Space` — вперёд, `Enter` — на строку, `b` — назад, `h` — справка, `q` — выход.

### Команды head и tail

```bash
head [-n] имя_файла
tail [-n] имя_файла
```

По умолчанию выводят первые/последние 10 строк.

### Команда cp

Копирование файлов и каталогов:

```bash
cp [-опции] исходный_файл целевой_файл
```

Опции:

- `-i` — запрос подтверждения при перезаписи;
- `-r` — рекурсивное копирование каталогов.

### Команда mv

Перемещение и переименование:

```bash
mv [-опции] старый_файл новый_файл
```

Опция `-i` — запрос подтверждения перезаписи.

### Права доступа

Каждый файл или каталог имеет права доступа:

- тип файла: `-` — файл, `d` — каталог;
- права владельца: `r`, `w`, `x`, `-`;
- права группы: то же;
- права остальных: то же.

Примеры:

```
-rw-r--r--   файл: владелец rw, группа r, остальные r
-rwx------   файл: владелец rwx, остальные без прав
drwxr-xr-x   каталог: владелец rwx, группа и остальные rx
```

### Команда chmod

```bash
chmod режим имя_файла
```

Символьная запись:

- `u` (user) — владелец;
- `g` (group) — группа;
- `o` (others) — остальные;
- `+` — добавить право, `-` — убрать, `=` — установить;
- `r`, `w`, `x` — чтение, запись, выполнение.

Числовая запись (восьмеричная):

| Двоичная | Восьмеричная | Символьная |
| -------- | ------------ | ---------- |
| 111      | 7            | rwx        |
| 110      | 6            | rw-        |
| 101      | 5            | r-x        |
| 100      | 4            | r--        |
| 011      | 3            | -wx        |
| 010      | 2            | -w-        |
| 001      | 1            | --x        |
| 000      | 0            | ---        |

### Файловые системы

Основные файловые системы Linux: ext2fs, ext3fs, ext4, ReiserFS, xfs, fat, ntfs.

Просмотр смонтированных файловых систем:

```bash
mount
```

Просмотр `/etc/fstab`:

```bash
cat /etc/fstab
```

Проверка использования диска:

```bash
df
df -h
```

Объём каталога:

```bash
du -a ~
du -sh ~
```

Проверка целостности файловой системы:

```bash
fsck имя_устройства
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

### 1. Подготовка файлов

```bash
cd ~
touch abc1
echo "content of abc1" > abc1
ls -l abc1
```

![屏幕截图 2026-10-07 174541.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20174541.png)

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya 16 Oct  7 17:42 abc1
```

### 2. Команда cp

```bash
cd ~
cp abc1 april
cp abc1 may
ls -l april may
```

![屏幕截图 2026-10-07 174812.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20174812.png)

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya 16 Oct 7 17:46 april
-rw-r--r--. 1 zhaoxinya zhaoxinya 16 Oct 7 17:47 may
```

```bash
mkdir monthly
cp april may monthly
ls monthly
```

![屏幕截图 2026-10-07 174932.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20174932.png)

Результат:

```
april  may
```

```bash
cp monthly/may monthly/june
ls monthly
```

![屏幕截图 2026-10-07 180429.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20180429.png)

Результат:

```
april  june  may
```

```bash
mkdir monthly.00
cp -r monthly monthly.00
ls monthly.00
```

![屏幕截图 2026-10-07 175705.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20175705.png)

Результат:

```
monthly
```

```bash
cp -r monthly.00 /tmp
ls /tmp | grep monthly
```

![屏幕截图 2026-10-07 180924.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20180924.png)

Результат:

```
monthly.00
```

### 3. Команда mv

```bash
cd ~
mv april july
ls -l july
```

![屏幕截图 2026-10-07 181155.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181155.png)

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya 16 Oct 7 17:46 july
```

```bash
mv july monthly.00
ls monthly.00
```

![屏幕截图 2026-10-07 181309.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181309.png)

Результат:

```
july  monthly
```

```bash
mv monthly.00 monthly.01
mkdir reports
mv monthly.01 reports
mv reports/monthly.01 reports/monthly
ls reports
```

![屏幕截图 2026-10-07 181644.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181644.png)

Результат:

```
monthly
```

### 4. Права доступа (chmod)

```bash
cd ~
touch may
chmod u+x may
ls -l may
chmod u-x may
ls -l may
```

![屏幕截图 2026-10-07 181818.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181818.png)

![屏幕截图 2026-10-07 181857.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181857.png)

![屏幕截图 2026-10-07 181924.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20181924.png)

Результат:

```
-rw-r--r--. 1 zhaoxinya zhaoxinya ... may
```

```bash
mkdir -p monthly_perm
chmod g-r,o-r monthly_perm
ls -ld monthly_perm
```

![屏幕截图 2026-10-07 182130.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182130.png)

Результат:

```
drwx------. ... monthly_perm
```

```bash
chmod g+w abc1
ls -l abc1
```

![屏幕截图 2026-10-07 182209.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182209.png)

Результат:

```
-rw-rw-r--. ... abc1
```

### 5. Анализ файловой системы

Просмотр смонтированных файловых систем:

```bash
mount | head -20
```

![屏幕截图 2026-10-07 182356.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182356.png)

![屏幕截图 2026-10-07 182409.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182409.png)

![屏幕截图 2026-10-07 182419.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182419.png)

Ключевые строки:

```
/dev/sda3 on / type btrfs (rw,relatime,...)
/dev/sda2 on /boot type ext4 ...
tmpfs on /tmp type tmpfs ...
```

Просмотр `/etc/fstab`:

```bash
cat /etc/fstab
```

![屏幕截图 2026-10-07 182433.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182433.png)

![屏幕截图 2026-10-07 182443.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182443.png)

Ключевые строки:

```
UUID=... /      btrfs  subvol=root,compress=zstd:1  0 0
UUID=... /boot  ext4   defaults                      1 2
UUID=... /home  btrfs  subvol=home,compress=zstd:1  0 0
```

Использование диска:

```bash
df -h
```

![屏幕截图 2026-10-07 182505.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182505.png)

Результат:

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        79G  4.4G   74G   6% /
/dev/sda2       974M  326M  581M  36% /boot
/dev/sda3        79G  4.4G   74G   6% /home
```

Объём домашнего каталога:

```bash
du -sh ~
```

![屏幕截图 2026-10-07 182527.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182527.png)

Результат:

```
523M    /home/zhaoxinya
```

### 6. Задания по работе с файлами и каталогами

#### 3(a) Копирование файла

```bash
cp /etc/hosts ~/equipment
ls -l ~/equipment
```

![屏幕截图 2026-10-07 182716.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182716.png)

#### 3(b) Создание каталога ski.plases

```bash
mkdir ~/ski.plases
```

![屏幕截图 2026-10-07 182819.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182819.png)

#### 3(c) Перемещение файла в ski.plases

```bash
mv ~/equipment ~/ski.plases/
```

![屏幕截图 2026-10-07 182928.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20182928.png)

#### 3(d) Переименование в equiplist

```bash
mv ~/ski.plases/equipment ~/ski.plases/equiplist
```

![屏幕截图 2026-10-07 183133.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20183133.png)

#### 3(e) Копирование abc1 в equiplist2

```bash
cp ~/abc1 ~/ski.plases/equiplist2
```

![屏幕截图 2026-10-07 183359.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20183359.png)

#### 3(f) Создание каталога equipment

```bash
mkdir ~/ski.plases/equipment
```

![屏幕截图 2026-10-07 183527.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20183527.png)

#### 3(g) Перемещение файлов в equipment

```bash
mv ~/ski.plases/equiplist ~/ski.plases/equiplist2 ~/ski.plases/equipment/
ls -R ~/ski.plases/
```

![屏幕截图 2026-10-07 183819.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20183819.png)

Результат:

```
/home/zhaoxinya/ski.plases/:
equipment  plans

/home/zhaoxinya/ski.plases/equipment:
equiplist  equiplist2

/home/zhaoxinya/ski.plases/plans:
```

#### 3(h) Перемещение newdir в plans

```bash
mv ~/newdir ~/ski.plases/plans
ls -R ~/ski.plases/
```

![屏幕截图 2026-10-07 183959.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20183959.png)

### 7. Права доступа (задание 4)

```bash
cd ~/ski.plases
mkdir -p australia play
touch my_os feathers

chmod 744 australia   # drwxr--r--
chmod 711 play        # drwx--x--x
chmod 544 my_os       # -r-xr--r--
chmod 664 feathers    # -rw-rw-r--
```

![屏幕截图 2026-10-07 184225.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184225.png)

![屏幕截图 2026-10-07 184320.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184320.png)

![屏幕截图 2026-10-07 184350.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184350.png)

![屏幕截图 2026-10-07 184452.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184452.png)

![屏幕截图 2026-10-07 184536.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184536.png)

Проверка:

```
drwxr--r--. australia
drwx--x--x. play
-r-xr--r--. my_os
-rw-rw-r--. feathers
```

### 8. Задание 5

```bash
cd ~/ski.plases
cp feathers file.old
mv file.old play/
cp -r play fun
mv fun play/games
ls -R play/
```

![屏幕截图 2026-10-07 184920.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20184920.png)

![屏幕截图 2026-10-07 185047.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185047.png)

![屏幕截图 2026-10-07 185236.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185236.png)

![屏幕截图 2026-10-07 185329.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185329.png)

![屏幕截图 2026-10-07 185422.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185422.png)

Результат:

```
play/:
file.old  games

play/games:
file.old
```

Отмена права чтения:

```bash
chmod u-r feathers
ls -l feathers
```

![屏幕截图 2026-10-07 185532.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185532.png)

Результат:

```
--w-rw-r--. feathers
```

```bash
cat feathers
```

![屏幕截图 2026-10-07 185546.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185546.png)

Результат:

```
cat: feathers: Permission denied
```

```bash
cp feathers feathers.copy
```

![屏幕截图 2026-10-07 185640.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185640.png)

Результат:

```
cp: cannot open 'feathers' for reading: Permission denied
```

Восстановление права чтения:

```bash
chmod u+r feathers
ls -l feathers
```

![屏幕截图 2026-10-07 185720.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185720.png)

Результат:

```
-rw-rw-r--. feathers
```

Отмена права выполнения для каталога:

```bash
chmod u-x play
ls -ld play
```

![屏幕截图 2026-10-07 185720.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185720.png)

Результат:

```
drw---x--x. play
```

```bash
cd play
```

![屏幕截图 2026-10-07 185735.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185735.png)

Результат:

```
bash: cd: play: Permission denied
```

Восстановление:

```bash
cd ~
chmod u+x ~/ski.plases/play
ls -ld ~/ski.plases/play
```

![屏幕截图 2026-10-07 185846.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185846.png)

Результат:

```
drwx--x--x. /home/zhaoxinya/ski.plases/play
```

### 9. Основные опции команд (задание 6)

| Команда | Назначение                                 | Основные опции                                    |
| ------- | ------------------------------------------ | ------------------------------------------------- |
| `mount` | Монтирование файловой системы              | `-t` — тип, `-o` — опции, `-a` — все              |
| `fsck`  | Проверка и восстановление файловой системы | `-y` — все «да», `-f` — принудительно, `-A` — все |
| `mkfs`  | Создание файловой системы                  | `-t` — тип, `-L` — метка                          |
| `kill`  | Отправка сигнала процессу                  | `-9` — SIGKILL, `-15` — SIGTERM                   |

![屏幕截图 2026-10-07 185935.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185935.png)

![屏幕截图 2026-10-07 185948.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20185948.png)

![屏幕截图 2026-10-07 190001.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20190001.png)

![屏幕截图 2026-10-07 190014.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20190014.png)

---

## Выводы

В ходе лабораторной работы были изучены:

- файловая система Linux (btrfs для корня и `/home`, ext4 для `/boot`);
- основные команды работы с файлами и каталогами: `touch`, `cat`, `less`, `head`, `tail`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`;
- команды изменения прав доступа `chmod` (символьная и числовая запись);
- команды анализа файловой системы: `mount`, `/etc/fstab`, `df`, `du`, `fsck`;
- работа с правами доступа: изменение прав, влияние отсутствия прав на выполнение операций (`cat`, `cp`, `cd`);
- основные опции команд `mount`, `fsck`, `mkfs`, `kill`.

Практически подтверждено влияние прав доступа на возможность чтения файлов, копирования и входа в каталог.

---

## Ответы на контрольные вопросы

### 1. Что такое файловая система?

Файловая система — часть операционной системы, обеспечивающая организацию, хранение и доступ к файлам и каталогам на носителе информации.

Примеры:

- `ext4` — журналируемая, стандартная для Linux;
- `btrfs` — современная, поддерживает снапшоты и сжатие;
- `xfs` — высокопроизводительная, для серверов;
- `ntfs` — для Windows;
- `fat` — совместимая, но с ограничениями.

### 2. Какие команды для просмотра файлов вы знаете?

```bash
cat file
less file
head -n file
tail -n file
```

### 3. Как создать каталог?

```bash
mkdir dir
mkdir -p dir1/dir2/dir3
mkdir -m 755 dir
```

### 4. Как скопировать файл или каталог?

```bash
cp file1 file2
cp -r dir1 dir2
cp -i file1 file2
```

### 5. Как переместить или переименовать файл?

```bash
mv old new
mv file dir/
```

### 6. Как удалить файл и каталог?

```bash
rm file
rm -r dir
rmdir dir
```

### 7. Как изменить права доступа?

```bash
chmod 755 file
chmod u+x file
chmod g-r file
```

### 8. Что такое права доступа?

Права доступа определяют, какие действия может выполнять владелец, члены группы и остальные пользователи над файлом или каталогом.

Числовая запись:

- `r` = 4
- `w` = 2
- `x` = 1

### 9. Как посмотреть смонтированные файловые системы?

```bash
mount
cat /proc/mounts
df -h
cat /etc/fstab
```

### 10. Как определить объём каталога?

```bash
du -sh ~
du -a ~
```

### 11. Приведите основные возможности команды mv.

- Перемещение файлов и каталогов.
- Переименование.
- Опция `-i` — запрос подтверждения перезаписи.
- Опция `-r` — рекурсивное перемещение (не требуется, но иногда полезно).

### 12. Что такое права доступа? Как они могут быть изменены?

Права доступа определяют, кто и что может делать с файлом или каталогом. Изменяются командой `chmod`:

- символьная запись: `u`, `g`, `o`, `+`, `-`, `=`;
- числовая запись: восьмеричное число.

---

## Библиография

1. Курячий Г. В., Маслинский К. А. Операционная система Linux. — М.: Интуит.Ру, 2005. — 392 с.: ил.
2. Робачевский А. Операционная система UNIX / А. Робачевский, С. Немнюгин, О. Стесик. — 2-е изд. — СПб.: БХВ-Петербург, 2010. — 656 сс.
3. Немет Э. Unix и Linux: руководство системного администратора / Э. Немет, Г. Снайдер, Т. Р. Хейн, Б. Уэйли. — 4-е изд. — Вильямс, 2014. — 1312 сс.
4. Robbins A. Bash Pocket Reference / A. Robbins. — O’Reilly Media, 2016. — 156 сс.