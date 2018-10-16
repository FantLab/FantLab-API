# Авторы

## Список авторов
Запрос
```
GET /autors{X}
```
Параметры
```
X - код заглавной буквы (в кодировке Windows-1251) фамилии автора на русском языке
```
Пример
```
GET /autors192 - список всех открытых авторов на букву "A"
```
Альтернативный запрос для получения всего списка
```
GET /autorsall
```
Ответ
```
{
    list:[
        {
            type: String           # тип сегмента (в данном случае всегда autor)
            id: Int,               # id автора
            url: Url               # ссылка на страницу автора
            autor_id: Int,         # id автора
            is_fv: Boolean,        # является ли автор фантастоведом
            name: String,          # имя на русском языке
            name_orig: String,     # имя на языке оригинала
            name_rp: String,       # имя на русском языке в родительном падеже
            name_short: String,    # имя на русском языке для перечислений (сначала фамилия, затем имя)
        },
        ...
    ],
    liter: String,                 # по какой заглавной букве фамилии ищем
    liter_code: Int|null,          # код заглавной буквы фамилии (в кодировке Windows-1251)
    liter_list: String             # список ссылок на побуквенные списки авторов
}
```
Замечания:
1. В списке только открытые авторы, параметра для получения списка всех авторов нет


# Автор

## Основная информация
```
GET /autor/{id}
```

Пример:
> [/autor/1](https://api.fantlab.ru/autor/1) - Дэн Симменс


Ответ:
```
{
    type: String                      # тип сегмента (в данном случае всегда autor)
    id: Int,                          # id автора
    autor_id: Int,                    # id автора (дубль, название переменной с типом)
    url: Url                          # ссылка на страницу автора
    last_modified: DateTime,          # дата последнего редактирования

    is_opened: Boolean,               # открыта ли страница автора
    anons: String,                    # краткий анонс биографии
    birthday: Date|null,              # дата рождения
    country_id: Int|null,             # id страны
    country_name: String,             # название страны
    deathday: Date|null,              # дата смерти
    fantastic: Int,                   # ?
    image: String,                    # ссылка на основное фото автора
    image_preview: String,            # ссылка на превью основного фото автора
    name: String,                     # имя на русском языке
    name_orig: String,                # имя в оригинале
    name_pseudonyms: [                # список псевдонимов
        ...: String|null,
        ...
    ],
    name_rp: String,                  # имя на русском языке в родительном падеже
    name_short: String,               # имя на русском языке для перечислений (сначала фамилия, затем имя)
    sex: String,                      # пол ("m" - мужской, "f" - женский)
    stat: {                           # статистика
        awardcount: Int|null,         # [any] количество наград
        editioncount: Int,            # количество изданий
        markcount: Int,               # количество поставленных автору оценок
        moviecount: Int,              # количество фильмов (экранизаций и т.д.)
        responsecount: Int,           # количество написанных на произведения автора отзывов
        workcount: Int|null           # [any] количество произведений
    },
    sites: [ |null                    # [any] сайты автора
        {
            descr: String,            # описание ссылки
            site: String              # ссылка
        },
        ...
    ],
}
```

## Расширенная информация
```
GET /autor/{id}/extended
```
в расширенной информации о произведении добавляются блоки:  
*biography - биография
awards - наград
la_resume - лингвоанализ
biblio_blocks - библиография (список произведений)
sort - вариант сортировки библиографии (необязательный)
    year - по году публикации
    rating - по рейтингу
    markcount - по количеству оценок
    rusname - по русскому названию
    name - по оригинальному названию
    writeyear - по году написания*

Пример:
> [/autor/1/extended](https://api.fantlab.ru/autor/1/extended) - Дэн Симменс

*Есть поддержка запроса с получением частичных данных - указанием параметров и сортировки:
> /autor/1?biography=1&awards=1&la_resume=1&biblio_blocks=1&sort=rating*

Ответ
```
{
    id: Int,                    # id автора
    ... все поля обычной карточки автора (см. выше) и добаляются новые: ...

    biography: String|null,           # [biography] биография
    biography_notes: String|null,     # [any] примечания к биографии
    source: String|null,              # [any] описание источника биографии
    source_link: String|null,         # [any] ссылка на источник биографии

    compiler: String|null,            # [any] составитель библиографии
    curator: Int|null,                # id куратора библиографии

    la_resume: [ |null                # [la_resume] лингвоанализ
        ...: String,                  # отдельный пункт анализа
        ...
    ],
    registered_user_id: Int|null,         # id автора как пользователя Fantlab'а (если зарегистрирован)
    registered_user_login: String|null,   # логин автора как пользователя Fantlab'а,

    awards: { |null                   # [awards] список наград
        nom: [
            {
                award_icon: String,             # ссылка на логотип премии
                award_id: Int,                  # id премии
                award_in_list: Int,             # ?
                award_is_opened: Boolean,       # открыта ли страница премии
                award_name: String,             # название премии
                award_rusname: String,          # русскоязычное название премии
                contest_id: Int,                # id конкурса
                contest_name: String,           # название конкурса (обычно сопадает с годом)
                contest_year: Int,              # год проведения конкурса
                cw_id: Int,                     # ?
                cw_is_winner: Int,              # ?
                cw_postfix: String,             # ?
                cw_prefix: String,              # ?
                nomination_id: Int|null,             # id номинации
                nomination_name: String|null,        # название номинации
                nomination_rusname: String|null,     # русскоязычное название номинации
                work_id: Int,                   # id награжденного произведения (0, если награда относится не к произведению)
                work_name: String,              # название награжденного произведения
                work_rusname: String,           # русскоязычное название награжденного произведения
                work_year: Int|null             # год публикации произведения
            },
            ...
        ],
        win: [
            {
                ...                             # те же самые поля
            },
            ...
        ]
    },

    cycles_blocks: { |null            # [biblio_blocks] блок циклов
        {block_id}: {                 # id блока (см. константы)
            id: Int,                  # id блока (совпадает с block_id)
            list: [                   # список циклов в блоке
                {
                    authors: [                   # список авторов
                        {
                            id: Int,             # id автора
                            name: String,        # русскоязычное имя автора
                            type: String         # тип автора (autor|art)        
                        },
                        ...
                    ],
                    children: [                  # список произведений, входящих в цикл
                        {
                            authors: [                   # список авторов
                                {
                                    id: Int,             # id автора
                                    name: String,        # русскоязычное имя автора
                                    type: String         # тип автора (autor|art)        
                                },
                                ...
                            ],
                            deep: Int,                                 # глубина вложенности
                            plus: ?|null,                              # доп. произведение [с "плюсом"] или нет
                            public_download_file: Boolean,             # доступен ли файл с текстом для скачивания
                            publish_status: String,                    # статус публикации
                            val_midmark: Float|null,                   # простое среднее по оценкам (эта цифра видна только в Рейтинг -> Подробнее)
                            val_midmark_by_weight: Float|null,         # рейтинг произведения
                            val_midmark_rating: Float|null,            # ?
                            val_rating: Float|null,                    # ? (совпадает с val_midmark_rating)
                            val_responsecount: Int|null,               # количество отзывов на произведение
                            val_voters: Int|null,                      # количество голосов в рейтинге
                            work_id: Int,                       # id произведения
                            work_lp: Int,                       # ?
                            work_name: String,                  # русскоязычное название произведения
                            work_name_alt: String,              # альтернативные названия
                            work_name_bonus: String,            # ?
                            work_name_orig: String,             # название произведения в оригинале
                            work_notfinished: Boolean,          # произведение не закончено?
                            work_preparing: Boolean,            # произведение в планах?
                            work_published: Boolean,            # произведение опубликовано?
                            work_root_saga: [                   # список "родительских" произведений
                                {
                                    prefix: String,             # степень отношения произведения к "родителю" (например, "входит в ")
                                    work_id: Int,               # id произведения
                                    work_name: String,          # русскоязычное название произведения
                                    work_type: String,          # тип произведения
                                    work_type_id: Int,          # id типа произведения
                                    work_type_in: String        # ?
                                },
                                ...
                            ],
                            work_type: String,                  # тип произведения
                            work_type_icon: String,             # иконку какого типа произведений показывать
                            work_type_id: Int,                  # id типа произведения
                            work_type_name: String,             # название типа произведения
                            work_year: ?|null,                  # год публикации
                            work_year_of_write: ?|null          # год написания
                        },
                        ...
                    ]
                    position_index: Int|null,           # ?
                    position_is_node: Int,              # ?
                    position_level: Int,                # ?
                    position_show_in_biblio: Int,              # ?
                    position_show_subworks_in_biblio: Int,     # ?
                    public_download_file: Boolean,             # доступен ли файл с текстом для скачивания
                    publish_status: String,                    # статус публикации
                    val_midmark: Float|null,                   # простое среднее по оценкам (эта цифра видна только в Рейтинг -> Подробнее)
                    val_midmark_by_weight: Float|null,         # рейтинг произведения
                    val_midmark_rating: Float|null,            # ?
                    val_rating: Float|null,                    # ? (совпадает с val_midmark_rating)
                    val_responsecount: Int|null,               # количество отзывов на произведение
                    val_voters: Int|null,                      # количество голосов в рейтинге
                    work_id: Int,                       # id произведения
                    work_lp: Int,                       # ?
                    work_name: String,                  # русскоязычное название произведения
                    work_name_alt: String,              # альтернативные названия
                    work_name_bonus: String,            # ?
                    work_name_orig: String,             # название произведения в оригинале
                    work_notfinished: Boolean,          # произведение не закончено?
                    work_preparing: Boolean,            # произведение в планах?
                    work_published: Boolean,            # произведение опубликовано?
                    work_root_saga: null,               # список "родительских" произведений
                    work_type: String,                  # тип произведения
                    work_type_icon: String,             # иконку какого типа произведений показывать
                    work_type_id: Int,                  # id типа произведения
                    work_type_name: String,             # название типа произведения
                    work_year: ?|null,                  # год публикации
                    work_year_of_write: ?|null          # год написания
                },
                ...
            ],
            name: String,             # категория блока
            title: String             # русскоязычное название блока
        },
        ...
    },
    works_blocks: { |null             # [biblio_blocks] блок отдельных произведений
        {block_id}: {                 # id блока (см. константы)
            id: Int,                  # id блока (совпадает с block_id)
            list: [                   # список произведений в блоке
                {
                    authors: [                   # список авторов
                        {
                            id: Int,             # id автора
                            name: String,        # русскоязычное имя автора
                            type: String         # тип автора (autor|art)        
                        },
                        ...
                    ],
                    position_index: Int|null,           # ?
                    position_is_node: Int,              # ?
                    position_level: Int,                # ?
                    position_show_in_biblio: Int,              # ?
                    position_show_subworks_in_biblio: Int,     # ?
                    public_download_file: Boolean,             # доступен ли файл с текстом для скачивания
                    publish_status: String,                    # статус публикации
                    val_midmark: Float|null,                   # простое среднее по оценкам (эта цифра видна только в Рейтинг -> Подробнее)
                    val_midmark_by_weight: Float|null,         # рейтинг произведения
                    val_midmark_rating: Float|null,            # ?
                    val_rating: Float|null,                    # ? (совпадает с val_midmark_rating)
                    val_responsecount: Int|null,               # количество отзывов на произведение
                    val_voters: Int|null,                      # количество голосов в рейтинге
                    work_id: Int,                       # id произведения
                    work_lp: Int,                       # ?
                    work_name: String,                  # русскоязычное название произведения
                    work_name_alt: String,              # альтернативные названия
                    work_name_bonus: String,            # ?
                    work_name_orig: String,             # название произведения в оригинале
                    work_notfinished: Boolean,          # произведение не закончено?
                    work_preparing: Boolean,            # произведение в планах?
                    work_published: Boolean,            # произведение опубликовано?
                    work_root_saga: [                   # список "родительских" произведений
                        {
                            prefix: String,             # степень отношения произведения к "родителю" (например, "входит в ")
                            work_id: Int,               # id произведения
                            work_name: String,          # русскоязычное название произведения
                            work_type: String,          # тип произведения
                            work_type_id: Int,          # id типа произведения
                            work_type_in: String        # ?
                        },
                        ...
                    ],
                    work_type: String,                  # тип произведения
                    work_type_icon: String,             # иконку какого типа произведений показывать
                    work_type_id: Int,                  # id типа произведения
                    work_type_name: String,             # название типа произведения
                    work_year: ?|null,                  # год публикации
                    work_year_of_write: ?|null          # год написания
                },
                ...
            ],
            name: String,             # категория блока
            title: String             # русскоязычное название блока
        },
        ...
    }
}
```


## Список изданий
Запрос
```
GET /autor/{id}/alleditions?editions_blocks={0|1}
```
Параметры
```
id - id автора
editions_blocks - выводить ли сам список изданий (необязательный; по-умолчанию 0)
```
Пример
```
GET /autor/133/alleditions?editions_blocks=1 - список всех изданий Джорджа Р.Р. Мартина
```
Ответ
```
{
    editions_blocks: { |null   # [editions_blocks] список изданий
        {id}: {                # id блока (список практически совпадает с editions_info -> booktype -> type)
            block: String,     # категория блока
            id: Int,           # id блока
            list: [            # список изданий в данном блоке
                {
                    autors: String,         # авторы
                    compilers: String,      # составители
                    correct_level: Float,   # уровень проверенности издания (0 - не проверено, 0.5 - проверено не полностью, 1 - проверено)
                    cover_type: Int,        # id типа обложки
                    ebook: Boolean,         # электронная книга или нет
                    edition_id: Int,        # id издания
                    fiction: Int,           # ?
                    lang: String,           # русскоязычное название языка
                    lang_code: String,      # код языка
                    lang_id: Int,           # id языка
                    name: String,           # название издания
                    pic_num: Int,           # ?
                    plandate: ?|null,       # ?
                    plandate_txt: String,   # пояснение насчет планируемой даты
                    translators: Int|null,  # id переводчика
                    type: Int,              # id типа издания
                    year: Int               # год издания
                },
                ...
            ],
            name: String,      # название блока
            title: String      # русскоязычное название блока
        },
        ...
    },
    editions_info: {           # сводная информация об изданиях
        all: Int,              # количество всех изданий
        booktype: {            # статистика по типу издания
            {type}: { |null            # тип издания (см. константы)
                count: Int,            # количество изданий данного типа
                id: Int,               # id типа издания (равен type)
                name: String,          # название типа
                title: String          # русскоязычное название типа
            },
            ...
        },
        langs: {               # статистика по языку издания
            {lang}: { |null            # язык издания (см. константы)
                count: Int,            # количество изданий на данном языке
                lang_code: String,     # код языка
                lang_id: Int,          # id языка (равен lang)
                lang_name: String      # русскоязычное название языка
            },
            ...
        },
        translators: [         # статистика по переводчикам
            {
                count: Int,            # количество изданий с данным переводчиком
                id: Int,               # id переводчика
                name: String           # имя переводчика
            }
        ]
    },
    stat: {
        editioncount: Int      # количество всех изданий
    },
    title: String              # ссылка на страницу автора (используется на вебе)
}
```


## Список отзывов на произведения
см. [Docs/responses.md#Отзывы на все произведения автора](../Docs/responses.md#Отзывы-на-все-произведения-автора)


## Список наград отдельно
Запрос
```
GET /autor/{id}/awards?sort={byorder|byaward|bychrono|bywork}
```
Параметры
```
id - id автора
sort - вариант сортировки наград (необязательный; по-умолчанию bychrono)
    * byorder - по порядку
    * byaward - по премиям
    * bychrono - по хронологии
    * bywork - по произведениям
```
Пример
```
GET /autor/133/awards?sort=bywork - награды Дж. Р.Р. Мартина, осортированные по произведениям
```
Ответ
```
[
    {
        ...          # те же самые поля, что и в autor -> awards
    }
]
```
**Nota bene**

список отдается без разбиения по категориям
