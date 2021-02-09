# Константы

Запрос
```
GET /config.json
```

Ответ:
```
{
    film_types: [{           # типы фильмов
        id: Int,             # id типа
        title: String        # название
    }],
    work_types: [{           # типы произведений          
        id: Int,             # id типа
        image: Url,          # ссылка на заглушку
        title: {             # название
            en: String,      # на английском
            rus: String      # на русском
        }
    }]
}
```
