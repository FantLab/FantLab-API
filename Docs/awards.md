# Премии
## Список премий
Запрос
```
/GET /awards?nonfant={0|1}&sort={name|country|type|lang}
```
Параметры
```
nonfant - показывать ли нефантастические премии (необязательный; по-умолчанию 0)
sort - вариант сортировки (необязательный; по-умолчанию lang)
```
Пример
```
/GET /awards?nonfant=1&sort=country - вывести список всех премий, отсортированный по странам
```
Ответ
```
[
    {
        award_close: Boolean,       # закрыта ли премия
        award_id: Int,              # id премии
        award_type: Int,            # ?
        contests_count: Int,        # количество конкурсов
        country_id: Int,            # id страны
        country_name: String,       # страна
        description_length: Int,    # длина описания (в символах)
        homepage: String,           # ссылка на официальный сайт премии
        is_opened: Boolean,         # открыта ли страница премии
        lang_id: Int,               # id языка премии
        lang_name: String,          # язык премии
        max_date: Date,             # дата последнего на данный момент конкурса (в формате YYYY-MM-DD)
        min_date: Date,             # дата первого конкурса (в формате YYYY-MM-DD)
        name: String,               # название в оригинале
        nomi_count: Int,            # количество номинаций
        non_fantastic: Boolean,     # нефантастическая премия?
        rusname: String,            # русскоязычное название
        show_in_list: Boolean       # ?
    },
    ...
]
```
Замечания:
1. Разделения по категориям в выдаче нет. То есть, скажем, при сортировке по языку в выдаче нет категорий "английский язык", "русский язык" и пр., все премии перечислены одним списком
## Премия
Запрос
```
/GET /award/{id}?include_nomi={0|1}&include_contests={0|1}&sort={contest|nomi}
```
Параметры
```
id - id премии
include_nomi - показывать ли информацию о номинациях (необязательный; по-умолчанию 0)
include_contests - показывать ли информацию о конкурсах (необязательный; по-умолчанию 0)
sort - вариант сортировки (необязательный; по-умолчанию contest)
```
Пример
```
/GET /award/2?include_nomi=1&include_contests=1&sort=nomi - выдать всю информацию о Хьюго с сорировкой результатов по номинациям
```
Ответ
```
{
    award_close: Boolean,      # закрыта ли премия
    award_id: Int,             # id премии
    award_type: Int,           # ?
    comment: String,           # комментарий
    compiler: String,          # ?
    contests: [ |null          # [include_contests] список конкурсов (см. ниже)
        ...
    ],
    copyright: String,         # ?
    copyright_link: String,    # ?
    country_id: Int,           # id страны
    country_name: String,      # страна
    curator_id: Int,           # id куратора премии
    description: String,       # описание (могут присутствовать bb-теги)
    homepage: String,          # ссылка на официальный сайт премии
    is_opened: Boolean,        # открыта ли страница премии
    lang_id: Int,              # id языка
    max_date: Date,            # дата последнего на данный момент конкурса (в формате YYYY-MM-DD)
    min_date: Date,            # дата первого конкурса (в формате YYYY-MM-DD)
    name: String,              # название в оригинале
    nominations: [ |null       # [include_nomi] список номинаций
        {
            book_count: Int,              # количество номинированных позиций за все время существования премии
            description: String,          # описание
            description_length: Int,      # длина описания (в символах)
            name: String,                 # название номинации в оригинале
            nomination_id: Int,           # id номинации
            number: Int,                  # порядковый номер номинации
            rusname: String               # русскоязычное название номинации
        },
        ...
    ],
    non_fantastic: Boolean,    # нефантастическая премия?
    notes: String,             # примечания
    process_status: String,    # ?
    rusname: String,           # русскоязычное название
    show_in_list: Boolean      # ?
}
```
Выдача массива *contests* зависит от варианта сортировки. При **sort=contest** выдача полностью совпадает с выдачей [конкурса](contest.md#Конкурс). При **sort=nomi** выдача выглядит следующим образом:
```
{
    award_id: Int,        # id премии
    contest_works: [      # список номинантов
        ...               # выдача номинанта совпадает с contest -> contest_works (см. Конкурс)
    ],
    name: String,         # название номинации в оригинале
    nomination_id: Int,   # id номинации
    rusname: String       # русскоязычное название номинации
}
```
## Конкурс
Запрос
```
GET /contest/{id}?include_works={0|1}
```
Параметры
```
id - id конкурса
include_works - выводить ли номинантов (необязательный; по-умолчанию 0)
```
Пример
```
GET /contest/7955?include_works=1 - Хьюго 2017г. с номинантами
```
Ответ
```
{
    award_id: Int,                  # id премии
    award_name: String,             # название премии в оригинале
    award_rusname: String,          # русскоязычное название премии
    contest_id: Int,                # id конкурса
    contest_works: [ |null          # список победителей по номинациям в конкурсе
        {
            autor_rusname: String,            # русскоязычное имя автора
            award_opened: Boolean,            # открыта ли страница премии
            contest_id: Int,                  # id конкурса
            contest_work_id: Int,             # id номинанта конкурса
            contest_year: Int,                # год проведения конкурса
            cw_link_id: Int,                  # id номинанта
            cw_link_type: String,             # тип номинанта (work, autor и т.д.) (итоговая сслыка: https://fantlab.ru/{cw_link_type}{cw_link_id})
            cw_name: String,                  # автор + имя/название номинанта в оригинале
            cw_number: Int,                   # ?
            cw_postfix: String,               # ?
            cw_prefix: String,                # ?
            cw_rusname: String,               # русскоязычные автор + имя/название номинанта
            cw_winner: Boolean,               # победитель (в данном случае всегда 1)
            nomination_id: Int,               # id номинации
            nomination_name: String,          # название номинации в оригинале
            nomination_number: Int,           # порядковый номер номинации
            nomination_rusname: String,       # русскоязычное название номинации
            work_rusname: String              # русскоязычное имя/название номинанта
        },
        ...
    ],
    date: Date,                     # дата проведения конкурса (в формате YYYY-MM-DD)
    description: String,            # описание конкурса
    description_length: Int,        # длина описания (в символах)
    name: String,                   # название конкурса в оригинале
    name_year: String,              # ?
    non_winner_count: Int,          # ?
    number: Int,                    # ?
    place: String,                  # место проведения
    short_description: String       # краткое описание
}
```
