---
title: Юникод в URLах Django
---

Патчим django (и django-cms) для поддержки  символов юникода в URL. Сама Джанго
поддерживает юникод,  но не абсолютно  везде. К примеру,  валидатор `SLugField`
ругается, фильтр  `slugify` не-ascii  символы тупо  вырезает. Поискав,  я нашел
пару патчей и много дискуссий...

Как вариант, можно  юзать транскрипцию, но выглядит она  настолько уродско, что
даже модификация пакетов нанесет меньший урон карме.

Итак, модифицируем `django.template.defaultfilters.slugify`:

```python
# django/template/defaultfilters.py
def slugify(value):
    value = unicodedata.normalize('NFKD', value)
    value = unicode(re.sub('(?u)[^\w\s-]', '', value).strip().lower())
    return mark_safe(re.sub('[-\s]+', '-', value).strip('-'))
```

Модифицируем валидатор  `SlugField`. Все, что нужно  - это добавить флаг  `U` к
регэкспу:

```python
# django/core/validators.py
slug_re = re.compile(r'^[-\w]+$', re.U)
```

Вот и все. Работы не ахти как  много, система вроде как работает, крупных (да и
мелких) косяков я пока не заметил.

Теперь - бонус.  Модифицируем django-cms для тех же  целей. При конструировании
slug   страницы   (если  он   не   добавлен   вручную)  cms   использует   (уже
модифицированный нами ранее) фильтр  `slugify`. При этом валидирует добавленный
вручную slug  с помощью соответствующего  валидатора. Но он  тоже модифицирован
уже, и  на юникод ругаться  не должен. Однако  - при запросе  страницы получаем
`404`. WTF, спрашивается? Ответ вновь в  регэкспах, а именно - в url-pattern'ах
CMS.

Смотрим    в   самое    начало   `cms.urls`    и   видим    url   под    именем
`pages-details-by-slug`, который ограничивает slugи маской `[0-9A-Za-z-_.//]+`,
то есть цифры,  буквы разных регистров, дефис, нижний подчерк,  точка и слэш от
одного и до скольки угодно. Буквы - только латинские, опять же.

Меняем условия, которые  там написаны только ради добавления слеша  в конце при
включенной соответсвующей опции на это:

```python
# cms/urls.py
reg_re = r'(?u)^(?P<slug>[-.//\w]+)%s$' % (settings.APPEND_SLASH and '/' or '')
reg = url(reg_re, details, name='pages-details-by-slug')
```

Таким нехитрым способом можно заставить  django-cms понимать юникод в URL, что,
собственно, и  требовалось. Почему это  до сих пор  не реализовано, я  не знаю.
Возможно, контрибуторам  не нравится  идея таких URLов,  как мне,  например, не
нравится это дело в доменных именах.

В   качестве  возможного   улучшения:   заменить   модификацию  исходников   на
манкипатчинг во время запуска приложения. Пока не задумывался, как.

* * *

Несколько обновлений.

При создании новой страницы  в django-cms используется автоматическая генерация
slugа на основе  заголовка. Проблема в том, что он  транслитерируется. Я обошел
это следующим образом.

Во-первых, создал  в папке со  статикой каталоги `admin/js/` и  скопировал туда
исходный  джанговский `urlify.js`  ([скачать с  гитхаба][urlify.js]). Благодаря
особенности  работы джанговского  фреймворка  статики, мы  можем таким  образом
"оверрайдить"  статику в  своих  проектах.  С другой  стороны,  раз уж  внаглую
пропатчили Django - почему бы и статику не пропатчить там же? Дело вкуса.

Во-вторых,  закомментировал   строчку  `ALL_DOWNCODE_MAPS[4]=RUSSIAN_MAP`.  Это
делает недоступным словарь русских символов, подлежащих замене.

В-третьих,   изменил   регулярку   в  операции   замены   "ненужных"   символов
(`s.replace(/[^-\w\s]/g, '');`)  на то, что  не будет сжирать  русские символы:
`/[^-а-яА-Я\w\s]/g`.  При желании  можно добавить  также символы  `ёЁ`, они  не
входят в интервалы русских символов.

Таким  же  нехитрым  способом  можно разрешить  использование  символов  других
языков, но русского в моем случае было вполне достаточно.

* * *

Я  [использую  uwsgi][uwsgi post]  в  своих  проектах. После  проделанных  мной
изменений процесс начал падать на некоторых URLах. В логах было ругание навроде
`invalid request block  size: 6702...skip`, мол, слишком  большой размер (http)
блока, пропускаю - ведь на юникод тратится больше байт.

Увеличение размера  буфера (по  умолчанию равен четырем  килобайтам) директивой
`buffer-size = 32768` в ini-файле решило проблему.



[urlify.js]: http://git.io/VIR5Ow
[uwsgi post]: {% post_url 2012-05-30-nginx-uwsgi-django-virtualenv %}
