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
    contests: [ |null          # [include_contests] список конкурсов
        {
            award_id: Int,                       # id премии
            award_name: String|null,             # название премии в оригинале (!= null при sort=contest)
            award_rusname: String|null,          # русскоязычное название премии (!= null при sort=contest)
            contest_id: Int|null,                # id конкурса (!= null при sort=contest)
            contest_works: [ |null               # список победителей по номинациям в конкурсе
                {
                    autor_rusname: String,            # русскоязычное имя автора
                    award_opened: Boolean,            # открыта ли страница премии
                    contest_id: Int,                  # id конкурса
                    contest_work_id: Int,             # id номинанта конкурса
                    contest_year: Int,                # год проведения конкурса
                    cw_link_id: Int,                  # id номинанта
                    cw_link_type: String,             # тип номинанта (work, autor и т.д.)
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
            date: Date|null,                     # дата проведения конкурса (в формате YYYY-MM-DD) (!= null при sort=contest)
            description: String|null,            # описание конкурса (!= null при sort=contest)
            description_length: Int|null,        # длина описания (в символах) (!= null при sort=contest)
            name: String,                        # название конкурса в оригинале
            name_year: String|null,              # ? (!= null при sort=contest)
            nomination_id: Int|null,             # id номинации (!= null при sort=nomi)
            non_winner_count: Int|null,          # ? (!= null при sort=contest)
            number: Int|null,                    # ? (!= null при sort=contest)
            place: String|null,                  # место проведения (!= null при sort=contest)
            rusname: String|null,                # русскоязычное название конкурса (!= null при sort=nomi)
            short_description: String|null       # краткое описание (!= null при sort=contest)
        },
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
