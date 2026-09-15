# Lab 3
# Отчёт по лабораторной работе: CI/CD для статического сайта в SourceCraft

## 1. Цель работы

Реализовать автоматическое развёртывание статического сайта на mkdocs двумя способами: через платформу SourceCraft (SourceCraft Sites) и через GitHub Actions на GitHub Pages, используя один локальный репозиторий с двумя удалёнными репозиториями.

## 2. Что было выполнено

1. Создана публичная организация в SourceCraft (`Labs`, идентификатор `ivanratnikov-labs`).
2. Создан пустой репозиторий `labs` внутри организации.
3. Создан персональный токен доступа (PAT) с ролью «Ответственный за репозиторий» (Maintainer) для работы по HTTPS.
4. К локальному git-репозиторию добавлен второй remote:

        git remote add sourcecraft https://<username>:<token>@git.sourcecraft.dev/ivanratnikov-labs/labs.git

5. Настроен файл `.sourcecraft/sites.yaml`, указывающий SourceCraft Sites, откуда брать собранный сайт.
6. Сайт mkdocs пересобран из исходников (`source/mkdocs.yml`) в папку `docs/` командой:

        mkdocs build --site-dir ../docs

7. Изменения запушены в оба remote:

        git push origin main
        git push sourcecraft main

8. Создан workflow GitHub Actions (`.github/workflows/deploy.yml`) для автоматической сборки и деплоя на GitHub Pages при каждом пуше в `main`.
9. В настройках GitHub-репозитория включён GitHub Pages с источником `gh-pages`.

## 3. Итоговая структура репозитория

    r991-code.github.io/
    ├── source/
    │   ├── mkdocs.yml        (конфигурация mkdocs, исходники)
    │   └── docs/             (markdown-страницы)
    ├── docs/                 (собранный сайт, используется SourceCraft Sites)
    │   ├── index.html
    │   ├── assets/
    │   └── ...
    ├── .sourcecraft/
    │   └── sites.yaml        (конфигурация SourceCraft Sites)
    └── .github/
        └── workflows/
            └── deploy.yml    (workflow GitHub Actions)

## 4. Конфигурация SourceCraft Sites

Файл `.sourcecraft/sites.yaml`:

    site:
      root: "docs"
      ref: "main"

SourceCraft автоматически переопубликовывает сайт из указанной папки основной ветки в течение нескольких минут после каждого пуша — отдельного CI-пайплайна для этого не требуется.

## 5. Конфигурация GitHub Actions

Файл `.github/workflows/deploy.yml`:

    name: Deploy mkdocs site

    on:
      push:
        branches: [ main ]

    permissions:
      contents: write

    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4

          - uses: actions/setup-python@v5
            with:
              python-version: '3.x'

          - run: pip install mkdocs mkdocs-material

          - name: Build and deploy
            run: |
              cd source
              mkdocs gh-deploy --force --remote-branch gh-pages

Workflow запускается автоматически при каждом пуше в `main`, собирает документацию из `source/` и публикует в ветку `gh-pages`.

## 6. Что неоходимо сделать, чтобы выполнить деплой

1. Установить mkdocs: `pip install mkdocs mkdocs-material`.
2. Создать проект: `mkdocs new .` (или использовать существующие исходники в `source/`).
3. Создать пустые репозитории на GitHub и в организации SourceCraft.
4. Добавить оба remote'а (`origin`, `sourcecraft`).
5. Создать `.sourcecraft/sites.yaml` и `.github/workflows/deploy.yml`, как указано выше.
6. Запушить в оба remote'а — деплой запустится автоматически.

## 7. Настройки, необходимые в интерфейсах платформ

**SourceCraft:**

- Организация должна быть публичной (обязательное условие для SourceCraft Sites).
- Токен доступа — с ролью Maintainer / Ответственный за репозиторий, иначе push отклоняется.

**GitHub:**

- Settings → Pages → Source: Deploy from a branch → gh-pages / root.
- Для workflow нужны права `contents: write` (указаны в самом `deploy.yml`), иначе `mkdocs gh-deploy` не сможет запушить в `gh-pages`.

## 8. Итоговые ссылки

- Сайт на SourceCraft: https://ivanratnikov-labs.sourcecraft.site/labs/
- Репозиторий SourceCraft: https://git.sourcecraft.dev/ivanratnikov-labs/labs
- Сайт на GitHub Pages: https://r991-code.github.io/
- Репозиторий GitHub: https://github.com/r991-code/r991-code.github.io