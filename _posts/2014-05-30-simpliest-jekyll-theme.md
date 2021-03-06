---
title: smpl - простейшая тема для Jekyll
link: https://github.com/neoascetic/neoascetic.github.io
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

Итак,   как    пользоваться   `smpl`?   Всё   просто.    Форкаем   [репозиторий
темы][theme-repo]  и  переименовываем  его  в  `<никнейм-на-гитхаб>.github.io`.
Удаляем   все  посты   из   директорий  `_posts`,   `_drafts`   и  `about`,   а
также  изображения  из  `images`.   Изменяем  `CNAME`,  если  нужен  [кастомный
домен][github-pages-custom-domain],   либо   попросту   удаляем   его.   Меняем
`_config.yml`  согласно своим  требованиям. Всё  готово! Можно  начинать писать
посты.   Они  должны   быть   в  формате   [markdown][github-flavored-markdown]
и    располагаться   в    директории   `_posts`.    Именоваться   -    согласно
[требованиям    Jekyll][jekyll-post-requirements]   (YYYY-MM-DD-post-title.md).
В   верхушке   каждого   файла    поста   должен   присутствовать   минимальный
[front-matter][jekyll-front-matter], в  котором можно  переопределить заголовок
поста если сгенерированный из имени файла не устраивает.

Далее - согласно  [документации Jekyll][jekyll-docs]. На самом  деле, уже можно
сделать git push и любоваться своим минималистичным блогом.

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
