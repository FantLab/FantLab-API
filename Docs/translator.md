# Переводчик
## Основная информация
Запрос
```
GET /translator/{id}
```
Параметры
```
id - id переводчика
```
Пример
```
GET /translator/1 - переводчик Ирина Гавриловна Гурова
```
Ответ
```
{
    age: Int,        # возраст на данный момент или на момент смерти
    articles: [      # список статей
        ...: ?|null
    ],
    awards: [
        {
            award_icon: String,             # ссылка на логотип премии
            award_id: Int,                  # id премии
            award_in_list: Int,             # ?
            award_is_opened: Boolean,       # открыта ли страница премии
            award_name: String,             # название премии
            award_rusname: String,          # русскоязычное название премии
            contest_id: Int,                # id конкурса
            contest_name: String,           # название конкурса (обычно сопадает с годом)
            contest_year: Int,              # год проведения конкурса
            cw_id: Int,                     # ?
            cw_is_winner: Int,              # ?
            cw_postfix: String,             # ?
            cw_prefix: String,              # ?
            nomination_id: Int|null,             # id номинации
            nomination_name: String|null,        # название номинации
            nomination_rusname: String|null,     # русскоязычное название номинации
            work_id: Int,                   # id награжденного произведения (0, если награда относится не к произведению)
            work_name: String,              # название награжденного произведения
            work_rusname: String,           # русскоязычное название награжденного произведения
            work_year: Int|null             # год публикации произведения
        },
        ...
    ],
    biography: String,            # биография
    biography_notes: String,      # примечание к биографии
    biography_source: String,     # источник биографии
    birthday: Date,               # дата рождения
    countries: [                  # список стран
        {
            country_id: Int,      # id страны
            name: String          # страна
        },
        ...
    ],
    curator_id: Int,                 # id куратора
    curator_name: String,            # логин куратора
    deathday: Date,                  # дата смерти
    has_advanced_awards: Boolean,    # ?
    id: Int,                         # id переводчика
    name: String,                    # имя / фамилия
    name_orig: String,               # имя / фамилия в оригинале (для иностранных переодчиков)
    name_rp: String,                 # имя / фамилия в родительном падеже
    name_short: String,              # имя (инициалы) / фамилия
    name_short_rp: String,           # имя (инициалы) / фамилия в родительном падеже
    name_sort: String,               # полное ФИО для сортировки
    notes: String,                   # ?
    person_type: String,             # тип персоны (в данном случае всегда translator)
    photo: ?,                        # ?
    pseudo_id: Int,                  # id псевдонима
    pseudo_name: ?|null,             # ?
    sex: Int,                        # пол (1 - мужской, 2 - женский)
    show_in_list: Boolean,           # ?
    translated_from: String,         # "переводчик с ..."
    translated_to: String            # "переводчик на ..."
}
```
**TODO**
1. Список переведенных произведений
2. Список изданий с каждым переведенным произведением
3. Сортировку по году издания перевода / по автору / по типу произведения
## Список наград отдельно
Запрос
```
GET /translator/{id}/awards?sort={byorder|byaward|bychrono}
```
Параметры
```
id - id переводчика
sort - вариант сортировки наград (необязательный; по-умолчанию bychrono)
    * byorder - по порядку
    * byaward - по премиям
    * bychrono - по хронологии
```
Пример
```
GET /translator/1/awards?sort=byorder - награды И.Г. Гуровой по порядку
```
Ответ
```
[
    {
        ...          # те же самые поля, что и в translator -> awards
    }
]
```
**Nota bene**:

список отдается без разбиения по категориям
