# Fantlab-API
Начать отсюда: https://goo.gl/CwQfmb

1. Список авторов.
Запрос: https://api.fantlab.ru/autorsall
Результат:
`
{
    list: [
    {
        autor_id: int,
        is_fv: int (0/1 = false/true),
        name: String,
        name_orig: String|null,
        name_rp: String|null,
        name_short: String|null,
        type: String
    },
    ...
    ],
    liter: String,
    liter_code: int|null,
    liter_list: String
}
`
