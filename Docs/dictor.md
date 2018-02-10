# Диктор
Запрос
```
GET /dictor/{id}?biography={0|1}&awards={0|1}&editions={0|1}
```
Параметры
```
id - id диктора
biography - выводить ли биографию (необязательный; по-умолчанию 0)
awards - выводить ли награды (необязательный; по-умолчанию 0)
editions - выводить ли издания (необязательный; по-умолчанию 0)
```
Пример
```
GET /dictor/1?biography=1&awards=1&editions=1 - полная информация про диктора Влада Коппа
```
Ответ
```
{
    age: Int,                     # возраст на данный момент или на момент смерти
    anons: String,                # анонс биографии
    articles: [                   # список статей
        ...: ?|null
    ],
    awards: [ |null               # [awards] список наград
        ...: ?|null
    ],    
    biography: String|null,       # [biography] биография (могут присутствовать bb-теги)
    biography_notes: String,      # примечание к биографии
    biography_source: String,     # источник биографии
    birthday: Date,               # дата рождения (в формате YYYY-MM-DD)
    countries: [                  # список стран
        {
            country_id: Int,      # id страны
            name: String          # страна
        },
        ...
    ],
    curator_id: Int,              # id куратора
    curator_name: String,         # логин куратора
    deathday: Int|null,           # дата смерти
    editions: [ |null             # [editions] список изданий
        {
            autors: String,         # авторы
            compilers: String,      # составители
            correct_level: Float,   # уровень проверенности издания (0 - не проверено, 0.5 - проверено не полностью, 1 - не проверено)
            cover_type: Int,        # id типа обложки
            ebook: Boolean,         # электронная книга или нет
            edition_id: Int,        # id издания
            lang: String,           # русскоязычное название языка
            lang_code: String,      # код языка
            lang_id: Int,           # id языка
            name: String,           # название издания
            pic_num: Int,           # ?
            plandate: ?|null,       # ?
            plandate_txt: String,   # пояснение насчет планируемой даты
            type: Int,              # id типа издания
            year: Int               # год издания
        },
        ...
    ],
    id: Int,                      # id диктора
    image: String,                # ссылка на основное фото
    image_preview: String,        # ссылка на превью основного фото
    name: String,                 # имя
    name_rp: String,              # имя в родительном падеже
    notes: String,                # ?
    person_type: String,          # тип персоны (в данном случае всегда dictor)
    sex: String,                  # пол (m - мужской, f - женский)
    site_links: [ |null           # список официальных страниц
        {
            hint: String,         # подсказка (например, соц. сеть)
            icon: String,         # иконка (ссылка на изображение: https://fantlab.ru/img/{icon})
            text: String,         # название ссылки
            url: String           # адрес ссылки
        },
        ...
    ],
    stat: {                       # статистика
        editioncount: Int         # количество изданий
    }
}
```
