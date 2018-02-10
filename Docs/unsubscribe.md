# Отмена подписки
Запрос
```
GET https://fantlab.ru/notification/unsubscribe/{id}.json
```
Параметры
```
id - id подписки
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
GET https://fantlab.ru/notification/unsubscribe/1.json - отписка от оповещений по подписке №1
```
Ответ
```
{
    status: Boolean      # успешно ли прошла процедура отписки
}
```
