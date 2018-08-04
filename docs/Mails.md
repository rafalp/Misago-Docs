Sending mails
=============


Misago provides its own API for sending e-mails to forum users thats layer of abstraction above the default [Django mailer](https://docs.djangoproject.com/en/dev/topics/email/).

This API lives in `misago.core.mail` and provides following functions:


## `build_mail`(recipient, subject, template, sender=None, context=None)

Build e-mail message using supplied template name and (optionally) context. Template name shouldn't contain file extension, as Misago will automatically append `.html` for html content and `.txt` for plaintext content of the message.

* `subject` - A string.
* `template` - A string.
* `sender` - User model instance for message sender.
* `context` - The optional dictionary with extra context values that should be used in template render.

Email templates are always passed following context values:

* `SITE_ADDRESS` - The site address set in `MISAGO_ADDRESS` setting.
* `LANGUAGE_CODE` - Valid language code that can be used as value for `<html>` `lang` attribute.
* `LOGIN_URL` - The URL to login view specified in the `LOGIN_URL` setting.
* `user` - User model instance representing the message recipient.
* `sender` - If specified, instance of User model representing the message senderm eg. user that performed the action that triggered e-mail to be sent.
* `subject` - Subject of message being sent.
* `misago_settings` - Dict-like object enabling access to misago's running configuration.


## mail_user(recipient, subject, template, sender=None, context=None)

Shortcut function that calls `build_mail` to build message, then sends it to user.

* `request` - HttpRequest object instance.
* `recipient` - User model instance.
* `subject` - A string.
* `template` - A string.
* `sender` - User model instance for message sender.
* `context` - The optional dictionary with extra context values that should be available for message templates.


## mail_users(recipients, subject, template, sender=None, context=None)

Same as `mail_user`, but instead of sending message to single recipient, it sends it to many recipients at same time. Keep on mind this may be memory intensitive as this function creates one Mail object instance for every recipient specified, so you may want to batch the sending process.

* `recipients` - Iterable of User models.
* `subject` - A string.
* `template` - A string.
* `sender` - User model instance for message sender.
* `context` - The optional dictionary with extra context values that should be available for message templates.