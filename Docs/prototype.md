# Прототип API

## Замечания
1. Домен для запросов - http://dev3.fantlab.org:4242/
2. Поддерживается 2 варианта выдачи: `JSON` (по-умолчанию) и `Protobuf`. Для использования последнего необходимо добавить хэдер `Accept: application/x-protobuf`.
3. В выдаче `JSON` отсутствуют поля, если их значение совпадает с дефолтным (0 для int, пустая строка для строк etc). Это ограничение `Protobuf`.
4. Некоторые поля имеют сложносоставленное название (например, `topic_type`). Это сделано специально, чтобы избежать коллизий с зарезервированными словами из Java/Kotlin/Swift при генерации моделей через рефлексию.
5. Тип поля DateTime имеет разное значение в `JSON` и `Protobuf`. В первом случае это строка в формате `{год}-{месяц}-{день}T{часов}:{минут}:{секунд}Z` ([RFC 3339](https://www.ietf.org/rfc/rfc3339.txt)), во втором - UNIX-таймштамп.

## Содержание
1. Авторизация
    * Логин
2. Форумы
    * Список форумов
    * Форум
    * Тема
3. Блоги
    * Список рубрик
    * Список авторских колонок
    * Авторская колонка

## Авторизация
### Логин
Запрос
```
POST /v1/login
```
Параметры
```
login - логин пользователя
password - пароль пользователя
```
Ответ
```
{
    user_id: Int,                   # id пользователя
    session_token: String           # токен сессии (действителен 1 год)
}
```
Возможные ошибки
```
Пользователь уже залогинен
{
    error_code: 405,
    message: "log out first"
}

Пользователь не найден
{
    error_code: 404,
    message: "user not found"    
}

Неверный пароль
{
    error_code: 401,
    message: "incorrect_password"
}

Не удалось создать сессию
{
    error_code: 500,
    message: "failed to create session"
}
```
Примечание
```
Полученный session_token следует подставлять в хэдер X-Session
```

## Форумы
### Список форумов
Запрос
```
GET /v1/forums
```
Ответ
```
{
    forum_blocks:[                                 # список блоков форумов
        {
            id: Int,                               # id блока форумов
            title: String,                         # название блока форумов
            forums:[                               # список форумов
                {
                    id: Int,                       # id форума
                    title: String,                 # название форума
                    forum_description: String,     # описание форума
                    moderators:[                   # список модераторов форума
                        {
                            id: Int,               # id пользователя
                            login: String          # логин пользователя
                        },
                        ...
                    ],
                    stats: {                       # статистика форума
                        topic_count: Int,          # количество тем в форуме
                        message_count: Int         # количество сообщений в форуме
                    },
                    last_message: {                # последнее сообщение в форуме
                        id: Int,                   # id сообщения
                        topic: {                   # тема последнего сообщения
                            id: Int,               # id темы
                            title: String          # название темы
                        },
                        user: {                    # автор сообщения
                            id: Int,               # id пользователя
                            login: String          # логин пользователя
                        },
                        date: DateTime             # дата и время сообщения
                    }
                },
                ...
            ]
        },
        ...
    ]
}
```

### Форум
Запрос
```
GET /v1/forums/{id}?page={page}&limit={limit}
```
Параметры
```
id - id форума
page - номер страницы
limit - количество тем на одной странице (необязательный, по-умолчанию 20)
```
Ответ
```
{
    topics:[                               # список тем
        {
            id: Int,                       # id темы
            title: String,                 # название темы
            topic_type: String,            # тип темы (TOPIC (тема) или POLL (опрос))
            creation: {                    # данные о создании
                user: {                    # автор
                    id: Int,               # id пользователя
                    login: String          # логин пользователя
                },
                date: DateTime             # дата и время создания
            },
            is_closed: Boolean,            # тема закрыта?
            is_pinned: Boolean,            # тема закреплена?
            stats: {                       # статистика темы
                message_count: Int,        # количество сообщений
                views_count: Int           # количество просмотров
            },
            last_message: {                # последнее сообщение в теме
                id: Int,                   # id сообщения
                user: {                    # автор сообщения
                    id: Int,               # id пользователя
                    login: String          # логин пользователя
                },
                date: DateTime             # дата и время сообщения
            }
        },
        ...
    ]
}
```
Возможные ошибки
```
Некорректный id форума
{
    error_code: 400,
    message: "incorrect forum id: {id}"
}

Некорректный номер страницы
{
    error_code: 400,
    message: "incorrect page: {page}"
}

Некорректный лимит
{
    error_code: 400,
    message: "incorrect limit: {limit}"
}
```

### Тема
Запрос
```
GET /v1/topics/{id}?page={page}&limit={limit}
```
Параметры
```
id - id темы
page - номер страницы
limit - лимит сообщений в теме (необязательный, по-умолчанию 20)
```
Ответ
```
{
    messages:[                              # список сообщений в теме
        {
            id: Int,                        # id сообщения
            creation: {                     # данные о создании
                user: {                     # автор сообщения
                    id: Int,                # id пользователя
                    login: String,          # логин пользователя
                    gender: String,         # пол пользователя (MALE или FEMALE)
                    avatar: String,         # ссылка на аватар пользователя
                    class: Int              # класс пользователя
                },
                date: DateTime              # дата и время отправки сообщения
            },
            text: String,                   # текст сообщения
            stats: {                        # статистика сообщения
                plus_count: Int,            # количество плюсов
                minus_count: Int            # количество минусов
            }
        },
        ...
    ]
}
```
Возможные ошибки
```
Некорректный id темы
{
    error_code: 400,
    message: "incorrect topic id: {id}"
}

Некорректный номер страницы
{
    error_code: 400,
    message: "incorrect page: {page}"
}

Некорректный лимит
{
    error_code: 400,
    message: "incorrect limit: {limit}"
}
```

## Блоги
### Список рубрик
Запрос
```
GET /v1/communities
```
Ответ
```
{
    main:[                                         # основные рубрики
        {
            id: Int,                               # id рубрики
            title: String,                         # название рубрики
            community_description: String,         # описание рубрики
            stats: {                               # статистика
                article_count: Int,                # количество статей
                subscriber_count: Int              # количество подписчиков
            },
            last_article: {                        # последняя статья в рубрике
                id: Int,                           # id статьи
                title: String,                     # название статьи
                user: {                            # автор статьи
                    id: Int,                       # id пользователя
                    login: String                  # логин пользователя
                },
                date: DateTime                     # дата и время публикации статьи
            }
        },
        ...
    ],
    additional:[                                   # дополнительные рубрики
        ...
    ]
}
```

### Список авторских колонок
Запрос
```
GET /v1/blogs?page={page}&limit={limit}
```
Параметры
```
page - номер страницы
limit - количество авторских колонок на одной странице (необязательный, по-умолчанию 50)
```
Ответ
```
{
    blogs:[                                 # список авторских колонок
        {
            id: Int,                        # id авторской колонки
            user: {                         # владелец колонки
                id: Int,                    # id пользователя
                login: String,              # логин пользователя
                name: String                # имя пользователя
            },
            stats: {                        # статистика
                article_count: Int,         # количество статей в колонке
                subscriber_count: Int       # количество подписчиков
            },
            last_article: {                 # последняя статья в колонке
                id: Int,                    # id статьи
                title: String,              # название статьи
                date: DateTime              # дата и время публикации статьи
            }
        },
        ...
    ]
}
```
Возможные ошибки
```
Некорректный номер страницы
{
    error_code: 400,
    message: "incorrect page: {page}"
}

Некорректный лимит
{
    error_code: 400,
    message: "incorrect limit: {limit}"
}
```

### Авторская колонка
Запрос
```
GET /v1/blogs/{id}?page={page}&limit={limit}
```
Параметры
```
id - id авторской колонки
page - номер страницы
limit - количество статей на одной странице (необязательный, по-умолчанию 5)
```
Ответ
```
{
    articles:[                           # список статей
        {
            id: Int,                     # id статьи
            title: String,               # название статьи
            creation: {                  # данные о создании
                user: {                  # автор
                    id: Int,             # id пользователя
                    login: String,       # логин пользователя
                    gender: String,      # пол пользователя (MALE или FEMALE)
                    avatar: String       # ссылка на аватар пользователя
                },
                date: DateTime           # дата публикации статьи
            },
            text: String,                # текст статьи
            tags: String,                # список тегов
            stats: {                     # статистика
                like_count: Int,         # количество лайков
                view_count: Int,         # количество просмотров
                comment_count: Int       # количество комментариев
            }
        },
        ...
    ]
}
```
Возможные ошибки
```
Некорректный id авторской колонки
{
    error_code: 400,
    message: "incorrect blog id: {id}"
}

Некорректный номер страницы
{
    error_code: 400,
    message: "incorrect page: {page}"
}

Некорректный лимит
{
    error_code: 400,
    message: "incorrect limit: {limit}"
}
```
