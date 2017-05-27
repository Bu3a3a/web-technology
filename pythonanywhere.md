## PythonAnywhere

[PythonAnywhere](https://www.pythonanywhere.com/) позволяет разворачивать программы, написанные на языке Python. Если у вас возникнут какие-то сложности, посмотрите/ нет ли ответа на ваш вопрос в [помощи](https://help.pythonanywhere.com/pages/) или на [форуме](https://www.pythonanywhere.com/forums/).

### Разворачивание существующего проекта Django на PythonAnywhere

1. git clone https://github.com/myusername/myproject.git
2. mkvirtualenv --python=/usr/bin/python3.6 mysite-virtualenv
3. pip install -r requirements.txt
4. python manage.py migrate
5. Создание веб-приложения на PythonAnywhere:
    1. Перейдите на вкладку Web и создайте Web app с ручной конфигурацией (Manual Configuration) и нужной версией Python
    2. В разделе "Виртуальное окружение" (Virtualenv) укажите название виртуального окружения разворачиваемого проекта (не обязательно указывать полный путь до папки с ним, достаточно названия)
    3. Настройте WSGI в разделе Code (поправьте path и DJANGO_SETTINGS_MODULE и сохраните):
    ```python
    # +++++++++++ DJANGO +++++++++++
    # To use your own Django app use code like this:
    import os
    import sys

    # assuming your Django settings file is at '/home/myusername/mysite/mysite/settings.py'
    path = '/home/myusername/mysite'
    if path not in sys.path:
        sys.path.append(path)

    os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'

    ## Uncomment the lines below depending on your Django version
    ###### then, for Django >=1.5:
    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
    ###### or, for older Django <=1.4
    #import django.core.handlers.wsgi
    #application = django.core.handlers.wsgi.WSGIHandler()
    ```
    4. Настройте пути к статическим файлам в разделе Static files
    5. Перезапустите веб-приложение (кнопка Reload  в разделе Reload) и всё должно заработать!
    
### Бот для Телеграм с PythonAnywhere

https://blog.pythonanywhere.com/148/

Главное из статьи: для работы сбесплатного аккаунта с API Telegram необходимо сконфигурировать прокси:

```python
import telepot

# You can leave this bit out if you're using a paid PythonAnywhere account
proxy_url = "http://proxy.server:3128"
telepot.api._pools = {
    'default': urllib3.ProxyManager(proxy_url=proxy_url, num_pools=3, maxsize=10, retries=False, timeout=30),
}
telepot.api._onetime_pool_spec = (urllib3.ProxyManager, dict(proxy_url=proxy_url, num_pools=1, maxsize=1, retries=False, timeout=30))
# end of the stuff that's only needed for free accounts

bot = telepot.Bot(YOUR_AUTHORIZATION_TOKEN)
```

    
    



