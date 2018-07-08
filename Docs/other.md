## Теги, ссылки и пр.
В редакторе хватает кастомных тегов:
```
[b]text[/b] - жирный текст
[i]text[/i] - курсив
[u]text[/u] - подчеркнутый текст
[s]text[/s] - зачеркнутый текст

[p]text[/p] - абзац
[q]text[/q] - цитата
[h]text[/h] - спойлер

[LIST]
[*]1st
[*]2nd
[*]3rd
...
[/LIST] - список

[IMG]image_link[/IMG] - ссылка на изображение
[VIDEO=youtube_link] - ссылка на видео на Youtube
```

В приходящем с сервера тексте могут быть ссылки на внутренние сущности базы:
```
[autor=1]...[/autor] = "/autor1"
[work=1]...[/work] = "/work1"
[edition=1]...[/edition] = "/edition1"

[person=1]...[/person] = "/person1"
[user=1]...[/user] = "/user1"
[art=1]...[/art] = "/art1"
[dictor=1]...[/dictor] = "/dictor1"
[series=1]...[/series] = "/series1"
[film=1]...[/film] = "/film1"
[translator=1]...[/translator] = "/translator1"
[pub=1]...[/pub] = "/publisher1"

[link=blogarticle1]...[/link] = "/blogarticle1"
[url=http://google.com]...[/url] = "http://google.com" - внешняя ссылка
```

Также есть смайлы (набор `Kolobok smiles`), они в редакторе задаются через `:name:`.
