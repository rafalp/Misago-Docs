Context processors
==================

Context Processors are simple python functions that receive HttpRequest object and extend template context with additional values. In addition to [default context processors defined by Django](https://docs.djangoproject.com/en/dev/ref/templates/api/#subclassing-context-requestcontext), Misago defines its own context processors:


## `misago.core.context_processors.frontend_context`

Exposes `frontend_context` to templates, allowing you to JSON serialize and pass it to JavaScript frontend:

```
{% load misago_json %}

<script type="text/javascript">
  const context = {{ frontend_context|as_json }};
  misago.init(context);
</script>
```


## `misago.core.context_processors.request_path`

This function adds `request_path` value to template context that you can use together with `absoluteurl` tag to build absolute links to current page in your templates:

```
{% load misago_absoluteurl %}
# Will become "http://mysite.com/users/"
{% absoluteurl request_path %}
```
