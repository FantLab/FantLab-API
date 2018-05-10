# Издание

## Основная информация
```
GET /edition/{id}
```

Пример:
> [/edition/1](https://api.fantlab.ru/edition/1) - Дэн Симменс "Гиперион"


Ответ:
```
{
    edition_id: Int,         # id издания
    edition_name: String,    # название издания
    image: String,           # ссылка на основную обложку издания
    image_preview: String,   # ссылка на превью основной обложки
    creators: {
        authors: [ |null                # авторы
            {
                id: Int,                # id автора
                is_opened: Boolean,     # открыта ли страница автора
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
    edition_type_id: Int,      # id типа издания
    edition_type: String,      # тип издания
    edition_type_plus: [       # доп. типы издания
        ...: String|null,
        ...
    ],
    year: Int                  # год издания
    copies: Int,               # тираж (0, если неизвестен)
    correct_level: Float,      # степень проверенности издания (0 - не проверено, 0.5 - не полностью проверено, 1 - проверено)
    cover_type: String,        # тип обложки
    format: String,            # формат издания
    format_mm: String,         # формат издания (в мм.)
    isbns: [                   # все isbn-коды издания
        ...: String|null,
        ...
    ],
    lang: String,              # язык издания
    lang_code: String,         # код языка издания
    last_modified: DateTime,   # дата последнего редактирования
    pages: Int,                # количество страниц
    preread: Boolean,          # есть ли отрывок для чтения
    series: [                  # серии, в которые входит издание
        { |null
            id: Int,                  # id серии
            is_opened: Boolean,       # открыта ли страница серии
            name: String,             # название серии
            type: String              # тип (series) 
        },
        ...
    ],
    description: String,       # описание
    notes: String,             # примечания

    plan_date: String,         # план выхода (дата текстом). если поле не пустое - значит издание еще невышедшие
    plan_description: Srting,  # примечание к плану выхода издания

}
```

## Расширенная информация
```
GET /edition/{id}/extended
```
в расширенной информации о произведении добавляются поля:  
*content           - выводить ли содержание издания (необязательный; по-умолчанию 0)
images_plus       - выводить ли доп. изображения (необязательный; по-умолчанию 0)*

Пример:
> [/edition/35437/extended](https://api.fantlab.ru/edition/35437/extended) - Стивен Кинг "Верткая пуля"


Ответ:
```
{
    edition_id: Int,         # id издания
    ... все поля обычной карточки издания (см. выше) и добаляются новые: ...

    content: [ |null         # содержание
        ...: String,
        ...
    ],

    images_plus: { |null     # изображения в издании - доп.обложки и иллюстрации
        cover: [             # обложки
             {           
                image: Url,                     # обложка
                image_preview: Url,             # ссылка на превью обложки
                image_orig: Url,                # обложка в большом разрешеним, если есть
                image_spine: Url                # корешок оложки, если он есть
                pic_text: String,               # описание или копирайт изображения
            },
            ...
        ],
        plus: [ |null        # доп. изображения и иллюстрации
            {
                image: Url,                     # картинка
                image_preview: Url,             # ссылка на превью 
                pic_text: String,               # описание или копирайт изображения
            },
            ...
        ],
    },
```



--- 

**TODO:**

- доработать оглавление-иерархию в выдаче содержания

