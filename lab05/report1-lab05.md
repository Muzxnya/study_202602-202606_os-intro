# Лабораторная работа № 5

## Настройка рабочей среды

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

Приобретение практических навыков настройки рабочей среды пользователя в операционной системе Linux. Освоение менеджера паролей `pass` и системы управления файлами конфигурации `chezmoi`.

---

## Задание работы

1. Установить и настроить менеджер паролей `pass`.
2. Создать GPG-ключ и инициализировать хранилище паролей.
3. Настроить синхронизацию хранилища паролей с Git.
4. Установить и настроить систему управления файлами конфигурации `chezmoi`.
5. Создать собственный репозиторий `dotfiles` на основе шаблона.
6. Применить конфигурации на локальной машине.
7. Освоить основные команды `chezmoi`.

---

## Теоретические сведения

### Менеджер паролей pass

`pass` — стандартный менеджер паролей для Unix, разработанный в идеологии Unix. Основные свойства:

- данные хранятся в файловой системе в виде каталогов и файлов;
- файлы шифруются с помощью GPG-ключа;
- поддерживается синхронизация с Git;
- структура базы паролей может быть произвольной.

Пример семантической структуры базы:

```
example.com.pgp
example.com/user.pgp
user@example.com.pgp
example.com:22.pgp
```

### Управление файлами конфигурации chezmoi

`chezmoi` — утилита для управления файлами конфигурации домашнего каталога пользователя. Основные сведения:

- сайт: https://www.chezmoi.io/;
- репозиторий: https://github.com/twpayne/chezmoi;
- состояние файлов конфигурации сохраняется в каталоге `~/.local/share/chezmoi`, который является клоном репозитория `dotfiles`;
- конфигурационный файл `~/.config/chezmoi/chezmoi.toml` специфичен для локальной машины;
- команда `chezmoi apply` вычисляет желаемое содержимое файлов и приводит их в соответствие.

### Шаблоны chezmoi

Шаблоны используют синтаксис Go templates. Файл интерпретируется как шаблон, если:

- имя файла имеет суффикс `.tmpl`;
- файл находится в каталоге `.chezmoitemplates`.

Пример шаблона:

```
HOSTNAME={{- .chezmoi.hostname }}
```

Условные выражения:

```
{{ if eq .chezmoi.os "darwin" }}
darwin
{{ else if eq .chezmoi.os "linux" }}
linux
{{ else }}
other operating system
{{ end }}
```

---

## Соглашения об именовании

В соответствии с требованиями курса:

- имя пользователя внутри виртуальной машины: `zhaoxinya`;
- имя хоста виртуальной машины: `zhaoxinya`;
- имя виртуальной машины: `ZhaoXinya2`;
- имя репозитория для хранения паролей: `password-store`;
- имя репозитория для хранения конфигураций: `dotfiles`.

Проверка имени пользователя и хоста:

```bash
whoami
hostname
id
```

![屏幕截图 2026-10-06 234112.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-06%20234112.png)

---

## Ход выполнения работы

### Установка менеджера паролей pass

Установка:

```bash
sudo dnf -y install pass pass-otp
```

![屏幕截图 2026-10-07 150055.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150055.png)Проверка версии:

```bash
pass --version
```

![屏幕截图 2026-10-07 150129.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150129.png)

Результат:

```
= pass: the standard unix password manager =
v1.7.4
```

### Генерация GPG-ключа

Проверка существующих ключей:

```bash
gpg --list-secret-keys
```

![屏幕截图 2026-10-07 150206.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150206.png)

Ключ отсутствовал, поэтому был создан новый:

```bash
gpg --full-generate-key
```

![屏幕截图 2026-10-07 150340.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150340.png)

![屏幕截图 2026-10-07 150551.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150551.png)

![屏幕截图 2026-10-07 150607.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150607.png)

Параметры ключа:

- тип: ECC (sign and encrypt), кривая Curve 25519;
- срок действия: 0 (не истекает);
- Real name: `zhaoxinya`;
- Email address: `1132258332@rudn.ru`;
- Passphrase: задан пользователем.

После генерации:

```bash
gpg --list-secret-keys
```

![屏幕截图 2026-10-07 150651.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150651.png)

Результат:

```
sec   ed25519 2026-10-07 [SC]
      A9F386A1BEB16BED0D19EF500625F1CD9FFDB5CE
uid           [ultimate] zhaoxinya <1132258332@rudn.ru>
ssb   cv25519 2026-10-07 [E]
```

### Инициализация хранилища паролей

```bash
pass init 1132258332@rudn.ru
```

![屏幕截图 2026-10-07 150826.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150826.png)

Результат:

```
mkdir: created directory '/home/zhaoxinya/.password-store/'
Password store initialized for 1132258332@rudn.ru
```

### Добавление тестового пароля

```bash
pass insert test/example.com
```

Результат:

```
mkdir: created directory '/home/zhaoxinya/.password-store/test'
Enter password for test/example.com:
Retype password for test/example.com:
```

Проверка содержимого хранилища:

```bash
pass
```

Результат:

```
Password Store
└── test
    └── example.com
```

![屏幕截图 2026-10-07 150943.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20150943.png)

Извлечение пароля:

```bash
pass test/example.com
```

![屏幕截图 2026-10-07 151030.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20151030.png)

### Синхронизация хранилища с Git

Создание приватного репозитория на GitHub:

```bash
gh repo create password-store --private --description "Pass password store"
```

![屏幕截图 2026-10-07 151336.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20151336.png)

Результат:

```
✓ Created repository Muzxnya/password-store on github.com
https://github.com/Muzxnya/password-store
```

Инициализация git и первый push:

```bash
pass git init
pass git remote add origin git@github.com:Muzxnya/password-store.git
pass git push -u origin main
```

![屏幕截图 2026-10-07 151359.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20151359.png)

![屏幕截图 2026-10-07 151739.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20151739.png)

Результат:

```
To github.com:Muzxnya/password-store.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

### Установка chezmoi

```bash
sh -c "$(wget -qO- chezmoi.io/get)"
```

![屏幕截图 2026-10-07 152347.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20152347.png)Результат:

```
info found chezmoi version 2.73.0 for latest/linux/amd64
info found glibc version 2.41
info installed bin/chezmoi
```

Проверка:

```bash
chezmoi --version
```

![屏幕截图 2026-10-07 152452.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20152452.png)

Результат:

```
chezmoi version v2.73.0, commit 24b71e4cf9d98cce0801cfc68e7553355efeaff7, built at 2026-09-28T19:47:35Z, built by goreleaser
```

### Установка дополнительного программного обеспечения

```bash
sudo dnf -y install \
    dunst fontawesome-fonts powerline-fonts \
    light fuzzel swaylock kitty \
    waybar swaybg wl-clipboard mpv grim slurp
```

![屏幕截图 2026-10-07 153031.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20153031.png)

### Создание репозитория dotfiles

```bash
gh repo create dotfiles --template="yamadharma/dotfiles-template" --private
```

![屏幕截图 2026-10-07 153707.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20153707.png)

Результат:

```
✓ Created repository Muzxnya/dotfiles on github.com
https://github.com/Muzxnya/dotfiles
```

### Инициализация chezmoi

```bash
chezmoi init git@github.com:Muzxnya/dotfiles.git
```

![屏幕截图 2026-10-07 154105.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20154105.png)

В процессе инициализации chezmoi запросил данные пользователя (адрес электронной почты и имя) для генерации локального файла конфигурации.

Проверка различий:

```bash
chezmoi diff
```

![屏幕截图 2026-10-07 154235.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20154235.png)

Проверка статуса:

```bash
chezmoi status
```

Применение конфигураций:

```bash
chezmoi apply -v
```

![屏幕截图 2026-10-07 154803.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20154803.png)

После успешного применения повторный запуск `chezmoi status` не выводит ничего, что означает полное соответствие состояния домашнего каталога репозиторию `dotfiles`.

Проверка управляемых файлов:

```bash
chezmoi managed
```

![屏幕截图 2026-10-07 155035.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20155035.png)

![屏幕截图 2026-10-07 155051.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20155051.png)

Просмотр переменных шаблона:

```bash
chezmoi data
```

![屏幕截图 2026-10-07 155135.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20155135.png)

![屏幕截图 2026-10-07 155150.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20155150.png)

Тестирование шаблона:

```bash
chezmoi execute-template '{{ .chezmoi.hostname }}'
```

![屏幕截图 2026-10-07 155541.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20155541.png)

Результат:

```
zhaoxinya
```

### Ежедневные операции с chezmoi

Получить и применить последние изменения:

```bash
chezmoi update
```

Просмотреть изменения без применения:

```bash
chezmoi git pull -- --autostash --rebase && chezmoi diff
```

Применить изменения:

```bash
chezmoi apply
```

Установить dotfiles на новой машине одной командой:

```bash
chezmoi init --apply git@github.com:Muzxnya/dotfiles.git
```

### Автоматические commit и push

В файл `~/.config/chezmoi/chezmoi.toml` добавлены параметры:

```toml
[git]
    autoCommit = true
    autoPush = true
```

При этом следует помнить, что при использовании `autoPush` и публичном репозитории `dotfiles` случайно добавленный секрет будет автоматически опубликован.

---

## Выводы

В ходе лабораторной работы были приобретены практические навыки настройки рабочей среды пользователя в Linux:

- установлен и настроен менеджер паролей `pass`;
- создан GPG-ключ, применённый для шифрования хранилища;
- инициализировано хранилище паролей, добавлен тестовый пароль;
- настроена синхронизация хранилища паролей с приватным репозиторием на GitHub;
- установлена и настроена система управления файлами конфигурации `chezmoi`;
- создан собственный репозиторий `dotfiles` на основе шаблона;
- освоены основные команды `chezmoi`: `init`, `diff`, `apply`, `status`, `managed`, `data`, `execute-template`, `update`.

Все выполненные действия соответствуют требованиям задания лабораторной работы.

---

## Ответы на контрольные вопросы

### 1. Что такое менеджер паролей pass?

`pass` — стандартный менеджер паролей для Unix. Данные хранятся в файловой системе в виде дерева каталогов и файлов, каждый файл шифруется GPG-ключом. Поддерживается синхронизация с Git.

### 2. Где хранятся данные менеджера паролей pass?

В каталоге `~/.password-store/`. Каждый пароль хранится в отдельном `.gpg`-файле, структура каталогов может быть произвольной.

### 3. Как инициализировать хранилище pass?

```bash
pass init <gpg-id or email>
```

### 4. Как добавить новый пароль в pass?

```bash
pass insert [OPTIONAL DIR]/[FILENAME]
```

Например:

```bash
pass insert github/username
```

### 5. Как синхронизировать хранилище pass с Git?

```bash
pass git init
pass git remote add origin git@github.com:<username>/<repo>.git
pass git push -u origin main
```

Дальнейшая синхронизация:

```bash
pass git pull
pass git push
```

### 6. Что такое chezmoi?

`chezmoi` — утилита для управления файлами конфигурации домашнего каталога пользователя. Позволяет хранить dotfiles в репозитории Git и применять их на разных машинах.

### 7. Где chezmoi хранит исходные файлы?

В каталоге `~/.local/share/chezmoi`, который является клоном репозитория `dotfiles`.

### 8. Что делает команда `chezmoi apply`?

Вычисляет желаемое содержимое и права доступа для каждого файла и вносит изменения, чтобы домашний каталог соответствовал состоянию репозитория `dotfiles`.

### 9. Что такое шаблоны chezmoi?

Шаблоны используют синтаксис Go templates и позволяют изменять содержимое файла в зависимости от среды (имя хоста, ОС, архитектура и т. п.). Файл считается шаблоном, если его имя заканчивается на `.tmpl` или он находится в каталоге `.chezmoitemplates`.

### 10. Как установить dotfiles на новой машине одной командой?

```bash
chezmoi init --apply git@github.com:<username>/dotfiles.git
```

### 11. Что такое autoCommit и autoPush в chezmoi?

Это параметры конфигурации, включающие автоматическое создание commit и push изменений в репозиторий `dotfiles` при каждом изменении исходного каталога.

### 12. Почему опасно использовать autoPush с публичным репозиторием?

Потому что любой случайно добавленный секрет (пароль, токен, ключ) будет автоматически опубликован в публичном репозитории.

---

## Библиография

1. Password Store — the standard unix password manager [Электронный ресурс]. — URL: https://www.passwordstore.org/
2. GnuPG — The GNU Privacy Guard [Электронный ресурс]. — URL: https://gnupg.org/
3. chezmoi — manage your dotfiles across multiple diverse machines [Электронный ресурс]. — URL: https://www.chezmoi.io/
4. chezmoi на GitHub [Электронный ресурс]. — URL: https://github.com/twpayne/chezmoi
5. Git [Электронный ресурс]. — URL: https://git-scm.com/
6. GitHub CLI Documentation [Электронный ресурс]. — URL: https://cli.github.com/manual/
7. Немет, Э. Unix и Linux: руководство системного администратора / Э. Немет, Г. Снайдер, Т. Р. Хейн, Б. Уэйли. — 4-е изд. — Вильямс, 2014. — 1312 сс.