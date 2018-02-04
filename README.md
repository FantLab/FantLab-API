# Fantlab-API

Публичный гитхаб-репозиторий по API для библиобазы fantlab.ru

В настоящий момент используется для информирования о процессе работы над API сайта через тикет-систему Issues и для взаимодействия API фантлаба и работающих с ним программистов самого ФЛ и использующих его  клиентами и веб-сервисами.

Обновляемое руководство по API сайта - https://goo.gl/CwQfmb  
(обратите внимание, что до релиза первой версии API возможно большие изменения в API-выдачах)

Предложения и замечания по работе API пишите в Issues данного репозитория.

**Домен для запросов API - https://api.fantlab.ru**

## Список авторов
Запрос: 
```
GET /autorsX, X - код заглавной буквы фамилии автора на русском языке.
GET /autorsall - для запроса списка всех авторов
```
Ответ:
```
{
    list:[
        {
            autor_id: Int,         # id автора
            is_fv: Int,            # является ли автор фантастиковедом (1 - да, 0 - нет)
            name: String,          # имя на русском языке
            name_orig: String,     # имя на языке оригинала
            name_rp: String,       # имя на русском языке в родительном падеже
            name_short: String,    # имя на русском языке для перечислений (сначала фамилия, затем имя)
            type: String           # тип автора (в данном случае всегда autor)
        },
        ...
    ],
    liter: String,                 # по какой заглавной букве фамилии ищем (в случае, если всех авторов - пустая строка)
    liter_code: Int|null,          # код заглавной буквы фамилии
    liter_list: String             # список ссылок на побуквенные списки авторов
}
```
