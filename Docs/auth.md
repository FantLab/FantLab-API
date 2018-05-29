# Авторизация
Запрос
```
POST /login
```
Параметры
```
login - логин пользователя
password - пароль пользователя
```
Если авторизация прошла успешно, в response будет присутствовать хэдер *Set-Cookie* с куком *fl_s*. Если он отсутствует, значит, авторизация не удалась.
Пример:
```
Set-Cookie: fl_s=Z5RrvohMG2v56xph; expires=Mon, 30 Jul 2018 06:46:11 GMT; domain=.fantlab.ru;
```
Срок валидности данного кука равен 1 году. Для подписи запросов следует подставлять его в хэдер _Cookie_. В данном случае
```
fl_s=Z5RrvohMG2v56xph
```

**Nota bene**

При авторизации следует отключить автоматический редирект в HttpClient'е, иначе произойдет перенаправление на главную сайта, а там хэдера _Set-Cookie_ уже нет. В _Retrofit_ это можно сделать при первоначальном конфигурировании:
```java
httpClientBuilder.setFollowRedirects(false)
```
В _Fuel_ чуть сложнее:
```kotlin
FuelManager.instance.apply {
    removeAllResponseInterceptors()
	  addResponseInterceptor(validatorResponseInterceptor(200..299))
}
```
**TODO**

Перейти на использование [JSON Web Tokens](https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec).
