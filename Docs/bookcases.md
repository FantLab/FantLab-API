# Книжные полки

Личные подборки юзеров. Могут быть разных типов - произведений ("Буду читать", "Жду перевод", "Любимая детская фантастика" и прочие), Изданий - "Хочу приобрсти",

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
> https://api.fantlab.ru/user/3083/bookcases

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
> https://api.fantlab.ru/user/3083/bookcases/3056 - полка id 3056 юзера с id 3083

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
/my/bookcases/* . 
"/my/" - зона API выделелнная для работа пользователям с своими личными данныме;  
в отличие от роутов библиобазы тут построение урлов используют логику items/id/action и множественное число  



## Создание книжной полки
Запрос
```
AUTH GET /my/bookcases/add?name={name}&type={work|edition|film}&shared={0|1}&comment={comment}
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
> https://api.fantlab.ru/my/bookcases/add?name=Test%20bookcase&type=work&shared=0&comment=Test%20comment

Ответ
```
id новосозданной книжной полки
```
Примечание: содержимое полки в запросе передать нельзя. Предполагается, что элементы будут добавляться пользователем по одному вручную.


## Редактирование книжной полки
Запрос
```
GET /my/bookcases/{bookcase_id}edit?name={name}&type={work|edition|film}&shared={0|1}&comment={comment}
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
AUTH GET /my/bookcases/editbookcase11945?name=Test%20bookcase&type=work&shared=0&comment=Test%20comment
```
Ответ
```
1 в случае успешного завершения
```


## Удаление книжной полки
Запрос
```
AUTH GET /my/bookcases/{bookcase_id}/delete
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе)

Параметры
```
bookcase_id - id книжной полки для редактирования (передаваемый пользователь должен быть владельцем этой полки)
```

Пример
> https://api.fantlab.ru/my/bookcases/186919/delete

Ответ
```
1 в случае успешного завершения
```


## Содержимое одной полки авторизованного пользователя
Запрос
```
AUTH GET /my/bookcases/{bookcase_id}?offset={offset}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).   
В отличие от запроса для неавторизованного отдаются все полки (открытые и закрытые).   
Метод используется для получения содержимого полок всех типов, но ответ будет отличаться для разных типов.  
Но при попытке просмотреть полку с владельцем, не совпадающим с переданным, вернется ошибка.   

Параметры
```
user_id - id пользователя
bookcase_id - id пользователя
offset - смещение от начала
```
Пример
> https://api.fantlab.ru/my/bookcases/3056

Ответ для полки с произведениями:
```
Идентичен ответу для неавторизованного
```


## Добавление или удаление элементов с книжной полки
Запрос
```
AUTH GET /my/bookcases/{bookcase_id}/items/{item_id}/add    # добавить на полку 1 позицию
AUTH GET /my/bookcases/{bookcase_id}/items/{item_id}/delete # убрать с полки 1 позицию
альтернативный урл (уст.):
AUTH GET /my/bookcases/bookcaseclick{item_id}bc{bookcase_id}change{true|false}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).

Параметры
```
item_id - id элемента (произведения, издания или фильма)
bookcase_id - id книжной полки
и альтурнативном случае:
change - добавить (true) или удалить (false)
```
Пример
> https://api.fantlab.ru/my/bookcases/3056/items/1/add - добавить "Гиперион" Дэна Симмонса на книжную полку с id = 3056

Ответ
```
Количество элементов на полке после изменения
```


## Добавление комментария к item
Запрос
```
AUTH GET /my/bookcases/{bookcase_id}/items/{item_id}/editcomm?txt={comment}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).

Параметры
```
bookcase_id - id книжной полки
item_id - id item'а
txt - комментарий
```

Пример
> /my/bookcases/123/items/456/editcomm?txt=Отдал%20Васе

Ответ
```
1 в случае успеха
```


## Список всех полок авторизованного пользователя
Запрос
```
AUTH GET /my/bookcases/
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе).   
В отличие от списка для неавторизованного отдаются все полки (открытые и закрытые)

Параметры
```
user_id - id пользователя
```

Пример
> https://api.fantlab.ru/my/bookcases

Ответ
```
Идентичен ответу на запрос полок для неавторизованного пользователя
```


## Список полок авторизованного пользователя одного типа + наличие элемента на полках пользователя

*Используются для вывода список полок юзера в карточке базы.*

Запрос
```
AUTH GET /my/bookcases/type/{type}/{item_id}
```
Требует авторизации (передачи аутентификационного заголовка или кука в запросе). Возвращает список всех полок пользователя данного типа с признаком, присутствует ли данный элемент на этой полке (используются для показа списка полок в карточке - сразу список полок юзера и в каких подборках данных элемент есть)

Параметры
```
item_id - id item'а
type - тип полки (work|edition|film)
```

Пример
> https://api.fantlab.ru/my/bookcases/type/edition/281

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


___
##### персональные роуты для залогиненного пользователя (копия) - прошлый формат роутов
```
      $r->route('/my/bookcases/showbookcases')->over('login')->to('bookcase#personal_list')->name('bookcase_personal_list');
      $r->route('/my/bookcases/viewbookcase<:bookcase_id>', bookcase_id => qr/\d+/)->over('login')->to('bookcase#personal_show')->name('bookcase_personal_show');
      $r->route('/my/bookcases/addbookcase')->over('login')->to('bookcase#add')->name('bookcase_add');
      $r->route('/my/bookcases/editbookcase<:bookcase_id>')->over('login')->to('bookcase#edit')->name('bookcase_edit');
      $r->route('/my/bookcases/deletebookcase<:bookcase_id>')->over('login')->to('bookcase#remove');
      $r->route('/my/bookcases/bookcaseclick<:item_id>bc<:bookcase_id>change<:checked>')->over('login')->to('bookcase#add_remove_bookcase_item');
      $r->route('/my/bookcases/bookcasecomm<:bookcase_id>item<:item_id>comm')->over('login')->to('bookcase#comment_item');
      $r->route('/my/bookcases/viewitem<:item_id>')->over('login')->to('bookcase#item_bookcases')->name('item_bookcases_list');
```

