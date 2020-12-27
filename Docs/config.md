# Константы

Запрос
```
GET /config.json
```

Ответ:
```
{
    work_types: [                # типы произведений          
        {
            id: Int,             # id типа
            image: Url,          # ссылка на заглушку
            title: {             # название
                en: String,      # на английском
                rus: String      # на русском
            }
        },
        ...
    ]
}
```
