# Пользователь

## Основная информация
Запрос
```
GET /user/{id}
```
Параметры
```
id - id пользователя
```
Пример
> GET [/user/1](https://api.fantlab.ru/user/1) - юзеринфо creator'а

Ответ
```
{
   user_id: Int,                      # id юзера
   login: String,                     # логин юзера
   avatar: Url,                       # ссылка на аватарку юзера
   fio: String,                       # фио юзера (необязательный)
   sex: String,                       # пол (m - мужской, w - женский)
   birthday: DateTime,                # дата рождения

   user_class: Int,                   # id класса развития
   class_name: String,                # названия класса развития на сайте
   level: Float,                      # развитие в классе

   location: String,                  # место жительства в текстовом виде (строка текста)
   country_name: String,              # страна
   country_id: Int,                   # id страны по внутренний базе
   city_name: String,                 # город
   city_id: Int,                      # id города по внутренний базе 

   date_of_reg: DateTime,             # дата регистрации на сайте
   date_of_last_action: DateTime,     # дата последнего посещения сайта
   user_timer: Int,                   # сколько времени был на сайте за последение 30 дней (секунды)
   sign: String,                      # подпись под постами на форуме

   markcount: Int,                    # оценено произведений
   responsecount: Int,                # написано отзывов на произведения
   descriptioncount: Int,             # написно аннотаций
   classifcount: Int,                 # сделано классификаций произведений
   votecount: Int,                    # кол-во получил плюсов за свои отзывы

   topiccount: Int,                   # открыл тем на форуме
   messagecount: Int,                 # написал постов на форуме
   bookcase_count: Int,               # заведено книжных поток

   curator_autors: Int,               # кол-во курируемых позиций на сайте
   ticket_count: Int,                 # написано тикитов в базу сайта

   autor_id: Int,                     # id автора в базе FL, если юзер является автором
   autor_name: String,                # имя
   autor_name_orig: String,           # имя англ.
   autor_is_opened: Int,              # открыт ли этот автор на фл (0 - нет / 1 - да)

   blog_id: Int,                      # id авторской колонки юзера (если есть)

   block: Int,                        # юзер в бане (0 - нет / 1 - да)     
   date_of_block: DateTime,           # дата начало бана
   date_of_block_end: DateTime,       # дата окончание бана (если null - бан вечный)
}
```


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
