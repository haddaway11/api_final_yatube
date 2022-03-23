Проект REST API для проекта Yatube

Возможности:

Просматривать, создавать новые, удалять и изменять посты.
Просматривать и создавать группы.
Комментировать, смотреть, удалять и обновлять комментарии.
Выполнять поиск по полям.
Подписываться на пользователя.

### Как запустить проект:

Установить Python 3

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

Документация к API проекта Yatube:

Получение публикаций
Получить список всех публикаций. При указании параметров limit и offset выдача должна работать с пагинацией.
/api/v1/posts/

QUERY PARAMETERS
limit	
integer
Количество публикаций на страницу

offset	
integer
Номер страницы после которой начинать выдачу

Responses
200 Удачное выполнение запроса без пагинации
RESPONSE SCHEMA: application/json
Array 
id	
integer (id публикации)
author	
string (username пользователя)
text	
string (текст публикации)
pub_date	
string <date-time>
image	
string or null <binary>
group	
integer or null (id сообщества)

GET
/api/v1/posts/
http://127.0.0.1:8000/api/v1/posts/
Response samples
200
Content type
application/json

Copy
Expand allCollapse all
{
"count": 123,
"next": "http://api.example.org/accounts/?offset=400&limit=100",
"previous": "http://api.example.org/accounts/?offset=200&limit=100",
"results": [
{}
]
}
Создание публикации
Добавление новой публикации в коллекцию публикаций. Анонимные запросы запрещены.

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст публикации)
image	
string or null <binary>
group	
integer or null (id сообщества)
Responses
201 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Запрос от имени анонимного пользователя

POST
/api/v1/posts/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string",
"image": "string",
"group": 0
}
Response samples
201400401
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
Получение публикации
Получение публикации по id.

PATH PARAMETERS
id
required
integer
id публикации

Responses
200 Удачное выполнение запроса
404 Попытка запроса несуществующей публикации

GET
/api/v1/posts/{id}/
Response samples
200404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
Обновление публикации
Обновление публикации по id. Обновить публикацию может только автор публикации. Анонимные запросы запрещены.

PATH PARAMETERS
id
required
integer
id публикации

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст публикации)
image	
string or null <binary>
group	
integer or null (id сообщества)
Responses
200 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка изменения несуществующей публикации

PUT
/api/v1/posts/{id}/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string",
"image": "string",
"group": 0
}
Response samples
200400401403404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
Частичное обновление публикации
Частичное обновление публикации по id. Обновить публикацию может только автор публикации. Анонимные запросы запрещены.

PATH PARAMETERS
id
required
integer
id публикации

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст публикации)
image	
string or null <binary>
group	
integer or null (id сообщества)
Responses
200 Удачное выполнение запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка изменения несуществующей публикации

PATCH
/api/v1/posts/{id}/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string",
"image": "string",
"group": 0
}
Response samples
200401403404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
Удаление публикации
Удаление публикации по id. Удалить публикацию может только автор публикации. Анонимные запросы запрещены.

PATH PARAMETERS
id
required
integer
id публикации

Responses
204 Удачное выполнение запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка удаления несуществующей публикации

DELETE
/api/v1/posts/{id}/
Response samples
401403404
Content type
application/json

Copy
Expand allCollapse all
{
"detail": "Учетные данные не были предоставлены."
}
Получение комментариев
Получение всех комментариев к публикации.

PATH PARAMETERS
post_id
required
integer
id публикации

Responses
200 Удачное выполнение запроса
404 Получение списка комментариев к несуществующей публикации

GET
/api/v1/posts/{post_id}/comments/
Response samples
200404
Content type
application/json

Copy
Expand allCollapse all
[
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
]
Добавление комментария
Добавление нового комментария к публикации. Анонимные запросы запрещены.

PATH PARAMETERS
post_id
required
integer
id публикации

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст комментария)
Responses
201 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Запрос от имени анонимного пользователя
404 Попытка добавить комментарий к несуществующей публикации

POST
/api/v1/posts/{post_id}/comments/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string"
}
Response samples
201400401404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
Получение комментария
Получение комментария к публикации по id.

PATH PARAMETERS
post_id
required
integer
id публикации

id
required
integer
id комментария

Responses
200 Удачное выполнение запроса
404 Попытка запросить несуществующий комментарий или к несуществующей публикации

GET
/api/v1/posts/{post_id}/comments/{id}/
Response samples
200404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
Обновление комментария
Обновление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.

PATH PARAMETERS
post_id
required
integer
id публикации

id
required
integer
id комментария

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст комментария)
Responses
200 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка изменить несуществующий комментарий или к несуществующей публикации

PUT
/api/v1/posts/{post_id}/comments/{id}/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string"
}
Response samples
200400401403404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
Частичное обновление комментария
Частичное обновление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.

PATH PARAMETERS
post_id
required
integer
id публикации

id
required
integer
id комментария

REQUEST BODY SCHEMA: 
application/json
text
required
string (текст комментария)
Responses
200 Удачное выполнение запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка изменить несуществующий комментарий или к несуществующей публикации

PATCH
/api/v1/posts/{post_id}/comments/{id}/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"text": "string"
}
Response samples
200401403404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"author": "string",
"text": "string",
"created": "2019-08-24T14:15:22Z",
"post": 0
}
Удаление комментария
Удаление комментария к публикации по id. Обновить комментарий может только автор комментария. Анонимные запросы запрещены.

PATH PARAMETERS
post_id
required
integer
id публикации

id
required
integer
id комментария

Responses
204 Удачное выполнение запроса
401 Запрос от имени анонимного пользователя
403 Попытка изменения чужого контента
404 Попытка удалить несуществующий комментарий или к несуществующей публикации

DELETE
/api/v1/posts/{post_id}/comments/{id}/
Response samples
401403404
Content type
application/json

Copy
Expand allCollapse all
{
"detail": "Учетные данные не были предоставлены."
}
Список сообществ
Получение списка доступных сообществ.

Responses
200 Удачное выполнение запроса

GET
/api/v1/groups/
Response samples
200
Content type
application/json

Copy
Expand allCollapse all
[
{
"id": 0,
"title": "string",
"slug": "string",
"description": "string"
}
]
Информация о сообществе
Получение информации о сообществе по id.

PATH PARAMETERS
id
required
integer
id сообщества

Responses
200 Удачное выполнение запроса
404 Попытка запроса несуществующего сообщества

GET
/api/v1/groups/{id}/
Response samples
200404
Content type
application/json

Copy
Expand allCollapse all
{
"id": 0,
"title": "string",
"slug": "string",
"description": "string"
}
Подписки
Возвращает все подписки пользователя, сделавшего запрос. Анонимные запросы запрещены.

QUERY PARAMETERS
search	
string
Возможен поиск по подпискам по параметру search

Responses
200 Удачное выполнение запроса
401 Запрос от имени анонимного пользователя

GET
/api/v1/follow/
Response samples
200401
Content type
application/json

Copy
Expand allCollapse all
[
{
"user": "string",
"following": "string"
}
]
Подписка
Подписка пользователя от имени которого сделан запрос на пользователя переданного в теле запроса. Анонимные запросы запрещены.

REQUEST BODY SCHEMA: 
application/json
following
required
string (username)
Responses
201 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса или оно не соответствует требованиям
401 Запрос от имени анонимного пользователя

POST
/api/v1/follow/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"following": "string"
}
Response samples
201400401
Content type
application/json

Copy
Expand allCollapse all
{
"user": "string",
"following": "string"
}
Получить JWT-токен
Получение JWT-токена.

REQUEST BODY SCHEMA: 
application/json
username
required
string
password
required
string
Responses
200 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Переданная учетная запись не существует

POST
/api/v1/jwt/create/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"username": "string",
"password": "string"
}
Response samples
200400401
Content type
application/json

Copy
Expand allCollapse all
{
"refresh": "string",
"access": "string"
}
Обновить JWT-токен
Обновление JWT-токена.

REQUEST BODY SCHEMA: 
application/json
refresh
required
string
Responses
200 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Передан невалидный токен

POST
/api/v1/jwt/refresh/
Request samples
Payload
Content type
application/json

Copy
Expand allCollapse all
{
"refresh": "string"
}
Response samples
200400401
Content type
application/json

Copy
Expand allCollapse all
{
"access": "string"
}
Проверить JWT-токен
Проверка JWT-токена.

REQUEST BODY SCHEMA: 
application/json
token
required
string
Responses
200 Удачное выполнение запроса
400 Отсутствует обязательное поле в теле запроса
401 Передан невалидный токен