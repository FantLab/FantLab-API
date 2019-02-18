## Файлы конфигурации. Глобальные переменные

### Типы произведений

Запрос
```
GET /conf/worktypes.json
```

Пример
> [/conf/worktypes.json](https://api.fantlab.ru/conf/worktypes.json)

Ответ:
```
  {
    work_type_id: Int,       # id типа произведения
    work_type: String,       # id-name типа произведения
    work_type_name: String,  # тип произведения на русском
    work_type_icon: String,  # id-name группы типов произведений (novel, cycle и пр.). используются для вывода минииконки
    work_type_image: Url,    # ссылка на иконку-заглушку для постера. для использования если нет самого постера
  },
  ...
```
