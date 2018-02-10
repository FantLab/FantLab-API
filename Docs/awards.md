# Список премий
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
