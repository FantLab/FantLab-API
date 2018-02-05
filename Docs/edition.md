# Издание
Запрос
```
GET /edition/{id}?content={0/1}&images_plus={0/1}
```
Параметры
```
id - id издания,
content - выводить ли содержание издания (необязательный),
images_plus - выводить ли доп. изображения (необязательный)
```
Пример
```
/edition/35437?content=1&images_plus=1
```
Альтернативный запрос для получения всей информации
```
GET /edition/{id}/extended
```
Ответ
```
{
    content: ?|null,         # содержание
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
    image: String,           # ссылка на обложку издания
    image_preview: String,   # ссылка на превью обложки
    images_plus: ?|null,
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
            id: Int,         # id серии
            is_opened: Int,  # открыта ли страница серии (1 - да, 0 - нет)
            name: String,    # название серии
            type: String     # тип (series) 
        },
        ...
    ],
    type: Int,               # тип издания
    volume: ?,               # ?
    year: Int                # год издания
}
```
