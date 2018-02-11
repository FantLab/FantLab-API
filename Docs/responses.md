
# Отзывы

## Новые отзывы на сайте
*ожидается*


## Отзывы на все произведения автора

Запрос
```
GET /autor/{id}/responses
GET /autor/{id}/responses?page={page}&sort={date|rating|mark}
```

Параметры
```
id - id автора
page - страница (необязательный; по-умолчанию 1). Запрос с пагинацией, отдает по 50 отзывов на странице
sort - вариант сортировки (необязательный; по-умолчанию по date - т.е. вначале самые новые отзывы)
    * date - по дате (default)
    * rating - по рейтингу отзыва
    * mark - по оценке
```

Пример
> GET [/autor/133/responses?page=2&sort=rating](https://api.fantlab.ru/autor/133/responses?sort=rating) - 50 хороших отзывы на произведения Дж. Р.Р. Мартина

Ответ:
```
[
    {
        response_id: Int,                            # id отзыва
        response_date: DateTime,                     # дата-время написания отзыва
        response_text: String,                       # текст отзыва
        response_votes: int,                         # рейтинг отзыва
        mark: Int,                                   # оценка произведению, данная вместе с отзывом (1-10)

        work_id: Int,                                # id произведения
        work_name: String,                           # название произведения на русском
        work_name_orig: String,                      # оригинальное название произведения
        work_type_id: Int,                           # id типа произведения

        user_id: Int,                                # юзер-id
        user_name: String,                           # login
        user_sex: Int,                               # мол: m- мужской, w-женский
    },
    ...
]
```


## Отзывы на одно произведение

```
GET /work/{id}/responses
```

Параметры
```
id - id автора
page - страница (необязательный; по-умолчанию 1). Запрос с пагинацией, отдает по 15 отзывов на странице
sort - вариант сортировки (необязательный; по-умолчанию по rating - т.е. вначале самые хорошие отзывы (в отличие от списка отзывов на автора - там по умолчанию "новые")
    * date - по дате
    * rating - по рейтингу отзыва (default)
    * mark - по оценке
```

Пример:
> [/work/1/responses](https://api.fantlab.ru/work/1/responses) - хорошие отызывы на "Гиперион" Дэна Симменса  
> [/work/1/responses?sort=date](https://api.fantlab.ru/work/1/responses?sort=date) - новые отзывы на "Гиперион" Дэна Симменса

Ответ:
```
полностью соответвует выдачи как у списка отзывов автора
```

