# Книжные полки

Общие замечания по запросам содержимого книжных полок:
- Для всех запросов содержимого тип полки следует знать заранее, поскольку неверный тип полки не является невалидным для данного запроса - просто id будут восприняты как id item-ов другого типа
- За один раз отдается 10 позиций, так что offset в запросе может принимать значения от `0` до `10 * (page_count - 1)`

## Содержимое полки произведений
Запрос
```
/GET https://fantlab.ru/bookcasechange{id}?offset={offset}&type=work
```
Параметры
```
id - id книжной полки
offset - смещение от начала
```
Пример
> [/bookcasechange518?offset=10&type=work](https://fantlab.ru/bookcasechange518?offset=10&type=work) - 2 страница полки "e-texts (engl) - антологии" Claviceps P.
Ответ
```
{
    bookcase_id: Int,                        # id книжной полки
    bookcase_items: [ | null                 # содержимое
        {
            autor1_name: String|null,        # 1-й автор издания
            autor2_name: String|null,        # 2-й автор издания
            autor_name: String|null,         # комбинированная строка с авторами
            bookcase_item_id: Int,           # id item-а книжной полки
            item_comment: String|null,       # комментарий
            item_id: Int,                    # id произведения
            name: String,                    # название произведения
            rusname: String                  # русскоязычное название произведения
        },
        ...
    ],
    bookcase_type: String,                   # тип книжной полки
    count: Int,                              # количество item-ов на книжной полке
    current_page: Int|null,                  # номер текущей страницы (если null, то первая)
    offset: Int,                             # смещение
    offset_h: Int,                           # верхняя граница смещения
    page_count: Int                          # номер текущей страницы
}
```

## Содержимое полки изданий
Запрос
```
/GET https://fantlab.ru/bookcasechange{id}?offset={offset}&type=edition
```
Параметры
```
id - id книжной полки
offset - смещение от начала
```
Пример
> [/bookcasechange1?offset=10&type=edition](https://fantlab.ru/bookcasechange1?offset=10&type=edition) - 2 страница полки "Фантлаб рекомендует"

Ответ (отличается только массив `bookcase_items`)
```
{
    bookcase_items: [ | null
        {
            autors: String,                  # авторы издания
            bookcase_item_id: Int,           # id item-а книжной полки
            edition_id: Int,                 # id издания
            item_comment: String|null,       # комментарий
            name: String,                    # название издания 
            publisher: String,               # издатель
            year: Int                        # год издания
        },
        ...
    ],
    ...
}
```

## Содержимое полки фильмов
Запрос
```
/GET https://fantlab.ru/bookcasechange{id}?offset={offset}&type=film
```
Параметры
```
id - id книжной полки
offset - смещение от начала
```
Пример
> [/bookcasechange488?offset=0&type=film](https://fantlab.ru/bookcasechange488?offset=0&type=film) - 1 страница полки "Любимая фантастика" vad'a

Ответ (отличается только массив `bookcase_items`)
```
{
    bookcase_items: [ | null
        {
            bookcase_item_id: Int,           # id item-а книжной полки
            country: String,                 # страна
            director: String,                # режиссер(ы) фильма
            film_id: Int,                    # id фильма
            item_comment: String|null,       # комментарий
            name: String,                    # название фильма
            rusname: String,                 # русскоязычное название фильма
            year: Int                        # год выпуска
        },
        ...
    ],
}
```

## Добавление или удаление произведения с книжной полки
Запрос
```
/GET https://fantlab.ru/bookcaseclick{work_id}bc{bookcase_id}change{true|false}
```
Параметры
```
work_id - id проивзедения
bookcase_id - id книжной полки
change - добавить (true) или удалить (false)
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
/GET https://fantlab.ru/bookcaseclick1bc1changetrue - добавить "Гиперион" Дэна Симмонса на книжную полку с id = 1
```
Ответ
```
HTML-разметка страницы произведения
```

## Добавление или удаление издания с книжной полки
То же самое, что и при добавлении/удалении произведения, только вместо `work_id` подставляется `edition_id`.

## Добавление или удаление фильма с книжной полки
То же самое, что и при добавлении/удалении произведения, только вместо `work_id` подставляется `film_id`.
