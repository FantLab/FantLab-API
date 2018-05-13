# Издание

## Основная информация
Запрос
```
GET /edition/{id}?content={0|1}&images_plus={0|1}
```
Параметры
```
id - id издания
content - выводить ли содержание (необязательный; по-умолчанию 0)
images_plus - выводить ли доп. изображения (необязательный; по-умолчанию 0)
```
Пример
```
GET /edition/1?content=1&images_plus=1 - "Гиперион" Дэна Симмонса
```
Альтернативный запрос для получения всей информации
```
GET /edition/{id}/extended
```
Ответ
```
    content: [ |null          # [content] содержание
        ...: String,
        ...
    ],
    copies: Int,              # тираж (0, если неизвестен)
    correct_level: Float,     # степень проверенности издания (0 - не проверено, 0.5 - не полностью проверено, 1 - проверено)
    cover_type: String,       # тип обложки
    creators: {
        authors: [ |null               # список авторов
            {
                id: Int,               # id автора
                is_opened: Boolean,    # открыта ли страница автора
                name: String,          # имя автора
                type: String           # тип (autor - автор, art - художник)
            }
        ],
        compilers: [ |null             # список составителей (если сборник/антология)
            {
                id: Int|null,          # id составителя
                name: String,          # имя составителя (может быть "не указан")
                type: String|null      # тип (compiler)
            }
        ],
        publishers: [ |null            # список издателей
            {
                id: Int|null,          # id издателя
                name: String,          # название издателя
                type: String|null      # тип (publisher)
            }
        ]
    },
    description: String,      # описание
    edition_id: Int,          # id издания
    edition_name: String,     # название издания
    edition_type: String,     # тип издания
    edition_type_plus: [      # доп. типы издания
        ...: String|null,
        ...
    ],
    format: String,           # формат издания ("0", если неизвестен)
    format_mm: String?,       # формат издания (в мм.)
    image: Url,               # основная обложка издания
    image_preview: Url,       # превью основной обложки
    images_plus: [ |null                  # [images_plus] доп.обложки и иллюстрации
        cover: [                          # список обложек
            {
                image: Url,               # обложка
                image_orig: Url,          # обложка в большом разрешеним, если есть
                image_preview: Url,       # превью обложки
                image_spine: Url,         # корешок оложки, если он есть
                pic_text: String          # описание или копирайт изображения
            }
        ],
        plus: [ |null                     # доп. изображения и иллюстрации
            {
                image: Url,               # изображение
                image_preview: Url,       # превью изображения
                pic_text: String          # описание или копирайт изображения
            }
        ]
    ],
    isbns: [                   # список ISBN
        ...: String|null,
        ...
    ],
    lang: String,              # язык издания
    lang_code: String,         # код языка издания
    notes: String,             # примечания
    pages: Int,                # количество страниц
    plan_date: String,         # план выхода (дата текстом). если поле не пустое - значит издание еще не вышло
    plan_description: Srting,  # примечание к плану выхода издания
    preread: Boolean,          # есть ли отрывок для чтения
    series: [                         # серии, в которые входит издание
        { |null
            id: Int,                  # id серии
            is_opened: Boolean,       # открыта ли серия
            name: String,             # название серии
            type: String              # тип (series)
        }
    ],
    year: Int      # год издания
```
--- 

**TODO:**
- доработать оглавление-иерархию в выдаче содержания

