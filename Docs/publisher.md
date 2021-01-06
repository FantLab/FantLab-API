# Издательства

## Список издательств

Запрос
```
GET /publishers?letter={letter}&country_id={country_id}&type={type}&from={from}&to={to}&sort={sort}&page={page}
```

Параметры
```
letter - первая буква названия, от А до Я, от A до Z (необязательный; по умолчанию любая [пустая строка])
country_id - id страны (необязательный; по умолчанию все [0])
type - категория издательств (необязательный; по умолчанию все [0])
    * 0 - все
    * 1 - фантастические
    * 2 - аудио-издательства
    * 3 - электронные
    * 4 - современные
    * 5 - советские
    * 6 - дореволюционные
    * 7 - эмигрантские
    * 8 - русское зарубежье
from - с какого года (необязательный; по умолчанию за все года [пустая строка])
to - по какой год (необязательный; по умолчанию за все года [пустая строка])
sort - вариант сортировки (необязательный; по умолчанию по названию [name])
    * name - по названию
    * editions_count - по количеству изданий
    * country - по стране
    * city - по городу
page - номер страницы (необязательный; по умолчанию 1). Запрос с пагинацией, отдает по 250 результатов на странице
```

Пример
> [/publishers?country_id=2&sort=editions_count](https://api.fantlab.ru/publishers?country_id=2&sort=editions_count) - издательства США, отсортированные по количеству изданий

Ответ:
```
[
    {
        alt_names: String,            # альтернативные названия
        audiopub: Boolean,            # аудио-издательство
        city: String,                 # город
        closed: Boolean,              # издательство закрыто
        country_id: Int,              # id страны (флаг, если country_id != 0, всегда располагается по ссылке https://data.fantlab.ru/images/flags/{country_id}.png)
        country_name: String|null,    # страна (null, когда country_id = 0)
        editions_count: Int,          # количество изданий
        epub: Boolean,                # электронное издательство
        is_fant: Boolean|null,        # фантастическое издательство
        name: String,                 # название
        nopub: Boolean,               # не является издательством как таковым
        publisher_id: Int,            # id (логотип, если есть, всегда располагается по ссылке htpps://data.fantlab.ru/images/publisher/{publisher_id}_1)
        site: Url|null,               # ссылка на сайт
        year_close: Int|null,         # год закрытия (null или 0, когда closed = 0)
        year_open: Int|null           # год открытия
    },
    ...
]
```

## Топ-30 издательств

Запрос
```
GET /publishers/top.json?country_id={country_id}
```

Параметры
```
country_id - id страны (необязательный; по умолчанию все [0])
```

Пример
> [/publishers/top.json?country_id=1](https://api.fantlab.ru/publishers/top.json?country_id=1) - топ-30 издательств России

Ответ:
```
аналогичен ответу /publishers
```
