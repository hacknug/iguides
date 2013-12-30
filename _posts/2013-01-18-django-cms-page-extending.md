---
layout: index
title: Расширение модели Page в Django-CMS
date: 2013-01-18 22:24
---

Уже несколько раз при разработке проектов на базе Django-CMS у меня возникала
задача привязать к той или иной странице дополнительную информацию. Чаще это
были этакие "аватарки" страницы, но однажды - и более объемная информация. Как
всегда, в первую очередь пошел искать, а не решил ли кто эту задачу до меня?

Как я и подозревал, с такой проблемой уже [сталкивались][1], и успешно решили -
тем же путем, о котором думал я. Фактически, здесь я приведу краткое содержание
трех статей в оригинальном блоге с некоторыми дополнениями.

Итак, наиболее очевидный и правильный (о чем [упоминают][2] даже авторы, но это
пока не нашло отражение в документации), не беря во внимание б-гопротивное
редактирование исходников, - прикрепление к Page модели с необходимой
информацией через внешний ключ:

```python
# app/models.py
from django.db import models
from django.utils.translation import ugettext_lazy as _
from cms.models.pagemodel import Page
from filebrowser.fields import FileBrowseField as FBF

class PageAvatars(models.Model):
    page = models.OneToOneField(Page,
                                editable=False,
                                verbose_name=_('Page'),
                                related_name='extended_fields')
    big_avatar = FBF(_(u'Big Avatar'), max_length=255, blank=True)
    small_avatar = FBF(_(u'Small Avatar'), max_length=255, blank=True)
```

В оригинальной статье предлагается использовать `ForeignKey`, но у нас связь
явно один-к-одному - поэтому я использую `OneToOneField`.

Далее, необходимо прикрепить связанную со страницей информацию в админке:

```python
# app/admin.py
from django.contrib import admin
from cms.admin.pageadmin import PageAdmin
from cms.models.pagemodel import Page
from app.models import PageAvatars

class PageAvatarsAdmin(admin.TabularInline):
    model = PageAvatars

PageAdmin.inlines.append(PageAvatarsAdmin)

# перерегистрируем страницу модели в админке
admin.site.unregister(Page)
admin.site.register(Page, PageAdmin)
```

Вроде бы все, но не тут-то было! Часто требуется получать доступ к этой
информации при построении меню (в шаблонах) - например, чтобы для каждой
странички вывести ее "аватар". Проблема в том, что в при построении меню через
стандартные шаблонные теги Django-CMS в качестве объекта навигации в шаблон
передаются наборы объектов `NavigationNode`, которые содержат лишь самую
минимальную информацию о странице. Сделано это ради оптимизации. Но - нам нужно
решить задачу! Поэтому - на лету добавляем классу `NavigationNode` метод для
получения связанной страницы (сделать это можно там же, в `models.py`):

```python
# app/models.py
from menus.base import NavigationNode
NavigationNode.page = property(lambda p: Page.objects.get(pk=p.id))
```

Теперь в шаблонах можно проделывать такие трюки: `child.page.big_avatar`.

Большая проблема в том, что "благодаря" использованному замыканию для каждого
элемента меню, учавствующего в шаблоне, будет извлекаться связанная страница, и
только после этого - связанный со страницей объект. Все это очень печально, ибо
появляется множество обращений к базе, что тормозит генерацию страницы. Если на
сайте используется кеширование, проблема становится менее значительной. Другой
способ уменьшить количество запросов (и объем ответов) - комбинировать нужным
образом методы QuerySet [only][] (или [defer][]) - для того чтобы ограничить
выбираемые из БД поля только необходимыми, а также [select_related][] - для
того, чтобы вытянуть связанные со страницей данные одним запросом.

* * *

На этом автор оригинального поста остановился, но я решил копнуть немного глубже
в поисках, чего бы еще оптимизировать. И вот что я нашел. Каждый объект класса
`NavigationNode` имеет свойство `id`, которое совпадает с `id` соответсвующей
страницы. Зная, что при использовании внешних ключей в БД Django на самом деле
создает колонку с суффиксом `_id`, где хранится, собственно, внешний ключ, мы
можем миновать выборку страницы и сразу обращаться к связанным с ней данным.
Код манкипатчинга `NavigationNode` примет следующий вид:

```python
# app/models.py
from menus.base import NavigationNode
NavigationNode.avatars = property(lambda p: PageAvatars.objects.get(page_id=p.id))
```

Это должно замечательно работать. Но в некоторых случаях необходимо получить
доступ к свойстам страницы непосредственно (дата публикации или создания,
например) - тогда нужно использовать предыдущий вариант.



[1]: http://ilian.i-n-i.org/extending-django-cms-page-model/
[2]: https://github.com/divio/django-cms/issues/1412
[defer]: https://docs.djangoproject.com/en/dev/ref/models/querysets/#defer
[only]: https://docs.djangoproject.com/en/dev/ref/models/querysets/#only
[select_related]: https://docs.djangoproject.com/en/dev/ref/models/querysets/#select-related
