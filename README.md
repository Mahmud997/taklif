# Shaqyrtu — конструктор приглашений

Статический проект для GitHub Pages + Firebase. В проекте сохранены три страницы:
- `index.html` — редактор приглашений (доступ только админам)
- `invite.html?id=ID` — публичная страница приглашения
- `admin.html` — RSVP, пожелания и управление файлами Storage
- `firebase-config.js` — конфигурация Firebase
- `firestore.rules`, `storage.rules` — правила безопасности

## 1. Настройка Firebase

В Firebase Console создайте проект и включите:
1. **Authentication → Sign-in method → Google**.
2. **Firestore Database**.
3. **Storage**.

В **Project settings → Your apps → Web app** скопируйте конфигурацию и заполните значения в `firebase-config.js`. Не меняйте имя экспорта `firebaseConfig`.

В **Authentication → Settings → Authorized domains** добавьте `ВАШ-ЛОГИН.github.io` и `localhost` для локального тестирования.

## 2. Опубликовать правила безопасности

Скопируйте `firestore.rules` в Firebase Console → Firestore Database → Rules и нажмите Publish.
Скопируйте `storage.rules` в Firebase Console → Storage → Rules и нажмите Publish.

Правила ограничивают создание/редактирование приглашений админами. Документы админов нельзя создавать из приложения — назначайте админов вручную в Firebase Console.

## 3. Добавить первого администратора

1. Опубликуйте сайт или запустите локальный сервер.
2. Откройте `admin.html` и войдите через Google.
3. На странице отказа в доступе скопируйте свой UID.
4. В Firestore создайте коллекцию `admins`, документ с ID, равным UID, и поле `role` типа string со значением `admin`.
5. Обновите `admin.html`. Для второго администратора повторите процесс с его UID.

## 4. Запуск без Node.js

ES-модули не работают при открытии HTML через `file://`. Варианты:
- опубликовать репозиторий на GitHub Pages;
- либо, если установлен Python, из папки проекта выполнить `python -m http.server 8000`, затем открыть `http://localhost:8000`.

Node.js и сборка не требуются.

## 5. GitHub Pages

Загрузите содержимое этой папки в корень репозитория. Откройте Settings → Pages → Build and deployment → Deploy from a branch, выберите `main` и `/ (root)`. После публикации ссылки будут иметь вид:
- `https://ВАШ-ЛОГИН.github.io/ИМЯ-РЕПОЗИТОРИЯ/`
- `https://ВАШ-ЛОГИН.github.io/ИМЯ-РЕПОЗИТОРИЯ/invite.html?id=ID`

## Проверка перед запуском

- Заполните `firebase-config.js` настоящими данными проекта.
- Убедитесь, что Google Auth, Firestore и Storage включены.
- Опубликуйте оба набора правил.
- Создайте документ администратора.
- Музыка начинает играть только после действия пользователя — это ограничение браузеров.
