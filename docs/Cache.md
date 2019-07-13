Cache
=====

Misago uses caching aggressively to save and re-use results of costful operations like users ACLs resolution.

Setting up cache - ideally memory based one like Redis - is great way to speed up your forum and cut down database traffic.

> **Note:** If you are using Misago-Docker, this is already done without any additional configuration.


## Cache versioning

The `misago.cache.versions` module implements cache versioning feature. Cache version is string made of 8 random characters that should be included in cache key, or as value of `version` named argument on `cache.set` and `cache.get`.

Current cache versions are available under the `cache_versions` attribute on `request`. It's set by `misago.cache.middleware.cache_versions_middleware` middleware.


## Invalidating cache

If you wish to invalidate certain versioned cache, you may do it with `misago.cache.versions.invalidate_cache` function:

```python
from misago.cache.versions import invalidate_cache


# Invalidates "bans" cache after creating new ban
invalidate_cache("bans")
```


## Invalidating all versioned caches

Misago provides two ways for invalidating all versioned caches.


### Invalidating all versioned caches from python code

All versioned caches can be invalidated by calling `misago.cache.versions.invalidate_all_caches`:

```python
from misago.cache.versions import invalidate_all_caches

invalidate_all_caches()
```


### Invalidating all versioned caches using management command

Misago has management command that can be executed from console to invalidate all versioned caches:

```
python manage.py invalidateversionedcaches
```


## Versioning new caches

`misago.cache.operations` module provides migration operations that allow you to easily add or remove cache versioning:


```python
from django.db import migrations

from misago.cache.operations import StartCacheVersioning, StopCacheVersioning


class Migration(migrations.Migration):
    dependencies = [
        ('misago_cache', '0001_initial'),
    ]

    operations = [
        # Starts cache versioning for "my_cache"
        StartCacheVersioning("my_cache"),
        # Stop cache versioning "my_cache"
        StopCacheVersioning("my_cache"),
    ]
```

In order for your migration to work, you need to declare dependency on `misago_cache` migration no. `0001_initial`.

