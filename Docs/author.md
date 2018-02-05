# Автор
Запрос
```
GET /autor/{id}?biography={0/1}&awards={0/1}&la_resume={0/1}&cycles_blocks={0/1}&works_blocks={0/1}
```
Параметры
```
id - id автора,
biography - выводить ли блок биографии (необязательный),
awards - выводить ли блок наград (необязательный),
la_resume - выводить ли блок лингвоанализа (необязательный),
cycles_blocks - выводить ли блок с циклами (необязательный),
works_blocks - выводить ли блок отдельных произведений (необязательный)
```
Пример
```
GET /autor/133?biography=1&awards=1&la_resume=1&cycles_blocks=1&works_blocks=1 - Джордж Р.Р. Мартин
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
    works: [ |null
    ],
    works_blocks: [ |null
    ]
}
```
