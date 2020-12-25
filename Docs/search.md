# Поиск по библиографической базе

Общие замечания по выдаче:
1. Все запросы, кроме общего поиска, поддерживают пагинацию. На одной странице выдается по 25 результатов, максимально возможное общее количество результатов равно 1000.
2. Во всех ответах сервера есть поле *attrs*, содержащее список полей в выдаче *matches*. Каждое поле обозначено именем и целочисленным типом. Например, "1" - Int, "7" - String.
## Общая структура ответа
```
{
    attrs: {              # список полей в выдаче
        {attr}: Int,
        ...
    },
    error: String,        # текст ошибки, если есть
    fields: [             # список полей, по которым осуществляется поиск
        ...: String,
        ...
    ],
    matches: [            # список найденных результатов
        {
            ...
        },
        ...
    ],
    time: Float,          # длительность поиска в секундах
    total: Int,           # общее количество результатов
    total_found: Int,     # общее количество найденных результатов (но получить можно максимум total)
    type: String,         # категория поиска
    warning: String,      # предупреждение, если есть
    words: {
        {word}: {
            docs: Int,    # количество найденных совпадений по данному слову
            hits: Int     # количество найденных уникальных совпадений по данному слову
        },
        ...
    }
}
```


## Поиск авторов

Запрос
```
GET /search-autors?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример
> [/search-autors?q=Asimov&page=1&onlymatches=1](https://api.fantlab.ru/search-autors?q=Asimov&page=1&onlymatches=1) - поиск по фразе "Asimov"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        autor_id: Int,           # id автора
        birthyear: Int,          # год рождения
        country: String,         # страна
        country_id: Int,         # id страны
        deathyear: Int,          # год смерти
        doc: Int,                # ?
        editioncount: Int,       # количество изданий
        is_opened: Boolean,      # открыта ли страница автора
        markcount: Int,          # количество оценок
        midmark: Int,            # средняя оценка автора * 100
        moviecount: Int,         # количество фильмов (экранизаций и т.д.)
        name: String,            # имя в оригинале
        pseudo_names: String,    # список псевдонимов
        responsecount: Int,      # количество отзывов
        rusname: String,         # русскоязычное имя
        weight: Int              # степень релевантности (влияет на порядковый номер в выдаче - чем больше, тем выше)
    },
    ...
]
```


## Поиск произведений
*TODO: нуждается в структоризации полей*  

Запрос
```
GET /search-works?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-works?q=Asimov&page=1&onlymatches=1](https://api.fantlab.ru/search-works?q=Asimov&page=1&onlymatches=1) - поиск по фразе "Asimov"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        all_autor_name: String,        # список всех авторов в оригинале
        all_autor_rusname: String,     # список всех авторов на русском языке (без bb-тегов)
        altname: String,               # ?
        autor1_id: Int,                # id 1-го автора
        autor1_is_opened: Boolean,     # открыта ли страница 1-го автора
        autor1_rusname: String,        # русскоязычное имя 1-го автора
        autor2_id: Int,                # id 2-го автора
        autor2_is_opened: Boolean,     # открыта ли страница 2-го автора
        autor2_rusname: String,        # русскоязычное имя 2-го автора
        autor3_id: Int,                # id 3-го автора
        autor3_is_opened: Boolean,     # открыта ли страница 3-го автора
        autor3_rusname: String,        # русскоязычное имя 3-го автора
        autor4_id: Int,                # id 4-го автора
        autor4_is_opened: Boolean,     # открыта ли страница 4-го автора
        autor4_rusname: String,        # русскоязычное имя 4-го автора
        autor5_id: Int,                # id 5-го автора
        autor5_is_opened: Boolean,     # открыта ли страница 5-го автора
        autor5_rusname: String,        # русскоязычное имя 5-го автора
        doc: Int,                      # ?
        fullname: String,              # полное название произведения (русскоязычное + оригинальное)
        keywords: String,              # ключевые слова
        level: Int,                    # ?
        markcount: Int,                # количество оценок
        midmark: [
            ...: Float                 # средняя оценка (не рейтинг)
        ],
        name: String,                  # оригинальное название
        name_eng: String,              # англоязычное название типа произведения
        name_show_im: String,          # русскоязычное название типа произведения
        parent_work_id: Int,           # ?
        parent_work_id_present: Int,   # ?
        pic_edition_id: String,        # ?
        pic_edition_id_auto: Int,      # id сопоставленного издания (назначается автоматически)
        rusname: String,               # русскоязычное название
        weight: Int,                   # степень релевантности
        work_id: Int,                  # id произведения
        year: Int                      # год издания
    },
    ...
]
```


## Поиск изданий

Запрос
```
GET /search-editions?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0). Если query - это ISBN, то при onlymatches=0 список обернут в шаблонный объект, но из полей только matches и total.
```

Пример

> [/search-editions?q=Asimov&page=1&onlymatches=1](https://api.fantlab.ru/search-editions?q=Asimov&page=1&onlymatches=1) - поиск по фразе "Asimov"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        autors: String,         # список авторов (могут присутствовать bb-теги)
        comment: String,        # комментарий
        compilers: String,      # список составителей (могут присутствовать bb-теги)
        correct: Int,           # уровень проверенности издания (0 - красный, 1 - желтый, 2 - зеленый)
        doc: Int,               # ?
        edition_id: Int,        # id издания
        isbn1: String,          # список isbn с разделителями ("-")
        isbn2: String,          # список isbn без разделителей
        name: String,           # название (могут присутствовать bb-теги)
        notes: String,          # примечания
        plan_date: ?,           # ?
        publisher: String,      # издательство (могут присутствовать bb-теги)
        series: String,         # серия (могут присутствовать bb-теги)
        weight: Int,            # степень релевантности
        year: Int               # год издания
    },
    ...
]
```


## Поиск книжных серий

Запрос
```
GET /search-series?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-series?q=Лавкрафт&page=1&onlymatches=1](https://api.fantlab.ru/search-series?q=Лавкрафт&page=1&onlymatches=1) - поиск по фразе "Лавкрафт"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        comment: String,                   # комментарий
        description: String,               # описание (могут присутствовать bb-теги)
        doc: Int,                          # ?
        editions_count: Int,               # количество изданий в серии
        name: String,                      # название
        publisher: String,                 # издательство (могут присутствовать bb-теги)
        publisher_alt_names: String,       # ?
        publisher_comment: String,         # ?
        publisher_description: String,     # ?
        publisher_name: String,            # ?
        publisher_notes: String,           # ?
        publishers: String,                # полный список изданий
        series_id: Int,                    # id книжной серии
        weight: Int,                       # степень релевантности
        year_close: Int,                   # год закрытия серии
        year_open: Int,                    # год открытия серии
    },
    ...
]
```


## Поиск премий

Запрос
```
GET /search-awards?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-awards?q=Asimov&page=1&onlymatches=1](https://api.fantlab.ru/search-awards?q=Asimov&page=1&onlymatches=1) - поиск по фразе "Asimov"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        award_id: Int,            # id премии
        country: String,           # страна вручения
        country_id: Int,           # id страны вручения
        description: String,       # описание
        doc: Int,                  # ?
        lang_id: Int,              # id языка премии
        name: String,              # название
        notes: String,             # примечания
        rusname: String,           # русскоязычное название
        type: Int,                 # ?
        weight: Int,               # степень релевантности
        year_close: Int,           # год окончания вручения премии
        year_open: Int             # год старта вручения премии
    },
    ...
]
```

## Поиск персон

Запрос
```
GET /search-persons?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-persons?q=Лавкрафт&page=1&onlymatches=1](https://api.fantlab.ru/search-persons?q=Лавкрафт&page=1&onlymatches=1) - поиск по фразе "Лавкрафт"


Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        allnames: String,      # список имен (русскоязычное, оригинальное и т.д.)
        biography: String,     # биография
        birthyear: Int,        # год рождения
        country: String,       # страна
        country_id: Int,       # id страны
        deathyear: Int,        # год смерти
        doc: Int,              # ?
        name: String,          # русскоязычное имя
        person_id: Int,        # id персоны
        type: String,          # тип персоны (итоговая ссылка для перехода на страницу персоны на сайте: https://fantlab.ru/{type}{person_id})
        weight: Int            # степень релевантности
    },
    ...
]
```


## Поиск издательств

Запрос
```
GET /search-publishers?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-publishers?q=Москва&page=1&onlymatches=1](https://api.fantlab.ru/search-publishers?q=Москва&page=1&onlymatches=1) - поиск по фразе "Москва"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        alt_names: String,      # список альтернативных названий издтельства
        city: String,           # город
        comment: String,        # комментарий
        country: String,        # страна
        country_id: Int,        # id страны
        description: String,    # описание (могут присутствовать bb-теги)
        doc: Int,               # ?
        name: String,           # название
        notes: String,          # заметки
        publisher_id: Int,      # id издательства
        weight: Int,            # степень релевантности
        year_close: Int,        # год закрытия
        year_open: Int          # год открытия
    },
    ...
]
```


## Поиск фильмов

Запрос
```
GET /search-films?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-films?q=Лавкрафт&page=1&onlymatches=1](/search-films?q=Лавкрафт&page=1&onlymatches=1) - поиск по фразе "Лавкрафт"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        actors: String,          # список актеров
        country: String,         # список стран производства
        description: String,     # описание
        director: String,        # список режиссеров
        doc: Int,                # ?
        film_id: Int,            # id фильма
        name: String,            # оригинальное название
        rusname: String,         # русскоязычное название
        screenwriter: String,    # список сценаристов
        tagline: String,         # слоган
        weight: Int,             # степень релевантности
        year: Int                # год
    },
    ...
]
```


## Поиск статей

Запрос
```
GET /search-articles?q={query}&page={page}&onlymatches={0|1}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```

Пример

> [/search-articles?q=Лавкрафт&page=1&onlymatches=1](/search-articles?q=Лавкрафт&page=1&onlymatches=1) - поиск по фразе "Лавкрафт"

Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        article_id: Int,     # id статьи
        autor: String,       # автор
        comment: String,     # комментарий
        doc: Int,            # ?
        name: String,        # название
        source: String,      # источник статьи
        text: String,        # текст
        weight: Int,         # степень релевантности
        year: Int            # год публикации
    },
    ...
]
```

## Общий поиск

Запрос
```
GET /searchmain?q={query}
```

Параметры
```
query - строка поиска. В качестве разделителя используется "+"
```

Пример

> [/searchmain?q=Лавкрафт](https://api.fantlab.ru/searchmain?q=Лавкрафт) - поиск по фразе "Лавкрафт"

Ответ (см. [общая структура ответа](#Общая-структура-ответа) и наборы полей для отдельных запросов)
```
[
    ...,   # список авторов
    ...,   # список произведений
    ...,   # список изданий
    ...,   # список серий
    ...,   # список премий
    ...,   # список персон
    ...,   # список издательств
    ...,   # список фильмов
    ...    # список статей
]
```
Примечания:
1. Запрос не поддерживает пагинацию
2. По каждой категории поиска выдается максимум 10 результатов, отсортированных по убыванию релевантности (*weight*)
3. Параметр **onlymatches=1** не работает (кроме случая, когда query - ISBN, тогда выдается просто массив результатов)
