# Лабораторная работа 3
**Тема:** CI/CD для статического сайта в SourceCraft

## Цель работы
Освоить настройку CI/CD для автоматической сборки статического сайта и его публикации в отдельную ветку release.

## Задание
Настроить CI/CD-пайплайн для сайта-портфолио с использованием платформы SourceCraft:

---
## Ход выполнения
### 1. Структура проекта
```
SITE/
├── .sourcecraft/
│   ├── ci.yaml             # sourceCraft workflow
│   └── sites.yaml         # конфигурация публикации
├── source/
│   ├── mkdocs.yml         # конфигурация MkDocs
│   └── docs/              # исходные файлы документации
├── requirements.txt       # зависимости Python
├── .gitignore
├── LICENSE
└── README.md
```

### 2. Реализация SourceCraft Workflow
**Файл `.sourcecraft/ci.yaml`:**
 
```yaml
on:
  push:
    workflows: build-site
    filter:
      branches: main

workflows:
  build-site:
    tasks:
      - name: build-and-publish-site
        cubes:
          - name: build-mkdocs-site
            image: docker.io/library/python:3.13-slim
            script:
              - python -m pip install --upgrade pip
              - "if [ -f requirements.txt ]; then pip install -r requirements.txt; fi"
              - cd source
              - mkdocs build -d ../docs
              - echo "Сайт собран в папке /docs"
              - ls -la ../docs
              
          - name: publish-to-release-branch
            script:
              - git checkout -b release
              - git add .
              - "git commit -m \"feat: автоматическое обновление сайта\""
              - "git push origin release -f"
```

### 3. Конфигурация публикации
 
**Файл `.sourcecraft/sites.yaml`:**
 
```yaml
site:
  root: docs
  ref: release
```

### 4. Результат

```python
origin  https://git@git.sourcecraft.dev/hellbane/site.git (fetch)
origin  https://git@git.sourcecraft.dev/hellbane/site.git (push)
sourcecraft     https://healbane:<token>@git.sourcecraft.dev/healbane/site.git (fetch)
sourcecraft     https://healbane:<token>@git.sourcecraft.dev/healbane/site.git (push)
```

**Сайт стал доступен по ссылке:** [https://hellbane.sourcecraft.site/site/](https://hellbane.sourcecraft.site/site/)

### 4. Выводы

- В ходе выполнения лабораторной работы был настроен полностью автоматизированный пайплайн сборки и публикации статического сайта с использованием платформы SourceCraft.

- Выполнение работы закрепило практические навыки настройки CI/CD-процессов, работы с Git-ветками.

---

**Дата выполнения:** 26.03.2026