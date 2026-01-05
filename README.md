# Continuous Integration

## Мета роботи
Налаштувати GitHub Workflow для автоматичної збірки Docker-образу front-end репозиторію та його завантаження у GitHub Container Registry.

## Опис виконаних дій

### 1. Створено GitHub Workflow
У репозиторії створено workflow-файл:
.github/workflows/docker-publish.yml


Workflow налаштований з двома тригерами:
- ручний запуск (workflow_dispatch)
- автоматичний запуск при push-коміті у гілки:
  - main
  - feature/*

### 2. Налаштовано job для збірки та публікації Docker-образу
Workflow містить один job, який виконує наступні кроки:

- Клонування репозиторію з використанням actions/checkout
- Встановлення менеджера пакетів pnpm за допомогою npm
- Встановлення залежностей та збірка front-end проєкту командою pnpm run build
  - усі команди виконуються в одному скрипті
- Авторизація в GitHub Container Registry з використанням:
  - імені користувача з GitHub context
  - вбудованого `GITHUB_TOKEN` як паролю
- Збірка та публікація Docker-образу з використанням docker/build-push-action
  - контекст збірки — поточна директорія
  - параметр push встановлено в true
  - тег образу має вигляд: ghcr.io/<логін>/<ім’я_репозиторію>

### 3. Створено мінімальний front-end проєкт
Для коректної роботи workflow у репозиторії додано мінімальний front-end проєкт:
- файл package.json зі скриптом build
- Dockerfile для створення Docker-образу зі статичними файлами

### 4. Перевірка результату
Після виконання workflow Docker-образ успішно опубліковано у GitHub Container Registry.  
У вкладці **Packages** GitHub відображається контейнер з ім’ям, що відповідає імені репозиторію.

## Висновок
У результаті роботи було створено та налаштовано GitHub Workflow для автоматичної збірки та публікації Docker-образу front-end проєкту в GitHub Container Registry.
