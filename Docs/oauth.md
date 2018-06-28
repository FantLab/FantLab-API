# OAuth-авторизация

OAuth - протокол авторизации, в котором пользователь может разрешить приложению-клиенту доступ к данным пользователя (ресурсу) на основном сайте, не передавая приложению свои логин/пароль. 

Определения:
- **Пользователь** - человек, зарегистрированный на сайте `fantlab.ru`.
- **Клиент** - третья сторона (сервис/сайт/etc), которая может перенаправлять _пользователей_ на OAuth-авторизацию.
- **Ресурс** - структурированная информация, которую может запрашивать _клиент_ на сайте `fantlab.ru` после авторизации с разрешения _пользователя_.

## Порядок авторизации
1. [Основной запрос на авторизацию](#Основной-запрос-на-авторизацию)
2. [Получение токена доступа](#Получение-токена-доступа) / [С использованием токена восстановления](#Получение-токена-доступа-с-использованием-токена-восстановления)
3. [Получение ресурса с использованием токена](#Получение-ресурса-с-использованием-токена)

## Основной запрос на авторизацию

Запрос
```
GET /oauth/login?response_type=code&client_id={clien_id}&redirect_uri={redirect_uri}&redirect_noauth_uri={redirect_noauth_uri}&redirect_cancel_uri={redirect_cancel_uri}
```

Параметры
```
client_id - идентификатор клиента
redirect_uri - uri для перенаправления после удачной авторизации
redirect_noauth_uri - uri для перенаправления при неудачной авторизации (необязательный)
redirect_cancel_uri - uri для перенаправления по нажатию кнопки "Отмена" (необязательный)
```

Пример
```
GET /oauth/login?response_type=code&client_id=1023&redirect_uri=http%3A%2F%2Fexample.com%2Fauth%2Fok
```

Ответ
```
уточнить
```

## Получение токена доступа

Запрос
```
GET /oauth/token?grant_type=authorization_code&code={code}&client_id={client_id}&client_secret={client_secret}&redirect_url=${redirect_url}
```

Параметры
```
code - код, полученный после авторизации
client_id - идентификатор клиента
client_secret - секрет клиента
redirect_url - uri, используемый для перенаправления при логине
```

Пример
```
GET /oauth/token?grant_type=authorization_code&code=fhFPky2CG&client_id=1023&client_secret=mWoWHPDhg&redirect_url=http%3A%2F%2Fexample.com%2Fauth%2Foktoken
```

Ответ
```
{
    access_token: String,    # токен доступа
    expires_in: Int,         # время жизни токена (с.)
    refresh_token: String,   # токен восстановления
    token_type: String       # тип токена
}
```

## Получение токена доступа с использованием токена восстановления
В случае, если при получении ресурса выдается ошибка `access_denied`, это означает, что истек срок действия токена. Получить новый токен можно с помощью токена восстановления (`refresh_token`), который был получен вместе с токеном доступа.

Запрос
```
GET /oauth/token?grant_type=refresh_token&refresh_token={refresh_token}&client_id={client_id}&client_secret={client_secret}&redirect_url={redirect_url}
```

Параметры
```
refresh_token - токен восстановления
client_id - идентификатор клиента
client_secret - секрет клиента
redirect_url - uri, используемый для перенаправления при логине
```

Пример
```
GET /oauth/token?grant_type=refresh_token&refresh_token=fhFPky2CG&client_id=121212&client_secret=mWoWHPDhg&redirect_url=http%3A%2F%2Fexample.com%2Fcb%2F123
```

Ответ
```
{
    access_token: String,    # новый токен доступа
    expires_in: Int,         # время жизни токена (с.)
    refresh_token: String,   # новый токен восстановления
    token_type: String       # тип токена
}
```

## Получение ресурса с использованием токена

Запрос
```
GET /oauth/get?oauth_token={oauth_token}&client_id={client_id}&resources={resources}
```

Параметры
```
oauth_token - токен доступа
client_id - идентификатор клиента
resources - список запрашиваемых ресурсов (в качестве разделителя используется `,`)
```

Пример
```
GET /oauth/get?oauth_token=mWoWHPDhg&client_id=121212&resources=em,rn,nk,sx,av,bd,cn,ct
```

Ответ
```
{
    id: Int|null,      # id пользователя
    em: String|null,   # адрес электронной почты
    sx: String|null,   # пол
    rn: String|null,   # настоящее имя
    nk: String|null,   # логин
    av: Url|null,      # url аватарки
    bd: Long|null,     # дата рождения (UNIX-таймштамп)
    cn: String|null,   # страна
    ct: String|null    # город
}
```

## Порядок получения `code` для клиента
Для тестирования можно использовать следующие параметры (ограничены по количеству обращений):
```
code = test321
client_secret = 2onNZkfjEK
```
При запуске приложения в релиз обратитесь на `support@fantlab.ru` для получения собственного кода для клиента.
