# Личка

## Отправка/сохранение в черновик сообщения
Запрос
```
/POST(Multipart) https://fantlab.ru/user{id}/sendprivatemessage
```
Параметры
```
id - id пользователя, которому отправляется сообщение
cookie - авторизационный хэдер (fl_s=*)

message - текст сообщения
file1 (с полем fileName и типом application/octet-stream), file2... - приложенные файлы
mode - режим отправки, реальная (send) или сохранение в черновик (preview)
action - действие (/user{id}/sendprivatemessage)
```
Пример
```
/POST(Multipart) https://fantlab.ru/user1/sendprivatemessage (message=Привет!) - отправить сообщение "Привет!" creator'у
```
Ответ
```
HTML-страница пользователя, которому отправляли сообщение
```

## Подтверждение отправки черновика
Запрос
```
/GET https://fantlab.ru/user{id}/submitpmpreview
```
Параметры
```
id - id пользователя, которому писали сообщение
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
/GET https://fantlab.ru/user1/submitpmpreview - подтвердить отправку сообщения creator'у
```
Ответ
```
HTML-страница пользователя, которому писали сообщение
```

## Отмена отправки черновика
Запрос
```
/GET https://fantlab.ru/user{id}/cancelpmpreview
```
Параметры
```
id - id пользователя, которому писали сообщение
cookie - авторизационный хэдер (fl_s=*)
```
Пример
```
/GET https://fantlab.ru/user1/cancelpmpreview - отменить отправку сообщения creator'у
```
Ответ
```
HTML-страница пользователя, которому писали сообщение
```
