Permissions framework
=====================

Misago brings its own ACL (Access Control Lists) framework for implementing permissions. This document explains to how to use and extend it with your own permissions.


## User permissions

Permissions are stored on special models named "roles" and assigned to users directly and trough ranks. Guest users always have permissions from "Guest" role, and users always have permissions form "Member" role.

During the http request lifecycle Misago uses the `user_acl_middleware` middleware to set `user_acl` property on the `request`. This `user_acl` property is plain dictionary that contains user acl as well as additional data useful for permission checks:

- `user_id` - ID of the user this `user_acl` belongs to.
- `cache_versions` - copy of cache versions used to create this `user_acl`.
- `is_authenticated`
- `is_anonymous`
- `is_staff`
- `is_superuser`

To obtain `user_acl` outside of http request, you will need to use `get_user_acl` function from `misago.acl.useracl` module, and `get_cache_versions` function from `misago.cache.versions` to obtain cache versions:

```python
from django.conf.auth import get_user_model
from misago.acl.useracl import get_user_acl
from misago.cache.versions import get_cache_versions

User = get_user_model()
user = User.objects.get(username="MyUser")

cache_versions = get_cache_versions()
user_acl = get_user_acl(user, cache_versions)
```


### Permissions cache

Construction of User's ACLs can be costful process, especially once you start installing extensions adding new features to your site. Because of this, Misago is not assigning ACLs to Users, but to combinations of roles. Each user has "ACL key" assigned that allows Misago to associate this user with valid ACL cache, and reuse acls for users with same roles.

ACLs are cached and only rebuild when data affecting ACLs changes. Misago also provides simple utility for clearing all ACL caches:

```python
from misago.acl import cache

cache.clear()
```


## Checking permissions

Simplest way to check if user has permission to do something is to look up the appropriate key on their `user_acl`:

```python
if not user_acl["can_have_signature"]:
    raise PermissionDenied("You don't have permission to set a signature.")
```

ACL entries may be of different type. For example `categories` entry is dict of dicts:

```python
if user_acl['categories'].get(category.pk, {}).get('can_see'):
    # We can see category!
```

If you have object instance, you may annotate it using the `add_acl_to_obj` utility function from `misago.acl.objectacl`. Doing so will create an `acl` property on the object for you to instrospect its permissions easily:

```python
if category.pk in user_acl['visible_categories']:
    # Not really shorter, but simpler to remember and works in django templates!

from misago.acl import add_acl

add_acl_to_obj(user_acl, categories)
for category in categories
    if category.acl['can_see']:
        # Now model instances in categories queryset are aware of their ACLs!
        # ACLs are easy to check in templates too now!
```

In templates user acl is also made available as `user_acl` variable:

```
{% if user_acl.can_search %}
    {% include "search_form.html" %}
{% endif %}
```

Lastly, permission modules may expose permission checking functions:

```python
from misago.category.permissions import allow_browse_category:

# Raises PermissionDenied error with error message that can be shown to user
# if they don't have permission to browse the category
allow_browse_category(user_acl, category)
```

Because ACL framework is very flexible, different features can have different ways to check their permissions.

Misago adds `Misago User ACL` option to the Django Debug Toolbar menu. This page display user roles permissions as well as final ACL assigned to current request.


## Extending permissions system

ACL framework extensions are modules registered in `MISAGO_ACL_EXTENSIONS` setting. By convention, those modules are either named "permissions", or they are located in "permissions" package.

Misago checks module for following functions:


### `change_permissions_form(role)`

Required. This function is called when change permissions form for role is being build for view. It's expected to return Form type or none, if provider is not recognizing role type (eg. there is no sense in adding profiles visibility permissions to forums role form).


#### Note

Misago provides custom `YesNoSwitch` form field that renders nice "Yes/No" switch as input. This field is simple wrapper around `TypedChoiceField` that coerces to `int`. If you use use it for your permissions, make sure your ACL implementation handles their values as `1` or `0`, not as `True` or `False`, or your forms will break!

Make sure that all fields in your form have initial value, or your form will make tests suite fail because it will be unable to mock POST requests to admin forms correctly.


### `build_acl(acl, roles, key_name)`

Required. Is used in process of building new ACL. Its supplied dict with incomplete ACL, list of user roles and name of key under which its permissions values are stored in roles `permissions` attributes. Its expected to access roles `permissions` attributes which are dicts of values coming from permission change forms and return updated `acl` dict.


### `register_with(registry)`

Optional. Is called by providers registry after provider module was imported, to allow it to register annotators and serializers for ACL's. Receives only one argument:

* `registry` - istance of PermissionProviders that imported module.


## Registering Annotators and Serializers

When module's `register_with` function is called, its passed `PermissionProviders` instance that exposes following methods:


#### `acl_annotator(hashable_type, func)`

Registers `func` as ACL annotator for `hashable_type`.


#### `user_acl_serializer(func)`

Registers `func` as user ACL serializer. This function will be called with copy of `user_acl` (excluding the `cache_versions` key), and is expected to perform any required changes before ACL will be converted to JSON and sent to client. For example, an serializer simplifies the `categories` entry to only contain ACL for categories that are browseable by the user.


#### `get_type_annotators(obj)`

Returns list of annotators registered for type of `obj` or empty list is none exist.


#### `get_user_acl_serializers()`

Returns list of user acl serializers registered empty list is none exist.


### Annotators

Annotators are functions called when object is being made ACL aware. It always receives two arguments:

* `user` - user asking to make target aware of its ACL's
* `target` - target instance, guaranteed to be an single object, not list or other iterable (like queryset)


## Algebra

Consider those three simple permission sets:

```python
roles_permissions = (
    {'can_be_knight': False},
    {'can_be_knight': True},
    {'can_be_knight': False},
)
```

In order to obtain final ACL, one or more ACLs have to be sum together. Such operation requires loop over ACLs which compares values of dicts keys and picks preffered ones.

This problem can be solved using simple implementation:

```python
final_acl = {'can_be_knight': False}

for acl in roles_permissions:
    if acl['can_be_knight']:
        final_acl['can_be_knight'] = True
```

But what if there are 20 permissions in ACL? Or if we are comparing numbers? What if complex rules are involved like popular "greater beats lower, zero beats all"? This brings need for more suffisticated solution and Misago provides one in forum of `misago.acl.algebra` module.

This module provides utilities for summing two acls and supports three most common comparisions found in web apps:

* **greater**: True beats False, 42 beats 13
* **lower**: False beats True, 13 beats 42
* **greater or zero**: 42 beats 13, zero beats everything
* **lower non zero**: 13 beats 42, everything beats zero


### `sum_acls(result_acl, acls=None, roles=None, key=None, **permissions)`

This function adds ACLs to result_acl using set or rules provided as additional kwargs. Alternatively, it access iterable of roles and extension key.

Example usage is following:

```python
from misago.acl import algebra

user_acls = [
    {
        'can_see': 0,
        'can_hear': 0,
        'max_speed': 10,
        'min_age': 16,
        'speed_limit': 50,
    },
    {
        'can_see': 1,
        'can_hear': 0,
        'max_speed': 40,
        'min_age': 20,
        'speed_limit': 0,
    },
    {
        'can_see': 0,
        'can_hear': 1,
        'max_speed': 80,
        'min_age': 18,
        'speed_limit': 40,
    },
]

defaults = {
    'can_see': 0,
    'can_hear': 0,
    'max_speed': 30,
    'min_age': 18,
    'speed_limit': 60,
}

final_acl = algebra.sum_acls(
    defaults, acls=user_acls,
    can_see=algebra.greater,
    can_hear=algebra.greater,
    max_speed=algebra.greater,
    min_age=algebra.lower,
    speed_limit=algebra.greater_or_zero
)
```

As you can see because tests are callables, its easy to extend `sum_acls` support for new tests specific for your ACLs.