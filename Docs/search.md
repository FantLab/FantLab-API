# Поиск
Общие замечания по выдаче:
1. Все запросы, кроме общего поиска, поддерживают пагинацию. На одной странице выдается по 25 результатов, максимально возможное общее количество результатов равно 1000.
2. Во всех ответах сервера есть поле *attrs*, содержащее список полей в выдаче *matches*. Каждое поле обозначено именем и целочисленным типом. Например, "1" - Int, "7" - String.
## Общая структура ответа
```
{
    attrs: {              # список полей в выдаче
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
    total_found: Int,     # ?
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
/GET /search-autors?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-autors?q=Asimov&page=1&onlymatches=1 - поиск по фразе "Asimov"
```
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
        is_opened: Int,          # открыта ли страница автора (1 - да, 0 - нет)
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
Запрос
```
/GET /search-works?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-works?q=Asimov&page=1&onlymatches=1 - поиск по фразе "Asimov"
```
Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        all_autor_name: String,        # список всех авторов в оригинале
        all_autor_rusname: String,     # список всех авторов на русском языке (без bb-тегов)
        altname: String,               # ?
        autor1_id: Int,                # id 1-го автора
        autor1_is_opened: Int,         # открыта ли страница 1-го автора
        autor1_rusname: String,        # русскоязычное имя 1-го автора
        autor2_id: Int,                # id 2-го автора
        autor2_is_opened: Int,         # открыта ли страница 2-го автора
        autor2_rusname: String,        # русскоязычное имя 2-го автора
        autor3_id: Int,                # id 3-го автора
        autor3_is_opened: Int,         # открыта ли страница 3-го автора
        autor3_rusname: String,        # русскоязычное имя 3-го автора
        autor4_id: Int,                # id 4-го автора
        autor4_is_opened: Int,         # открыта ли страница 4-го автора
        autor4_rusname: String,        # русскоязычное имя 4-го автора
        autor5_id: Int,                # id 5-го автора
        autor5_is_opened: Int,         # открыта ли страница 5-го автора
        autor5_rusname: String,        # русскоязычное имя 5-го автора
        autor_id: Int,                 # ?
        autor_is_opened: Int,          # ?
        autor_rusname: String,         # ?
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
/GET /search-editions?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-editions?q=Asimov&page=1&onlymatches=1 - поиск по фразе "Asimov"
```
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
## Поиск премий
Запрос
```
/GET /search-awards?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-awards?q=Asimov&page=1&onlymatches=1 - поиск по фразе "Asimov"
```
Ответ (при запросе с параметром **onlymatches=1**)
```
[
    {
        awards_id: Int,            # id премии
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
## Поиск фильмов
Запрос
```
/GET /search-films?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-films?q=Лавкрафт&page=1&onlymatches=1 - поиск по фразе "Лавкрафт"
```
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
/GET /search-articles?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-articles?q=Лавкрафт&page=1&onlymatches=1 - поиск по фразе "Лавкрафт"
```
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
## Поиск книжных серий
Запрос
```
/GET /search-series?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-series?q=Лавкрафт&page=1&onlymatches=1 - поиск по фразе "Лавкрафт"
```
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
## Поиск персон
Запрос
```
/GET /search-persons?q={query}&page={page}&onlymatches={0|1}
```
Параметры
```
query - строка поиска. В качестве разделителя используется "+"
page - номер страницы (необязательный; по-умолчанию 1)
onlymatches - выдавать только содержимое массива matches (необязательный; по-умолчанию 0)
```
Пример
```
/GET /search-persons?q=Лавкрафт&page=1&onlymatches=1 - поиск по фразе "Лавкрафт"
```
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
