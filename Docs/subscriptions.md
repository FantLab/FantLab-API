# Подписки
## Подписка на оповещения о новых произведениях или изданиях автора
Запрос
```
GET https://fantlab.ru/notification/subscribe/autors/{id}/{works|editions}.json
```
Параметры
```
id - id автора
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
GET https://fantlab.ru/notification/subscribe/autors/133/works.json - подписка на оповещения о новых произведениях Дж. Р.Р. Мартина
```
Ответ
```
{
    id: Int,            # id подписки
    status: Boolean     # успешно ли выполнена операция
}
```
## Подписка на оповещения о новых изданиях с переводами переводчика
Запрос
```
GET https://fantlab.ru/notification/subscribe/translators/{id}/editions.json
```
Параметры
```
id - id переводчика
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
GET https://fantlab.ru/notification/subscribe/translators/1/editions.json - подписка на оповещения о новых изданиях с переводами И.Г. Гуровой
```
Ответ
```
{
    id: Int,           # id подписки
    status: Boolean    # успешно ли выполнена операция
}
```
## Отмена подписки
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
    status: Boolean      # успешно ли выполнена операция
}
```
