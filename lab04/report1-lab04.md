# Лабораторная работа № 4

## Продвинутое использование git

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

Получение навыков правильной работы с репозиториями git. Освоение методологии git-flow, семантического версионирования и общепринятых коммитов (Conventional Commits).

---

## Задание работы

1. Установить необходимое программное обеспечение (git-flow, Node.js, pnpm, commitizen, conventional-changelog).
2. Создать новый репозиторий `git-extended` на GitHub.
3. Настроить и применить спецификацию Conventional Commits.
4. Освоить рабочий процесс Gitflow: создание функциональных веток, релизных веток, веток исправлений.
5. Применить семантическое версионирование при создании релизов.
6. Создать релизы 1.0.0 и 1.2.3 с автоматической генерацией журнала изменений (CHANGELOG).

---

## Теоретические сведения

### Рабочий процесс Gitflow

Gitflow Workflow — модель ветвления, предложенная Винсентом Дриссеном. Основные ветки:

- `main` — хранит официальную историю релизов;
- `develop` — ветка для объединения всех новых функций.

Дополнительные ветки:

- `feature/*` — для разработки отдельных функций (создаются от `develop`);
- `release/*` — для подготовки релиза (создаются от `develop`, сливаются в `main` и `develop`);
- `hotfix/*` — для срочных исправлений (создаются от `main`, сливаются в `main` и `develop`).

### Семантическое версионирование

Версия задаётся в виде `МАЖОРНАЯ.МИНОРНАЯ.ПАТЧ`:

- **МАЖОРНАЯ** — несовместимые изменения API;
- **МИНОРНАЯ** — новая функциональность без нарушения обратной совместимости;
- **ПАТЧ** — обратно совместимые исправления.

### Общепринятые коммиты

Структура коммита:

```
<тип>(<область>): <описание изменения>
<пустая строка>
[необязательное тело]
<пустая строка>
[необязательный нижний колонтитул]
```

Основные типы:

- `feat` — новая функция (MINOR);
- `fix` — исправление ошибки (PATCH);
- `docs` — изменения в документации;
- `chore` — служебные изменения;
- `refactor`, `style`, `test`, `perf`, `build`, `ci`.

---

## Соглашения об именовании

В соответствии с требованиями курса:

- имя пользователя внутри виртуальной машины: `zhaoxinya`;
- имя хоста виртуальной машины: `zhaoxinya`;
- имя виртуальной машины: `ZhaoXinya2`;
- имя репозитория для эксперимента: `git-extended`;
- имя основного курсового репозитория: `study_202602-202606_os-intro`.

Проверка имени пользователя и хоста:

```bash
whoami
hostname
id
```

![屏幕截图 2026-10-06 234112.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-06%20234112.png)

---

## Ход выполнения работы

### Установка программного обеспечения

Установка git-flow через COPR:

```bash
sudo dnf -y copr enable elegos/gitflow
sudo dnf -y install gitflow
```

![屏幕截图 2026-10-07 001641.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20001641.png)

Установка Node.js и pnpm:

```bash
sudo dnf -y install nodejs npm
sudo dnf -y install pnpm
```

![屏幕截图 2026-10-07 104743.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20104743.png)

![屏幕截图 2026-10-07 104834.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20104834.png)

![屏幕截图 2026-10-07 104853.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20104853.png)

Настройка pnpm:

```bash
pnpm setup
source ~/.bashrc
```

![屏幕截图 2026-10-07 104932.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20104932.png)

![屏幕截图 2026-10-07 104942.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20104942.png)

![屏幕截图 2026-10-07 105022.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20105022.png)

Установка commitizen и conventional-changelog:

```bash
pnpm add -g commitizen
pnpm add -g conventional-changelog-cli
```

![屏幕截图 2026-10-07 105119.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20105119.png)

![屏幕截图 2026-10-07 105500.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20105500.png)

![屏幕截图 2026-10-07 105508.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20105508.png)

Проверка версий:

```bash
git --version
git flow version
gh --version
node --version
npm --version
pnpm --version
git-cz --version
conventional-changelog --version
```

![屏幕截图 2026-10-07 004650.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20004650.png)

---

### Создание репозитория git-extended

Создание рабочего каталога и инициализация репозитория:

```bash
mkdir -p ~/work/git-extended
cd ~/work/git-extended
git init
```

![屏幕截图 2026-10-07 110845.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20110845.png)

![屏幕截图 2026-10-07 111203.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20111203.png)

Создание репозитория на GitHub с помощью утилиты `gh`:

```bash
gh repo create git-extended --public \
    --description "Git repo for educational purposes" \
    --source=. --remote=origin
```

![屏幕截图 2026-10-07 111518.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20111518.png)

Создание файла `package.json` с конфигурацией commitizen:

```bash
pnpm init
```

После редактирования `package.json` принял вид:

```json
{
  "name": "git-extended",
  "version": "1.0.0",
  "description": "Git repo for educational purposes",
  "main": "index.js",
  "author": "zhaoxinya <1132258332@rudn.ru>",
  "license": "CC-BY-4.0",
  "config": {
    "commitizen": {
      "path": "cz-conventional-changelog"
    }
  }
}
```

![屏幕截图 2026-10-07 111814.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20111814.png)

---

### Первый conventional commit

Добавление файлов в индекс и создание коммита с помощью `git cz`:

```bash
git add .
git cz
```

![屏幕截图 2026-10-07 115337.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20115337.png)

![屏幕截图 2026-10-07 115517.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20115517.png)

В интерактивном режиме были выбраны параметры:

- type: `feat`
- scope: (пропущено)
- short description: `initial commit`
- остальные поля: пропущены

Коммит успешно создан:

```
[main (root-commit) 928c072] feat: initial commit
 2 files changed, 171 insertions(+)
 create mode 100644 package.json
 create mode 100644 pnpm-lock.yaml
```

Отправка изменений на GitHub:

```bash
git branch -M main
git push -u origin main
```

![屏幕截图 2026-10-07 124000.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20124000.png)



![屏幕截图 2026-10-07 142730.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20142730.png)

---

### Инициализация git-flow

```bash
git flow init
```

При инициализации:

- main branch: `main`
- develop branch: `develop`
- feature prefix: `feature/`
- release prefix: `release/`
- hotfix prefix: `hotfix/`
- version tag prefix: `v`

Проверка текущей ветки:

```bash
git branch
```

Результат:

```
* develop
  main
```

![屏幕截图 2026-10-07 115929.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20115929.png)Публикация ветки develop:

```bash
git push -u origin develop
git push --all
```

![屏幕截图 2026-10-07 142904.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20142904.png)

![屏幕截图 2026-10-07 143014.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20143014.png)

![屏幕截图 2026-10-07 123047.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20123047.png)---

### Релиз 1.0.0

Создание релизной ветки:

```bash
git flow release start 1.0.0
```

![屏幕截图 2026-10-07 120741.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20120741.png)

Генерация журнала изменений:

```bash
conventional-changelog -p angular -r 0 > CHANGELOG.md
cat CHANGELOG.md
```

![屏幕截图 2026-10-07 121200.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20121200.png)

Содержимое `CHANGELOG.md`:

```
# 1.0.0 (2026-10-07)

### Features

* initial commit ([928c072](...))
```

Добавление журнала в индекс:

```bash
git add CHANGELOG.md
git commit -m "chore(site): add changelog"
```

![屏幕截图 2026-10-07 122754.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20122754.png)

Завершение релиза:

```bash
git flow release finish 1.0.0
```

![屏幕截图 2026-10-07 122940.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20122940.png)

Публикация:

```bash
git push --all
git push --tags
```

![屏幕截图 2026-10-07 143014.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20143014.png)

![屏幕截图 2026-10-07 123127.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20123127.png)

Создание релиза на GitHub:

```bash
gh release create v1.0.0 -F CHANGELOG.md -t "Release 1.0.0"
```

![屏幕截图 2026-10-07 131529.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20131529.png)

---

### Разработка новой функциональности

Создание функциональной ветки:

```bash
git flow feature start feature_branch
```

Добавление файла и коммит:

```bash
echo "New feature content" > feature.txt
git add feature.txt
git cz
```

![屏幕截图 2026-10-07 132603.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20132603.png)

![屏幕截图 2026-10-07 132613.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20132613.png)

В интерактивном режиме были выбраны параметры:

- type: `feat`
- short description: `add new feature.txt`

Коммит создан:

```
[develop 7da1cc4] feat: add new feature.txt
 1 file changed, 1 insertion(+)
 create mode 100644 feature.txt
```

Завершение функциональной ветки:

```bash
git flow feature finish feature_branch
```

![屏幕截图 2026-10-07 133432.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20133432.png)

---

### Релиз 1.2.3

Создание релизной ветки:

```bash
git flow release start 1.2.3
```

![屏幕截图 2026-10-07 132735.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20132735.png)

![屏幕截图 2026-10-07 132915.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20132915.png)

Обновление номера версии в `package.json`:

```json
"version": "1.2.3",
```

Генерация обновлённого журнала изменений:

```bash
conventional-changelog -p angular -r 0 > CHANGELOG.md
cat CHANGELOG.md
```

![屏幕截图 2026-10-07 133135.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20133135.png)

Содержимое `CHANGELOG.md`:

```
# 1.2.3 (2026-10-07)

### Features

* add new feature.txt ([7da1cc4](...))
* initial commit ([928c072](...))
```

![屏幕截图 2026-10-07 133824.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20133824.png)

Добавление и коммит:

```bash
git add CHANGELOG.md package.json
git commit -m "chore(site): update changelog and version"
```

Завершение релиза:

```bash
git flow release finish 1.2.3
```

![屏幕截图 2026-10-07 133432.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20133432.png)При этом возник конфликт слияния в файле `CHANGELOG.md`, который был разрешён вручную: конфликтующие маркеры (`<<<<<<<`, `=======`, `>>>>>>>`) удалены, оставлены обе строки Features.

После разрешения конфликта:

```bash
git add CHANGELOG.md
git commit -m "Merge branch 'release/1.2.3'"
```

![屏幕截图 2026-10-07 134046.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134046.png)

Создание тега версии:

```bash
git tag -a v1.2.3 -m "Release 1.2.3"
```

![屏幕截图 2026-10-07 134236.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134236.png)

Публикация изменений и тегов:

```bash
git push --all
git push --tags
```

![屏幕截图 2026-10-07 134248.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134248.png)

![屏幕截图 2026-10-07 134258.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134258.png)

Создание релиза на GitHub:

```bash
gh release create v1.2.3 -F CHANGELOG.md -t "Release 1.2.3"
```

![屏幕截图 2026-10-07 134414.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134414.png)

Проверка списка релизов:

```bash
gh release list
```

![屏幕截图 2026-10-07 134627.png](C:\Users\dell\Pictures\Screenshots\屏幕截图%202026-10-07%20134627.png)



Результат:

```
TITLE           TYPE      TAG NAME   PUBLISHED
Release 1.2.3   Latest    v1.2.3     ...
Release 1.0.0             v1.0.0     ...
```

---

## Выводы

В ходе лабораторной работы были получены практические навыки правильной работы с репозиториями git:

- освоена методология **Gitflow**: работа с ветками `main`, `develop`, `feature/*`, `release/*`;
- изучена спецификация **Conventional Commits**, применённая при создании коммитов через `git cz`;
- освоено **семантическое версионирование**: созданы релизы `1.0.0` и `1.2.3`;
- применена автоматическая генерация журнала изменений с помощью `conventional-changelog`;
- освоена работа с утилитой командной строки `gh` для создания репозитория и релизов на GitHub;
- получены навыки разрешения конфликтов слияния в Git.

Все релизы успешно опубликованы в репозитории [git-extended](https://github.com/Muzxnya/git-extended).

---

## Ответы на контрольные вопросы

### 1. Что такое Gitflow?

Gitflow — модель ветвления Git, предложенная Винсентом Дриссеном. Она предполагает использование двух долгоживущих веток (`main` и `develop`) и временных веток (`feature`, `release`, `hotfix`). Такая модель хорошо подходит для организации рабочего процесса на основе релизов.

### 2. Какие основные ветки используются в Gitflow?

- `main` — официальная история релизов;
- `develop` — ветка для интеграции всех новых функций;
- `feature/*` — для разработки отдельных функций;
- `release/*` — для подготовки релизов;
- `hotfix/*` — для срочных исправлений.

### 3. Что такое семантическое версионирование?

Семантическое версионирование (SemVer) — система нумерации версий вида `МАЖОРНАЯ.МИНОРНАЯ.ПАТЧ`:

- МАЖОРНАЯ увеличивается при несовместимых изменениях API;
- МИНОРНАЯ — при добавлении новой функциональности без нарушения совместимости;
- ПАТЧ — при обратно совместимых исправлениях.

### 4. Что такое Conventional Commits?

Conventional Commits — спецификация, регламентирующая структуру сообщений коммитов. Формат:

```
<тип>(<область>): <описание>
```

Основные типы: `feat`, `fix`, `docs`, `chore`, `refactor`, `style`, `test`, `perf`, `build`, `ci`.

### 5. Какая связь между Conventional Commits и SemVer?

Коммиты `fix` соответствуют PATCH-версии, `feat` — MINOR-версии. Коммиты с `BREAKING CHANGE` соответствуют MAJOR-версии. Это позволяет автоматически генерировать номера версий и журнал изменений.

### 6. Что делает команда `git flow init`?

Инициализирует структуру Gitflow в существующем репозитории: создаёт ветку `develop`, настраивает префиксы для временных веток и префикс тегов версий.

### 7. Что делает команда `git flow release start 1.0.0`?

Создаёт новую релизную ветку `release/1.0.0` на основе `develop`. В этой ветке ведётся подготовка релиза: обновление версии, генерация журнала изменений, последние исправления.

### 8. Что делает команда `git flow release finish 1.0.0`?

Завершает релиз: сливает `release/1.0.0` в `main` и `develop`, создаёт тег `v1.0.0`, удаляет релизную ветку.

### 9. Что такое CHANGELOG?

CHANGELOG — файл, содержащий список значимых изменений проекта по версиям. Обычно генерируется автоматически на основе сообщений коммитов (например, с помощью `conventional-changelog`).

### 10. Как создать релиз на GitHub?

```bash
gh release create v1.0.0 -F CHANGELOG.md -t "Release 1.0.0"
```

---

## Библиография

1. Chacon, S. Pro Git / S. Chacon, B. Straub. — Apress, 2014. — 456 сс.
2. Driessen, V. A successful Git branching model [Электронный ресурс]. — URL: https://nvie.com/posts/a-successful-git-branching-model/
3. Conventional Commits [Электронный ресурс]. — URL: https://www.conventionalcommits.org/
4. Semantic Versioning [Электронный ресурс]. — URL: https://semver.org/
5. Node.js Documentation [Электронный ресурс]. — URL: https://nodejs.org/docs/
6. GitHub CLI Documentation [Электронный ресурс]. — URL: https://cli.github.com/manual/