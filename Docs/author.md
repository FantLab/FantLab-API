# Автор
## Основная информация
Запрос
```
GET /autor/{id}?biography={0|1}&awards={0|1}&la_resume={0|1}&biblio_blocks={0|1}&sort={year|rating|markcount|rusname|name|writeyear}
```
Параметры
```
id - id автора
biography - выводить ли блок биографии (необязательный; по-умолчанию 0)
awards - выводить ли блок наград (необязательный; по-умолчанию 0)
la_resume - выводить ли блок лингвоанализа (необязательный; по-умолчанию 0)
biblio_blocks - выводить ли блоки циклов и отдельных произведений (необязательный; по-умолчанию 0)
sort - вариант сортировки библиографии (необязательный; по-умолчанию year)
```
Пример
```
GET /autor/133?biography=1&awards=1&la_resume=1&biblio_blocks=1&sort=rating - Джордж Р.Р. Мартин
```
Альтернативный запрос для получения всей информации
```
GET /autor/{id}/extended
```
Ответ
```
{
    anons: String,                    # краткий анонс биографии
    autor_id: Int,                    # id автора
    awards: { |null                   # [awards] список наград
        nom: [
            {
                award_icon: String,             # ссылка на логотип премии
                award_id: Int,                  # id премии
                award_in_list: Int,             # ?
                award_is_opened: Int,           # открыта ли страница премии (1 - да, 0 - нет)
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
    biography: String|null,           # [biography] биография
    biography_notes: String|null,     # [any] примечания к биографии
    birthday: Date|null,              # дата рождения (в формате YYYY-MM-DD)
    compiler: String|null,            # [any] составитель библиографии
    country_id: Int|null,             # id страны
    country_name: String,             # название страны
    curator: Int|null,                # id куратора библиографии
    cycles: [ |null
    ],
    cycles_blocks: [ |null
    ],
    deathday: Date|null,              # дата смерти (в формате YYYY-MM-DD)
    fantastic: Int,                   # ?
    fl_blog_anons: String|null,       # ?
    image: String,                    # ссылка на основное фото автора
    image_preview: String,            # ссылка на превью основного фото автора
    is_opened: Int,                   # открыта ли страница автора (1 - да, 0 - нет)
    la_resume: [ |null                # [la_resume] лингвоанализ
        ...: String,                  # отдельный пункт анализа
        ...
    ],
    last_modified: Date,              # дата последнего редактирования (в формате YYYY-MM-DD HH:mm:SS)
    name: String,                     # имя на русском языке
    name_orig: String,                # имя в оригинале
    name_pseudonyms: [                # список псевдонимов
        ...: String,
        ...
    ],
    name_rp: String,                  # имя на русском языке в родительном падеже
    name_short: String,               # имя на русском языке для перечислений (сначала фамилия, затем имя)
    registered_user_id: Int|null,         # id автора как пользователя Fantlab'а (если зарегистрирован)
    registered_user_login: String|null,   # логин автора как пользователя Fantlab'а,
    registered_user_sex: String|null,     # пол автора как пользователя Fantlab'а ("m" - мужской, "f" - женский)
    sex: String,                      # пол ("m" - мужской, "f" - женский)
    sites: [ |null                    # [any] сайты автора
        {
            descr: String,            # описание ссылки
            site: String              # ссылка
        },
        ...
    ],
    source: String|null,              # [any] описание источника биографии
    source_link: String|null,         # [any] ссылка на источник биографии
    stat: {                           # статистика
        awardcount: Int|null,         # [any] количество наград
        editioncount: Int,            # количество изданий
        markcount: Int,               # количество поставленных автору оценок
        moviecount: Int,              # количество фильмов (экранизаций и т.д.)
        responsecount: Int,           # количество написанных на произведения автора отзывов
        workcount: Int|null           # [any] количество произведений
    },
    works: ?|null,
    works_blocks: { |null             # [biblio_blocks] блок отдельных произведений
        {block_id}: {                 # id блока (см. константы)
            id: Int,                  # id блока (совпадает с block_id)
            list: [
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
                    public_download_file: Int,                 # доступен ли файл с текстом для скачивания (1 - да, 0 - нет)
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
                    work_notfinished: Int,              # произведение не закончено (1 - да, 0 - нет)
                    work_preparing: Int,                # произведение в планах (1 - да, 0 - нет)
                    work_published: Int,                # произведение опубликовано (1 - да, 0 - нет)
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
## Список отзывов на произведения
Запрос
```
GET /autor/{id}/responses?page={page}&sort={date|rating|mark}
```
Параметры
```
id - id автора
page - страница (необязательный; по-умолчанию 1). Запрос с пагинацией, отдает по 50 отзывов на странице
sort - вариант сортировки (необязательный; по-умолчанию date)
```
Пример
```
GET /autor/133/responses?page=2&sort=rating - 50-100 отзывы (по убыванию рейтинга) на произведения Дж. Р.Р. Мартина
```
Ответ
```
[
    {
        name: String,             # название произведения в оригинале
        posted_date: Date,        # дата публикации (в формате YYYY-MM-DD HH:mm:SS)
        response_id: Int,         # id отзыва
        response_text: String,    # текст отзыва
        rusname: String,          # русскоязычное называние произведения
        user_id: Int,             # id пользователя Fantlab'а - автора отзыва
        user_name: String,        # логин пользователя Fantlab'а - автора отзыва
        user_sex: Int,            # пол пользователя Fantlab'а - автора отзыва (1 - мужской, 0 - женский)
        user_voted: ?|null,       # ?
        vote_minus: Int,          # количество голосов "Против"
        vote_plus: Int,           # количество голосов "За"
        work_id: Int,             # id произведения
        work_type_id: Int         # id типа произведения
    },
    ...
]
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
                    correct_level: Float,   # уровень проверенности издания (0 - не проверено, 0.5 - проверено не полностью, 1 - не проверено)
                    cover_type: Int,        # id типа обложки
                    ebook: Int,             # электронная книга или нет (1 - да, 0 - нет)
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
## Список наград отдельно
Запрос
```
GET /autor/{id}/awards?sort={byorder|byaward|bychrono|bywork}
```
Параметры
```
id - id автора
sort - вариант сортировки наград (необязательный; по-умолчанию bychrono)
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
**Nota bene**: список отдается без разбиения по категориям
## Подписка на оповещения о новых произведениях или изданиях
Запрос
```
GET https://fantlab.ru/notification/subscribe/autors/{id}/{works|editions}.json
```
Параметры
```
id - id автора
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
GET https://fantlab.ru/notification/subscribe/autors/133/works.json - подписка на оповещения о новых произведениях Дж. Р.Р. Мартина
```
Ответ
```
{
    id: Int,         # id подписки
    status: Int      # статус операции (1 - успех, 0 - провал)
}
```
## Отмена конкретной подписки
Запрос
```
GET https://fantlab.ru/notification/unsubscribe/{id}.json
```
Параметры
```
id - id подписки
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
GET https://fantlab.ru/notification/unsubscribe/1.json - отписка от оповещений по подписке №1
```
Ответ
```
{
    status: Int      # статус операции (1 - успех, 0 - провал)
}
```
