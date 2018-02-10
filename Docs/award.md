# Премия
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
    contests: [ |null          # список конкурсов
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
    nominations: [ |null       # список номинаций
    ],
    non_fantastic: Boolean,    # нефантастическая премия?
    notes: String,             # примечания
    process_status: String,    # ?
    rusname: String,           # русскоязычное название
    show_in_list: Boolean      # ?
}
```
