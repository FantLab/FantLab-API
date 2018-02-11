# Пользователь
## Список отзывов
Запрос
```
GET /user/{id}/responses?page={page}&sort={date|rating|mark}
```
Параметры
```
id - id пользователя
page - страница (необязательный; по-умолчанию 1). Запрос с пагинацией, отдает по 50 отзывов на странице
sort - вариант сортировки (необязательный; по-умолчанию date)
    * date - по дате
    * rating - по рейтингу
    * mark - по оценке
```
Пример
```
GET /user/1/responses?page=2&sort=rating - 50-100 отзывы (по убыванию рейтинга) creator'а
```
Ответ
```
[
    {
        name: String,             # название произведения в оригинале
        posted_date: DateTime,    # дата публикации
        response_id: Int,         # id отзыва
        response_text: String,    # текст отзыва
        rusname: String,          # русскоязычное называние произведения
        user_id: Int,             # id пользователя Fantlab'а - автора отзыва
        user_name: String,        # логин пользователя Fantlab'а - автора отзыва
        user_sex: Int,            # пол пользователя Fantlab'а - автора отзыва (1 - мужской, 0 - женский)
        user_voted: ?|null,       # ?
        vote_minus: Int,          # количество голосов "Против"
        vote_plus: Int,           # количество голосов "За"
        work_id: Int,             # id произведения
        work_type_id: Int         # id типа произведения
    },
    ...
]
```
