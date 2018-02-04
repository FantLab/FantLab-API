# Fantlab-API

Публичный гитхаб-репозиторий по API для библиобазы fantlab.ru

В настоящий момент используется для информирования о процессе работы над API сайта через тикет-систему Issues и для взаимодействия API фантлаба и работающих с ним программистов самого ФЛ и использующих его  клиентами и веб-сервисами.

Обновляемое руководство по API сайта - https://goo.gl/CwQfmb  
(обратите внимание, что до релиза первой версии API возможно большие изменения в API-выдачах)

Предложения и замечания по работе API пишите в Issues данного репозитория.

**Домен для запросов API - https://api.fantlab.ru**

## Содержание
1. [Список открытых авторов](#Список-открытых-авторов)
2. [Автор](#Автор)
3. [Произведение](#Произведение)

## Список открытых авторов
Запрос:
```
GET /autors{X}
```
Параметры:
```
X - код заглавной буквы (в кодировке Windows-1251) фамилии автора на русском языке
```
Пример:
```
GET /autors192 - список всех открытых авторов на букву "A"
```
Альтернативный запрос для получения всего списка:
```
GET /autorsall
```
Ответ:
```
{
    list:[
        {
            autor_id: Int,         # id автора
            is_fv: Int,            # является ли автор фантастоведом (1 - да, 0 - нет)
            name: String,          # имя на русском языке
            name_orig: String,     # имя на языке оригинала
            name_rp: String,       # имя на русском языке в родительном падеже
            name_short: String,    # имя на русском языке для перечислений (сначала фамилия, затем имя)
            type: String           # тип автора (в данном случае всегда autor)
        },
        ...
    ],
    liter: String,                 # по какой заглавной букве фамилии ищем
    liter_code: Int|null,          # код заглавной буквы фамилии (в кодировке Windows-1251)
    liter_list: String             # список ссылок на побуквенные списки авторов
}
```

## Автор
Запрос:
```
GET /autor/{id}?biography={0/1}&awards={0/1}&la_resume={0/1}&cycles_blocks={0/1}&works_blocks={0/1}
```
Параметры (все, кроме *id*, опциональны):
```
id - id автора,
biography - выводить ли блок биографии,
awards - выводить ли блок наград,
la_resume - выводить ли блок лингвоанализа,
cycles_blocks - выводить ли блок с циклами,
works_blocks - выводить ли блок отдельных произведений
```
Пример:
```
GET /autor/133?biography=1&awards=1&la_resume=1&cycles_blocks=1&works_blocks=1 - Джордж Р.Р. Мартин
```
Альтернативный запрос для получения всей информации:
```
GET /autor/{id}/extended
```
Ответ:
```
{
    anons: String,                    # краткий анонс биографии
    autor_id: Int,                    # id автора
    awards: { |null                   # список наград (при запросе с ключом awards=1)
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
    biography: String|null,           # биография (при запросе с ключом biography=1)
    biography_notes: String|null,     # примечания к биографии (при запросе с любыми ключами)
    birthday: Date|null,              # дата рождения (в формате YYYY-MM-DD)
    compiler: String|null,            # составитель библиографии (при запросе с любыми ключами)
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
    la_resume: ?|null,
    last_modified: Date,              # дата последнего редактирования (в формате YYYY-MM-DD HH:mm:SS)
    name: String,                     # имя на русском языке
    name_orig: String,                # имя в оригинале
    name_pseudonyms: [                # список псевдонимов
        ...: String,
    ],
    name_rp: String,                  # имя на русском языке в родительном падеже
    name_short: String,               # имя на русском языке для перечислений (сначала фамилия, затем имя)
    registered_user_id: Int|null,         # id автора как пользователя Fantlab'а (если зарегистрирован)
    registered_user_login: String|null,   # логин автора как пользователя Fantlab'а,
    registered_user_sex: String|null,     # пол автора как пользователя Fantlab'а ("m" - мужской, "f" - женский)
    sex: String,                      # пол ("m" - мужской, "f" - женский)
    sites: [ |null                    # сайты автора (при запросе с любыми ключами)
        {
            descr: String,            # описание ссылки
            site: String              # ссылка
        },
        ...
    ],
    source: String|null,              # описание источника биографии (при запросе с любыми ключами)
    source_link: String|null,         # ссылка на источник биографии (при запросе с любыми ключами)
    stat: {                           # статистика
        awardcount: Int|null,         # количество наград (при запросе с любыми ключами)
        editioncount: Int,            # количество изданий
        markcount: Int,               # количество поставленных автору оценок
        moviecount: Int,              # количество фильмов (экранизаций и т.д.)
        responsecount: Int,           # количество написанных на произведения автора отзывов
        workcount: Int|null           # количество произведений (при запросе с любыми ключами)
    },
    works: [ |null
    ],
    works_blocks: [ |null
    ]
}
```
## Произведение
To be continued...
