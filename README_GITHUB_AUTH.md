# Настройка GitHub OAuth для DATATERM

## 1. Создание GitHub OAuth Application

1. Перейдите на https://github.com/settings/developers
2. Нажмите "New OAuth App" или "Register a new application"
3. Заполните форму:
   - **Application name**: DATATERM (или любое другое название)
   - **Homepage URL**: http://localhost:8080 (или ваш домен)
   - **Authorization callback URL**: http://localhost:8080/callback.html
4. Нажмите "Register application"
5. Скопируйте **Client ID** (начинается с Ov23...)
6. Нажмите "Generate a new client secret" и скопируйте секрет

## 2. Настройка index.html

Откройте файл `index.html` и найдите строки:

```javascript
const CLIENT_ID = 'Ov23liXXXXXXXXXXXX'; // Замените на ваш Client ID
const CLIENT_SECRET = 'YOUR_CLIENT_SECRET'; // Замените на ваш Client Secret
```

Замените значения на полученные от GitHub.

## 3. Важные замечания по безопасности

### ⚠️ ВНИМАНИЕ: Текущая реализация использует client-side OAuth flow

Это **НЕБЕЗОПАСНО** для production использования, так как:
- Client Secret виден в исходном коде
- Токены хранятся в localStorage

### Рекомендуемый подход для production:

1. Создайте backend-сервер (Node.js, Python, etc.)
2. Обменивайте code на token на сервере
3. Храните токены на сервере или используйте secure HTTP-only cookies
4. Сервер должен проверять права доступа к репозиторию

## 4. Альтернатива: Personal Access Token

Для тестирования можно использовать Personal Access Token:

1. Перейдите на https://github.com/settings/tokens
2. Нажмите "Generate new token (classic)"
3. Выберите scope: **repo** (Full control of private repositories)
4. Скопируйте токен (начинается с ghp_...)
5. Вставьте токен в поле "Personal Access Token" на странице входа

## 5. Проверка прав доступа

После авторизации система проверяет:
- Есть ли у пользователя права **push** к репозиторию DARKNEET69/DATATERM
- Если права есть - отображается индикатор [EDIT] зелёного цвета
- Только пользователи с [EDIT] могут сохранять изменения

## 6. Структура файлов

```
/workspace/
├── index.html          # Основной файл приложения
├── db.js               # База данных (вынесена отдельно)
├── callback.html       # Страница обратного вызова OAuth
└── README_GITHUB_AUTH.md # Эта инструкция
```

## 7. Как это работает

1. Пользователь нажимает "Войти через GitHub"
2. Открывается окно авторизации GitHub
3. После подтверждения GitHub перенаправляет на callback.html с code
4. code отправляется обратно в основное окно через postMessage
5. Приложение обменивает code на access token
6. Проверяются права доступа к репозиторию
7. Если есть права push - пользователь может редактировать данные

## 8. Редактирование данных

Пользователи с правами [EDIT] могут:
- Создавать новые записи
- Редактировать существующие
- Удалять записи
- Сохранять изменения напрямую в репозиторий GitHub

При сохранении создаётся commit с сообщением "Update DB via DATATERM UI".
