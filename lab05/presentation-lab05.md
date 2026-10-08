---
title: "Настройка рабочей среды"
subtitle: "Лабораторная работа № 5"
author: "Чжао Синья (zhaoxinya)"
date: "2026-10-07"
theme: default
---

# Настройка рабочей среды

## Лабораторная работа № 5

**Студент:** Чжао Синья  
**Учётная запись:** zhaoxinya  
**Номер студенческого билета:** 113225832  
**Дисциплина:** Архитектура компьютеров и операционные системы. Операционные системы

---

# Цель работы

Приобретение практических навыков настройки рабочей среды пользователя в ОС Linux.

- Освоение менеджера паролей **pass**
- Освоение системы управления файлами конфигурации **chezmoi**

---

# Задание работы

1. Установить и настроить менеджер паролей `pass`
2. Создать GPG-ключ и инициализировать хранилище паролей
3. Настроить синхронизацию хранилища паролей с Git
4. Установить и настроить `chezmoi`
5. Создать собственный репозиторий `dotfiles`
6. Применить конфигурации на локальной машине
7. Освоить основные команды `chezmoi`

---

# Менеджер паролей pass

`pass` — стандартный менеджер паролей для Unix.

### Основные свойства

- данные хранятся в файловой системе в виде каталогов и файлов
- файлы шифруются с помощью GPG-ключа
- поддерживается синхронизация с Git
- структура базы паролей может быть произвольной

### Пример семантической структуры

```
example.com.pgp
example.com/user.pgp
user@example.com.pgp
example.com:22.pgp
```

---

# chezmoi: общие сведения

`chezmoi` — утилита для управления файлами конфигурации домашнего каталога.

- сайт: https://www.chezmoi.io/
- репозиторий: https://github.com/twpayne/chezmoi
- исходные файлы: `~/.local/share/chezmoi`
- конфиг: `~/.config/chezmoi/chezmoi.toml`
- команда `chezmoi apply` приводит файлы в соответствие

---

# Шаблоны chezmoi

Шаблоны используют синтаксис **Go templates**.

Файл считается шаблоном, если:

- имя файла имеет суффикс `.tmpl`
- файл находится в каталоге `.chezmoitemplates`

Пример:

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

# Соглашения об именовании

- Пользователь ВМ: `zhaoxinya`
- Имя хоста: `zhaoxinya`
- Имя виртуальной машины: `ZhaoXinya2`
- Репозиторий паролей: `password-store`
- Репозиторий конфигураций: `dotfiles`

Проверка:

```bash
whoami
hostname
id
```

---

# Установка pass

```bash
sudo dnf -y install pass pass-otp
```

Проверка:

```bash
pass --version
```

Результат:

```
= pass: the standard unix password manager =
v1.7.4
```

---

# Генерация GPG-ключа

```bash
gpg --full-generate-key
```

Параметры ключа:

- тип: ECC (sign and encrypt), Curve 25519
- срок действия: 0 (не истекает)
- Real name: `zhaoxinya`
- Email: `1132258332@rudn.ru`

После генерации:

```bash
gpg --list-secret-keys
```

Результат:

```
sec   ed25519 2026-10-07 [SC]
      A9F386A1BEB16BED0D19EF500625F1CD9FFDB5CE
uid           [ultimate] zhaoxinya <1132258332@rudn.ru>
ssb   cv25519 2026-10-07 [E]
```

---

# Инициализация хранилища pass

```bash
pass init 1132258332@rudn.ru
```

Результат:

```
mkdir: created directory '/home/zhaoxinya/.password-store/'
Password store initialized for 1132258332@rudn.ru
```

---

# Добавление тестового пароля

```bash
pass insert test/example.com
```

Проверка:

```bash
pass
```

Результат:

```
Password Store
└── test
    └── example.com
```

Извлечение:

```bash
pass test/example.com
```

---

# Синхронизация pass с Git

Создание приватного репозитория:

```bash
gh repo create password-store --private --description "Pass password store"
```

Результат:

```
✓ Created repository Muzxnya/password-store
```

Инициализация git и push:

```bash
pass git init
pass git remote add origin git@github.com:Muzxnya/password-store.git
pass git push -u origin main
```

Результат:

```
To github.com:Muzxnya/password-store.git
 * [new branch]      main -> main
```

---

# Установка chezmoi

```bash
sh -c "$(wget -qO- chezmoi.io/get)"
```

Результат:

```
info found chezmoi version 2.73.0
info installed bin/chezmoi
```

Проверка:

```bash
chezmoi --version
```

Результат:

```
chezmoi version v2.73.0
```

---

# Установка дополнительного ПО

```bash
sudo dnf -y install \
    dunst fontawesome-fonts powerline-fonts \
    light fuzzel swaylock kitty \
    waybar swaybg wl-clipboard mpv grim slurp
```

---

# Создание репозитория dotfiles

```bash
gh repo create dotfiles --template="yamadharma/dotfiles-template" --private
```

Результат:

```
✓ Created repository Muzxnya/dotfiles
https://github.com/Muzxnya/dotfiles
```

---

# Инициализация chezmoi

```bash
chezmoi init git@github.com:Muzxnya/dotfiles.git
```

Результат: репозиторий клонирован в `~/.local/share/chezmoi`.

chezmoi запросил email и имя для генерации `~/.config/chezmoi/chezmoi.toml`.

Проверка различий:

```bash
chezmoi diff
```

Проверка статуса:

```bash
chezmoi status
```

Применение:

```bash
chezmoi apply -v
```

Повторный `chezmoi status` — пусто, значит всё применено.

---

# Управление файлами

```bash
chezmoi managed
```

Пример вывода:

```
.XCompose
.bash_logout
.bash_profile
.bashrc
.bashrc.d
.config/dunst
.config/fontconfig
...
```

Просмотр переменных:

```bash
chezmoi data
```

Тестирование шаблона:

```bash
chezmoi execute-template '{{ .chezmoi.hostname }}'
```

Результат:

```
zhaoxinya
```

---

# Ежедневные операции с chezmoi

Получить и применить изменения:

```bash
chezmoi update
```

Посмотреть изменения без применения:

```bash
chezmoi git pull -- --autostash --rebase && chezmoi diff
```

Применить изменения:

```bash
chezmoi apply
```

Развернуть dotfiles на новой машине одной командой:

```bash
chezmoi init --apply git@github.com:Muzxnya/dotfiles.git
```

---

# Автоматические commit и push

Файл `~/.config/chezmoi/chezmoi.toml`:

```toml
[git]
    autoCommit = true
    autoPush = true
```

⚠️ При публичном репозитории `dotfiles` случайно добавленный секрет будет автоматически опубликован.

---

# Выводы

- Установлен и настроен менеджер паролей `pass`
- Создан GPG-ключ для шифрования хранилища
- Инициализировано хранилище паролей
- Настроена синхронизация с GitHub
- Установлена и настроена система `chezmoi`
- Создан собственный репозиторий `dotfiles`
- Освоены основные команды `chezmoi`:
  `init`, `diff`, `apply`, `status`, `managed`, `data`, `execute-template`, `update`

---

# Ответы на контрольные вопросы (1/3)

**1. Что такое менеджер паролей pass?**

Стандартный менеджер паролей для Unix. Данные хранятся в виде дерева каталогов и файлов, каждый файл шифруется GPG-ключом. Поддерживается синхронизация с Git.

**2. Где хранятся данные pass?**

В каталоге `~/.password-store/`.

**3. Как инициализировать хранилище pass?**

```bash
pass init <gpg-id or email>
```

**4. Как добавить новый пароль?**

```bash
pass insert [OPTIONAL DIR]/[FILENAME]
```

---

# Ответы на контрольные вопросы (2/3)

**5. Как синхронизировать хранилище pass с Git?**

```bash
pass git init
pass git remote add origin git@github.com:<username>/<repo>.git
pass git push -u origin main
```

**6. Что такое chezmoi?**

Утилита для управления файлами конфигурации домашнего каталога. Позволяет хранить dotfiles в Git и применять их на разных машинах.

**7. Где chezmoi хранит исходные файлы?**

В каталоге `~/.local/share/chezmoi`.

---

# Ответы на контрольные вопросы (3/3)

**8. Что делает `chezmoi apply`?**

Вычисляет желаемое содержимое и права доступа и приводит домашний каталог в соответствие репозиторию `dotfiles`.

**9. Что такое шаблоны chezmoi?**

Go templates для изменения содержимого в зависимости от среды. Файл считается шаблоном, если заканчивается на `.tmpl` или находится в `.chezmoitemplates`.

**10. Как установить dotfiles на новой машине одной командой?**

```bash
chezmoi init --apply git@github.com:<username>/dotfiles.git
```

**11. Что такое autoCommit и autoPush?**

Параметры конфигурации, автоматически создающие commit и push при изменениях.

**12. Почему опасно использовать autoPush с публичным репозиторием?**

Случайно добавленный секрет будет автоматически опубликован.

---

# Спасибо за внимание!

**Репозитории:**

- https://github.com/Muzxnya/password-store
- https://github.com/Muzxnya/dotfiles