# Книжные полки (подборки) юзеров

## Список полок пользователя
Запрос
```
GET /user{user_id}/bookcases
```
Отображаются список публичных книжных полок юзера

Параметры
```
user_id - id пользователя
```

Пример
> https://api.fantlab.ru/user3083/bookcases

Ответ
```
[
    {
        "bookcase_comment": String|null,            # Комментарий (описание) полки
        "bookcase_group": String,                   # Тип книжной полки из списка (free, sale, buy, read, wait)
        "bookcase_id": Int,                         # id книжной полки
        "bookcase_name": String,                    # Название полки
        "bookcase_shared": Int,                     # Тип полки: 0 - закрытая, 1 - публичная
        "bookcase_type": String,                    # Тип книжной полки из списка (work, edition, film)
        "date_of_add": String,                      # Дата добавления в формате: "2011-04-23 15:59:50"
        "date_of_edit": String|null,                # Дата последнего редактирования в формате: "2011-04-23 15:59:50"
        "item_count": Int,                          # Количество элементов на полке
        "sort": Int                                 # Порядковый номер при сортировке
    },
    ...
]
```



## Содержимое книжной полки (подборки)

Запрос
```
GET /user{user_id}/bookcase{bookcase_id}?offset={offset}
вариант - GET /bookcase/{bookcase_id}?offset={offset} # может пока не подерживаться
```
Не требует авторизации, но при попытке получить содержимое закрытой полки вернется ошибка. Метод используется для получения содержимого полок всех типов, но ответ будет отличаться для разных типов.

Параметры
```
user_id - id пользователя
bookcase_id - id пользователя
offset - смещение от начала (не обязательный, по у)
```

Пример
> https://api.fantlab.ru/user3083/bookcase3056

Ответ для полки с произведениями:
```
{
    bookcase_id: Int,                        # id книжной полки
    bookcase_items: [ | null                 # содержимое
        {
            bookcase_item_id: Int,           # id item-а книжной полки (счетчик)
            item_id: Int,                    # id произведения
            item_comment: String|null,       # комментарий
            name: String,                    # название произведения
            rusname: String                  # русскоязычное название произведения
            autor_name: String|null,         # комбинированная строка с авторами
            ext: {                           # карточка произведения с полной информацией
                ...
            }
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

Ответ для полки с изданиями (отличается только структура `bookcase_items`):
```
{
    ...
    bookcase_items: [ | null                 # содержимое
        {
            autors: String,                  # авторы издания
            bookcase_item_id: Int,           # id item-а книжной полки
            edition_id: Int,                 # id издания
            item_comment: String|null,       # комментарий
            name: String,                    # название издания
            publisher: String,               # издатель
            year: Int                        # год издания
            ext: {                           # карточка издания с полной информацией
                ...
            }
        },
        ...
    ],
    ...
}
```

Ответ для полки с фильмами (отличается только структура `bookcase_items`):
```
{
    ...
    bookcase_items: [ | null                 # содержимое
        {
            bookcase_item_id: Int,           # id item-а книжной полки
            country: String,                 # страна
            director: String,                # режиссер(ы) фильма
            film_id: Int,                    # id фильма
            item_comment: String|null,       # комментарий
            name: String,                    # название фильма
            rusname: String,                 # русскоязычное название фильма
            year: Int                        # год выпуска
            ext: {                           # карточка фильма с полной информацией
                ...
            }
        },
        ...
    ],
    ...
}
```



---


# Свои полки (авторизированные запросы)

Персонализированные авторизованные запросы для работы юзера со своими личными полками просходит в роут-зоне
/my/bookcases/*
"/my/" - зона API выделелнная для работа пользователям с своими личными данныме;


## Создание книжной полки
Запрос
```
GET /my/bookcases/addbookcase?name={name}&type={work|edition|film}&shared={0|1}&comment={comment}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе)

Параметры
```
name - название полки
type - тип полки (work - произведения, edition - издания, film - фильмы)
shared - открытая полка (1) или нет (0)
comment - текст комментария к книжной полке
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/addbookcase?name=Test%20bookcase&type=work&shared=0&comment=Test%20comment
```
Ответ
```
id новосозданной книжной полки
```

Примечание: содержимое полки в запросе передать нельзя. Предполагается, что элементы будут добавляться пользователем по одному вручную.

## Редактирование книжной полки
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/editbookcase{bookcase_id}?name={name}&type={work|edition|film}&shared={0|1}&comment={comment}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе)

Параметры
```
bookcase_id - id книжной полки для редактирования (передаваемый пользователь должен быть владельцем этой полки)
name - название полки
type - тип полки (work - произведения, edition - издания, film - фильмы)
shared - открытая полка (1) или нет (0)
comment - текст комментария к книжной полке

```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/editbookcase11945?name=Test%20bookcase&type=work&shared=0&comment=Test%20comment
```
Ответ
```
1 в случае успешного завершения
```

## Удаление книжной полки
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/deletebookcase{bookcase_id}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе)

Параметры
```
bookcase_id - id книжной полки для редактирования (передаваемый пользователь должен быть владельцем этой полки)
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/deletebookcase186919
```
Ответ
```
1 в случае успешного завершения
```


## Список полок авторизованного пользователя
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/showbookcases
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе). В отличие от списка для неавторизованного отдаются все полки (открытые и закрытые)

Параметры
```
user_id - id пользователя
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/showbookcases
```
Ответ
```
Идентичен ответу на запрос полок для неавторизованного пользователя
```

## Содержимое полки авторизованного пользователя
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/viewbookcase{bookcase_id}?offset={offset}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе). В отличие от запроса для неавторизованного отдаются все полки (открытые и закрытые). Но при попытке просмотреть полку с владельцем, не совпадающим с переданным, вернется ошибка. Метод используется для получения содержимого полок всех типов, но ответ будет отличаться для разных типов.

Параметры
```
user_id - id пользователя
bookcase_id - id пользователя
offset - смещение от начала
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/viewbookcase3056
```
Ответ для полки с произведениями:
```
Идентичен ответу для неавторизованного
```

## Добавление или удаление элементов с книжной полки
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/bookcaseclick{item_id}bc{bookcase_id}change{true|false}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).

Параметры
```
item_id - id элемента (произведения, издания или фильма)
bookcase_id - id книжной полки
change - добавить (true) или удалить (false)
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/bookcaseclick1bc1changetrue - добавить "Гиперион" Дэна Симмонса на книжную полку с id = 1
```
Ответ
```
Количество элементов на полке после изменения
```

## Добавление комментария к item
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/bookcasecomm{bookcase_id}item{item_id}comm?txt={comment}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).

Параметры
```
bookcase_id - id книжной полки
item_id - id item'а
txt - комментарий
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/bookcasecomm123item456comm?txt=Есть
```
Ответ
```
1 в случае успеха
```

## Наличие элемента на книжных полках
Запрос
```
/GET https://api.fantlab.ru/my/bookcases/viewitem{item_id}?type={work|edition|film}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе). Возвращает список всех полок пользователя данного типа с признаком, присутствует ли данный элемент на этой полке

Параметры
```
item_id - id item'а
type - тип полки
```
Пример
```
/GET https://api.fantlab.ru/my/bookcases/viewitem281.json?type=edition
```
Ответ (список полок)
```
[
    {
        "bookcase_id": Int,                         # id книжной полки
        "bookcase_name": String,                    # Название полки
        "item_added": Int,                          # Возвращается 1, если элемент присутствует на данной полке, и 0, если нет
    },
    ...
]

```



---
### Обновленные роуты для персональной работы с полками:

```
GET  /my/bookcases/                                 # список полок
POST /my/bookcases/add                              # создание новой полки (отправка формы)
GET  /my/bookcases/<:bookcase_id>                   # инфа о полке
POST /my/bookcases/<:bookcase_id>/save              # редактирование полки (отправка формы) 
POST /my/bookcases/<:bookcase_id>/delete            # удалить полку

GET  /my/bookcases/<:bookcase_id>/items/<:item_id>/add       # добавить на полку 1 позицию
GET  /my/bookcases/<:bookcase_id>/items/<:item_id>/delete    # убрать с полки 1 позицию
GET  /my/bookcases/<:bookcase_id>/items/<:item_id>/editcomm  # записать комменатрий к позиции

GET  /my/bookcases/viewitem<:item_type>/items/<:item_id>     # на каких полках есть эта позиция у юзера
``




