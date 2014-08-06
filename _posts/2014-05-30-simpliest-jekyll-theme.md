---
title: smpl - простейшая тема для Jekyll
---

Простейший (sic!) способ вести блог like a hacker с помощью Github Pages.

[Github     Pages][github-pages]    -     крутой    сервис.     Он    позволяет
бесплатно    хостить   статические    веб-сайты,    что   является    идеальным
вариантом   для   документации   различных   проектов.  Ещё   круче,   что   он
[позволяет использовать][github-pages-jekyll-usage] замечательный **генератор**
статических  сайтов   Jekyll,  дабы  привнести  какое-то   подобие  динамики  и
избавиться  от  необходимости  создавать  каждую  html-страницу  вручную  (либо
генерировать сайт локально).

При этом  при создании  контента для  сайта нет  нужды пользоваться  убогим или
не  очень  убогим  WYSIWYG-редактором,  иметь браузер  или  вообще  графическое
оформление! Контент  - это просто  текстовые файлы,  шаблоны - это  тоже просто
текстовые файлы, а  отправка изменений - не сабмит формы  через сайт, а обычный
push в git-репозиторий!

Кроме того,  Github ныне  предоставляет полноценный [C][]R[U][][D][]  для всего
этого. Я  им пользуюсь только  для правки небольших  багов, но ничто  не мешает
использовать его как полноценный WYSIWYG!

В общем, все крутые  кулхацкеры ведут блог именно так. И я  - не исключение! Но
меня не устраивала  ни дефолтная тема Джекилла, ни одна  из сотен имеющихся. Не
было в них...  минималистичности, что ли, даже среди множества  тем с префиксом
"minimalistic". Минималистичности  не только во внешнем  виде, но и в  том, что
под  капотом  - шаблонах,  скриптах,  стилях...  Поэтому было  принято  решение
замутить свою тему, с блэкджеком и прочим. В качестве основного требования была
простота во всем выше перечисленном.

Я  не  дизайнер   и  дизайнить  не  люблю,  но  -   ведь  ведь  удивительно!  -
минималистичный дизайн,  когда контент  ставится во  главу угла,  content first
то  бишь, с  одной  стороны  более удобен  для  восприятия  читателем (ибо  нет
отвлекающих  внимание свистоперделок  и прочей  обвески), а  с другой  - просто
реализуется.

*Шаблоны*: зачем куча  партиалов? Ограничимся одним, простым  для модификации и
понимания!  *Стили*:  только базовый  простейший  лэйуат,  стили для  элементов
по-умолчанию оставим на откуп браузеру - благо визуально в разных браузерах они
отличаются только значениями отступов и  полей. *Скрипты...* какие скрипты, для
чего?! Зачем  какая-то динамичность в простом  - и стремящимся быть  простым! -
блоге?

Итак,  как  пользоваться  `smpl`?   Минимальный  набор  файлов,  который  нужно
иметь  в  репозитории блога  -  это  `_config.yml` (основные  настройки  сайта,
обязательно  загляните  туда),  `index.html`  (основной  -  и  единственный!  -
шаблон) и `styles.css` (собственно, минимальный набор каскадных таблиц стилей).
Создаём  git-репозиторий  с  именем `<никнейм-на-гитхаб>.github.io`,  пихаем  в
его  корень эти  три файла,  скачанные  из [репозитория  темы][theme-repo] и  -
начинаем писать  посты. Посты (в  формате [markdown][github-flavored-markdown])
должны  лежать   в  директории  `_posts`,  именоваться   согласно  [требованиям
Jekyll][jekyll-post-requirements]    (YYYY-MM-DD-post-title.md).    Вместе    с
поддержкой  сервисом  версии 2  генератора  [front-matter][jekyll-front-matter]
стал  опциональным.  Если же  вас  устраивает  сгенерированный из  имени  файла
заголовок поста, можно его переопределить:

```yaml
---
title: A post title
---
```

Далее - согласно  [документации Jekyll][jekyll-docs]. На самом  деле, уже можно
сделать git push и любоваться своим минималистичным блогом.

Опционально можно также  скопировать файлы `404.html` и  `atom.xml`. Они служат
для генерации кастомной  страницы для ошибки 404  (предупреждение плюс редирект
на главную спустя пять секунд)  и atom-потока соответственно. Если есть желание
иметь блог на собственном домене,  можно обратиться к соответствующему [разделу
документации][github-pages-custom-domain] Github Pages.

Посмотреть  на   тему  и   оценить  всю  прелесть   её  минимализма   можно  на
[этом  сайте][demo],   а  соответствующую   ему  структуру  -   в  [репозитории
темы][theme-repo].



[github-pages]: https://pages.github.com/
[github-pages-jekyll-usage]: https://help.github.com/articles/using-jekyll-with-pages
[github-flavored-markdown]: https://help.github.com/articles/github-flavored-markdown
[github-pages-custom-domain]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages
[C]: https://github.com/blog/1327-creating-files-on-github
[U]: https://github.com/blog/143-inline-file-editing
[D]: https://github.com/blog/1545-deleting-files-on-github

[jekyll-post-requirements]: http://jekyllrb.com/docs/posts/
[jekyll-front-matter]: http://jekyllrb.com/docs/frontmatter/
[jekyll-docs]: http://jekyllrb.com/docs/home/

[demo]: http://neoascetic.me
[theme-repo]: https://github.com/neoascetic/neoascetic.github.io