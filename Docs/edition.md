# Издание
## Основная информация
Запрос
```
GET /edition/{id}?content={0|1}&images_plus={0|1}
```
Параметры
```
id - id издания,
content - выводить ли содержание издания (необязательный; по-умолчанию 0),
images_plus - выводить ли доп. изображения (необязательный; по-умолчанию 0)
```
Пример
```
/edition/35437?content=1&images_plus=1 - Стивен Кинг "Верткая пуля"
```
Альтернативный запрос для получения всей информации
```
GET /edition/{id}/extended
```
Ответ
```
{
    content: [ |null         # [content] содержание
        ...: String,
        ...
    ],
    copies: Int,             # тираж (0, если неизвестен)
    correct_level: Float,    # степень проверенности издания (0 - не проверено, 0.5 - не полностью проверено, 1 - проверено)
    cover_type: String,      # тип обложки
    creators: {
        authors: [ |null                # авторы
            {
                id: Int,                # id автора
                is_opened: Int,         # открыта ли страница автора (1 - да, 0 - нет)
                name: String,           # имя автора
                type: String            # тип автора (autor - автор, art - художник)
            },
            ...
        ],
        compilers: [ |null              # составители
            {
                id: Int|null,           # id составителя
                name: String,           # имя составителя (может быть "не указан")
                type: String|null       # тип (compiler)
            },
            ...
        ],
        publishers: [ |null             # издатели
            {
                id: Int|null,           # id издателя
                name: String,           # название издателя
                type: String|null       # тип (publisher)
            },
            ...
        ]
    },
    description: String,     # описание
    edition_id: Int,         # id издания
    edition_name: String,    # название издания
    edition_type: String,    # тип издания
    edition_type_plus: [     # доп. типы издания
        ...: String|null,
        ...
    ],
    format: String,          # формат издания
    format_mm: String,       # формат издания (в мм.)
    image: String,           # ссылка на основную обложку издания
    image_preview: String,   # ссылка на превью основной обложки
    images_plus: { |null     # [images_plus] доп. изображения
        cover: {             # обложки
            {N}: {           # порядковый номер обложки (используется для сопоставления корешка и обложки)
                check: Int,                     # ?
                image: String,                  # ссылка на обложку
                image_preview: String|null,     # ссылка на превью обложки
                pic_copyright: ?,               # ?
                pic_orig: Int,                  # ?
                pic_text: String,               # ?
                pic_type: Int                   # ?
            },
            ...
        },
        plus: [ |null        # доп. изображения (суперобложки и т.д.)
            {
                ...          # те же самые поля, что и в cover -> int
            },
            ...
        ],
        spine: { |null       # корешки
            {N}: {           # порядковый номер корешка (для сопоставления обложке)
                ...          # те же самые поля, что и в cover -> int
            },
            ...
        }
    },
    isbns: [                 # все isbn-коды издания
        ...: String|null,
        ...
    ],
    lang: String,            # язык издания
    lang_code: String,       # код языка издания
    last_modified: Date,     # дата последнего редактирования (в формате YYYY-MM-DD HH:mm:SS)
    notes: String,           # примечания
    pages: Int,              # количество страниц
    plan_date: ?,            # ?
    plan_description: ?,     # ?
    preread: Int,            # есть ли отрывок для чтения (1 - да, 0 - нет)
    series: [                # серии, в которые входит издание
        { |null
            id: Int,             # id серии
            is_opened: Int,      # открыта ли страница серии (1 - да, 0 - нет)
            name: String,        # название серии
            type: String         # тип (series) 
        },
        ...
    ],
    type: Int,               # тип издания
    volume: ?,               # ?
    year: Int                # год издания
}
```
## Добавление или удаление с книжной полки
Запрос
```
/GET /bookcaseclick{edition_id}bc{bookcase_id}change{true|false}
```
Параметры
```
edition_id - id издания
bookcase_id - id книжной полки
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
/GET /bookcaseclick35437bc1changetrue - добавить "Верткую пулю" Стивена Кинга на книжную полку с id = 1
```
Ответ
```
HTML-разметка страницы издания
```
*TODO:* отдельный endpoint для работы с книжными полками?

**Nota bene:** узнать, на каких полках есть произведение, пока невозможно
