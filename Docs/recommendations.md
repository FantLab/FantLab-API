# Рекомендации

## Удаление рекомендации в мусорку
Запрос
```
/GET https://fantlab.ru/rtrashwork{id}
```
Параметры
```
id - id произведения
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
/GET https://fantlab.ru/rtrashwork1 - удаление "Гипериона" в мусорку рекомендаций
```
Ответ
```
HTML-шаблон
```

## Правка мусорки рекомендаций
Запрос
```
/POST https://fantlab.ru/rectrash/saverecomstrash
```
Параметры
```
cookie - авторизационный хэдер (fl_s=*)

w{id_1} = on (достать из мусорки)
w{id_2} = off (оставить в мусорке)
action = rectrash/saverecomstrash
```
Ответ
```
HTML-шаблон
```
