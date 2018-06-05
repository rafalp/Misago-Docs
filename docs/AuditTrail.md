Audit trail
===========

Misago implements an [audit trail](https://en.wikipedia.org/wiki/Audit_trail) feature that records user IP address on certain actions. This mechanism can be used for few purposes:

- enables site staff to find and block IP addresses associated with abusive users
- makes it easier to detect fraudent or deceptive activity like [sockpuppetry](https://en.wikipedia.org/wiki/Sockpuppet_(Internet))


Creating audit trail
--------------------

The audit trail has been designed to be easy for developers to use in their code. In fact only things needed to create new audit trail are `HttpRequest` instance for signed in user and any `Model` instance.

Here is example code snippet that creates new audit trail connecting currently signed in user and some forum thread:

```python
from misago.users.audittrail import create_audit_trail

def some_view(request):
    # code doing something with thread that we want to create audit trail for
    
    create_audit_trail(request, thread)

    # create some response and send it to client after our work is done
    return response
```

Note that you don't need to check for `request.user.is_authenticated` before using the `create_audit_trail`. If user is not signed in, the `create_audit_trail` will not create audit trail and will return `None`.


GDPR concerns
-------------

In light of GDPR the IP address and timestamp pair is considered personal data. Your privacy policy should inform your users that your site registers their IP address for purpose of protecting it from abuse, who has access to IP addresses registered and how long is this data stored.

Audit trails respect the `MISAGO_IP_STORE_TIME` setting, and are deleted by the `deleteoldips` management command if they are older than set store time.