### Проект REST API для проекта Yatube:

Возможности:

Просматривать, создавать новые, удалять и изменять посты.
Просматривать и создавать группы.
Комментировать, смотреть, удалять и обновлять комментарии.
Выполнять поиск по полям.
Подписываться на пользователя.

### Как запустить проект:

Установить Python 3
```

Клонировать репозиторий и перейти в него в командной строке:

```
git clone git@github.com:haddaway11/api_yatube.git
```

```
cd api_final_yatube
```

Cоздать и активировать виртуальное окружение:

```
python3 -m venv env
```

```
source env/bin/activate
```

```
python3 -m pip install --upgrade pip
```

Установить зависимости из файла requirements.txt:

```
pip install -r requirements.txt
```

Выполнить миграции:

```
python3 manage.py migrate
```

Запустить проект:

```
python3 manage.py runserver
```

### Документация к API проекта Yatube:

api/v1/posts/

GET: Получить список всех публикаций. При указании параметров limit и offset выдача должна работать с пагинацией.
POST: Добавление новой публикации в коллекцию публикаций. Анонимные запросы запрещены.

api/v1/posts/{id}/

GET: Получение публикации по id.
PUT: Обновление публикации по id. Обновить публикацию может только автор публикации. Анонимные запросы запрещены.
PATCH: Частичное обновление публикации по id. Обновить публикацию может только автор публикации. Анонимные запросы запрещены.
DEL: Удаление публикации по id. Удалить публикацию может только автор публикации. Анонимные запросы запрещены.

api/v1/posts/{post_id}/comments/

GET: Получение всех комментариев к публикации.
POST: Добавление нового комментария к публикации. Анонимные запросы запрещены.

api/v1/posts/{post_id}/comments/{id}/

GET: Получение комментария к публикации по id.
PUT: Обновление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.
PATCH: Частичное обновление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.
DEL: Удаление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.

api/v1/groups/

GET: Получение списка доступных сообществ.

api/v1/groups/{id}/

GET: Получение информации о сообществе по id.

api/v1/follow/

GET: Возвращает все подписки пользователя, сделавшего запрос. Анонимные запросы запрещены.
POST: Подписка пользователя от имени которого сделан запрос на пользователя переданного в теле запроса. Анонимные запросы запрещены.

api/v1/jwt/create/

POST: Получение JWT-токена.

api/v1/jwt/refresh/

POST: Обновить JWT-токен

api/v1/jwt/verify/

POST: Проверка JWT-токена.
