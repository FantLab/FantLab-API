# Конкурс
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
    award_name: String,             # название премии в оригинале (!= null при sort=contest)
    award_rusname: String,          # русскоязычное название премии (!= null при sort=contest)
    contest_id: Int,                # id конкурса (!= null при sort=contest)
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
    date: Date,                     # дата проведения конкурса (в формате YYYY-MM-DD) (!= null при sort=contest)
    description: String,            # описание конкурса (!= null при sort=contest)
    description_length: Int,        # длина описания (в символах) (!= null при sort=contest)
    name: String,                   # название конкурса в оригинале
    name_year: String,              # ? (!= null при sort=contest)
    non_winner_count: Int,          # ? (!= null при sort=contest)
    number: Int,                    # ? (!= null при sort=contest)
    place: String,                  # место проведения (!= null при sort=contest)
    short_description: String       # краткое описание (!= null при sort=contest)
}
```
