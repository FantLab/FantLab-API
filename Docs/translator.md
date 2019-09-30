# Переводчик

## Основная информация
Запрос
```
GET /translator/{id}
```
Параметры
```
id - id переводчика
```

Пример:
> [/translator/1](https://api.fantlab.ru/translator/1) - переводчик Ирина Гавриловна Гурова

Ответ:
```
{
    age: Int,        # возраст на данный момент или на момент смерти
    articles: [      # список статей, на данный момент всегда пустой
        ...: ?|null
    ],
    anons: String,                # биография с ограничением в 500 символов (ака превью)
    biography_notes: String,      # примечание к биографии
    biography_source: String,     # источник биографии
    birthday: Date,               # дата рождения
    countries: [                  # список стран
        {
            country_id: Int,      # id страны
            name: String          # страна
        },
        ...
    ],
    curator_id: Int,                 # id куратора
    curator_name: String,            # логин куратора
    deathday: Date,                  # дата смерти
    has_advanced_awards: Boolean,    # ?
    id: Int,                         # id переводчика
    image: String,                   # ссылка на картинку произведения (обложку по умолчанию)
    image_preview: String,           # ссылка на превью картинки произведения
    name: String,                    # имя / фамилия
    name_orig: String,               # имя / фамилия в оригинале (для иностранных переодчиков)
    name_rp: String,                 # имя / фамилия в родительном падеже
    name_short: String,              # имя (инициалы) / фамилия
    name_short_rp: String,           # имя (инициалы) / фамилия в родительном падеже
    name_sort: String,               # полное ФИО для сортировки
    notes: String,                   # ?
    person_type: String,             # тип персоны (в данном случае всегда translator)
    photo: ?,                        # ?
    pseudo_id: Int,                  # id псевдонима
    pseudo_name: ?|null,             # ?
    sex: Int,                        # пол (1 - мужской, 2 - женский)
    show_in_list: Boolean,           # ?
    translated_from: String,         # "переводчик с ..."
    translated_to: String            # "переводчик на ..."
}
```

## Расширенная информация

Запрос
```
GET /translator/{id}/extended?sort={year|autor|type}
```
Параметры
```
id - id переводчика
sort - вариант сортировки переведенных произведений (необязательный; по-умолчанию year)
    * year  - по году издания перевода
    * autor - по автору произведения
    * type  - по типу произведения
```
Пример:
> [/translator/1](https://api.fantlab.ru/translator/1) - переводчик Ирина Гавриловна Гурова

Ответ:
```
В выдачу к основной информации добавляются следующие поля:

    biography: String,            # полная биография
    awards: [
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
    sorted_works: [                  # список id переведенных произведений, отсортированных в соответствии с переданным типом
        ...: String
    ],
    translated: [                    # список переведенных произведений
        {translated_work_string_id}: {
            author: {                # карточка автора произведения
            ...
            },
            editions: [              # список изданий
                {                    # карточка издания
                ...
                },
                ...
            ],
            work:  {                # карточка произведения
            ...
            },
        },
        ...
    ],

```
В выдаче переведенных произведений карточки произведения, издания и автора сделаны в формате [миникарточек](search-ids.md)


## Список наград отдельно
Запрос
```
GET /translator/{id}/awards?sort={byorder|byaward|bychrono}
```
Параметры
```
id - id переводчика
sort - вариант сортировки наград (необязательный; по-умолчанию bychrono)
    * byorder - по порядку
    * byaward - по премиям
    * bychrono - по хронологии
```
Пример
```
GET /translator/1/awards?sort=byorder - награды И.Г. Гуровой по порядку
```
Ответ
```
[
    {
        ...          # те же самые поля, что и в translator -> awards
    }
]
```
**Nota bene**:

список отдается без разбиения по категориям

## Полная биография отдельно
Запрос
```
GET /translator/{id}/biography
```
Параметры
```
id - id переводчика
```
Пример
```
GET /translator/1/biography - полная биография И.Г. Гуровой
```
Ответ
```
Те же самые поля, что и в выдаче основной информации + поле biography из расширенной
```

## Библиография переведенных произведений отдельно
Запрос
```
GET /translator/{id}/translated?sort={year|autor|type}
```
Параметры
```
id - id переводчика
sort - вариант сортировки переведенных произведений (необязательный; по-умолчанию year)
    * year  - по году издания перевода
    * autor - по автору произведения
    * type  - по типу произведения
```
Пример
```
GET /translator/1/translated - список переведенных произведений И.Г. Гуровой
```
Ответ
```
Те же самые поля, что и в выдаче основной информации + поля sorted_works и translated из расширенной
```
