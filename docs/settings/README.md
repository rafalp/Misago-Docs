Settings
========

Misago settings belong to one of two groups:


## Dynamic settings

Those settings are stored in database and can be changed at the runtime using interface provided by admin control panel.

[Database settings reference](./Database.md)


### Accessing dynamic settings in python code

Dynamic settings can be accessed through the `settings` attribute on `request`:

```python
def my_view(request):
    # get forum name
    request.settings.forum_name
```

This method is available only during http request life-cycle. If you need to access dynamic settings outside of it (eg. in management command), you will have to instantiate `misago.conf.dynamicsettings.DynamicSettings` manually:

```python
from django.core.management.base import BaseCommand

from misago.conf.dynamicsettings import DynamicSettings


class Command(BaseCommand):
    def handle(self, *args, **options):
        settings = DynamicSettings()
        self.stdout.write("Forum name: %s" % settings.forum_name)
```


### Accessing dynamic settings in template

Dynamic settings object is made available to the templates as `settings` variable:

```
<title>{{ settings.forum_name }}</title>
```


### Lazy settings

Not all dynamic settings settings are available at all times. Some settings (called lazy settings), are only available as `True` or `None` when accessed. To obtain their "real" value you need to call the `get_lazy_setting_value` method on settings object:

```python
def my_view(request):
    # test if lazy setting has value
    if request.settings.some_lazy_setting:
        # get actual value of the setting
        lazy_setting_value = request.settings.get_lazy_setting_value("some_lazy_setting")
```


## Core settings

Those settings must be available when Misago starts and are not changeable from admin control panel.

To define or change those settings, you need to edit your site's `settings.py`

[Core settings reference](./Core.md)


### Accessing core settings in python code

Core settings are available as attributes on `settings` object importable from `misago.conf.settings`:

```python
from misago.conf import settings

if settings.MISAGO_USE_STOP_FORUM_SPAM:
    print("Misago is configured to use stop forum spam!")
```

`misago.conf.settings` first attempts to read setting value in your site's `settings.py`, and when its not defined here, uses default value defined inside Misago source code.

`misago.conf.settings` is fully compatible with `django.conf.settings`. If your code is not accessing `MISAGO_*` settings, you can use either of those approaches for accessing core configuration. `misago.conf.settings` is only recommended when you are using `MISAGO_*` settings in your code.


### Django Settings Reference

Django defines plenty of configuration options that control behaviour of different features that Misago relies on.

Those are documented and available in [Django documentation](https://docs.djangoproject.com/en/{{ book.django_version }}/ref/settings/).
