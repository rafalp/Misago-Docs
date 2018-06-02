Validating registrations
========================

Misago implements simple framework for extending process of new user registration with additional checks.

When user submits registration form, within its `clean` method form calls functions defined in `MISAGO_NEW_REGISTRATIONS_VALIDATORS` setting.

Each function is called with following arguments:

* `request` - current HttpRequest instance. You may use it to obtain IP address using form, via `request.user_ip` attribute.
* `cleaned_data` - dict containing cleaned data. Use it too look up user's input that you wish to validate.
* `add_error` - `None` or [`add_error`](https://docs.djangoproject.com/en/{{ book.django_version }}/ref/forms/api/#django.forms.Form.add_error) function accepting fieldname and list of errors. Allows you to set validation errors on field.

If function decides to interrup registration process and thus stop user from registering account, it can raise either `django.core.exceptions.ValidationError`, `django.core.exceptions.PermissionDenied`, or set errors on form via `form.add_error` utility.

This validation framework provides you with plenty of power, for example allowing you to write custom validator that queries database for number of registrations that occured for specific IP in last 8 hours, and banning both users and IP address via  `ban_ip` or `ban_user` utils from `misago.users.bans` module.