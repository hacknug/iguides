---
layout: index
title: Django, Nginx, Uwsgi [и Virtualenv]
---

Не то что бы я кучу времени тратил на это, но порой было весьма неудобно и
муторно поднять django в виртуальном окружении. Делал это однажды, юзая
passenger, приходилось создавать `passenger_wsgi.py` в директории проекта,
чудить по всякому...

Через `uwsgi` все гораздо проще и приятней. Для связки с `nginx` достаточно
всего двух файлов конфигурации!

Устанавливаем:

```bash
sudo apt-get install nginx uwsgi uwsgi-plugin-python
```

Переходим в `/etc/uwsgi/apps-available/`. Создаем `sitename.ini` с таким
содержанием:

```ini
; /etc/uwsgi/apps-available/sitename.ini
[uwsgi]
plugins = python27
chdir = /home/user/sitename/
pythonpath = ..
; это для 1.3, для 1.4 изменить на нужный путь импорта
; (как правило, sitename.wsgi:application, тогда и надобность в env отпадает)
env = DJANGO_SETTINGS_MODULE=settings
module = django.core.handlers.wsgi:WSGIHandler()

; при необходимости - путь к окружению
virtualenv = /home/user/.virtualenvs/someenv/
```

Делаем линк во "включенные" приложения: `sudo ln -s sitename ../apps-enabled/`.

Теперь - энджинкс. Переходим в `/etc/nginx/sites-available/`. Создаем, опять же,
`sitename` с примерно таким содержимым:

```nginx
# /etc/nginx/sites-available/sitename
server {
    listen 80;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:///var/run/uwsgi/apps/sitename/socket;
    }

    location ^~ /static/ {
        alias /home/user/sitename/public/static/;
        expires max;
    }

    location ^~ /media/ {
        alias /home/user/sitename/public/media/;
        expires max;
    }
}
```

Далее - снова "включаем" настройки: `sudo ln -s sitename ../sites-enabled/`.

Рестартуем оба сервиса - все должно работать. Вуаля, как говорится!
