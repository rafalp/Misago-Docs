Database Settings
=================

Those settings are stored in database and are available via `misago.conf.settings`. To change those settings you need to use admin control panel.


## account_activation

Preffered way in which new user accounts are activated. Can be either of those:

* `none` - no activation required.
* `user` - new user has to click link in activation e-mail.
* `admin` - board administrator has to activate new accounts manually.
* `block` - turn new registrations off.


## allow_custom_avatars

Controls if users may set avatars from outside forums.


## avatar_upload_limit

Max allowed size of uploaded avatars in kilobytes.


## default_avatar

Default avatar assigned to new accounts. Can be either `initials` for randomly generated pic with initials, `gravatar` or `gallery` which will make Misago pick random avatar from gallery instead.


## default_timezone

Default timezone used by guests and newly registered users that haven't changed their timezone prefferences.


## forum_addess

Complete HTTP address to your Misago site homepage. Misago relies on this address to create links in e-mails that are sent to the site users.


## forum_name

Forum name, displayed in titles of pages.


## google_site_verification

Site verification token used to create Google Search Console verification page.


## google_tracking_id

Google Analytics Tracking ID to include in generated HTML.


## index_header

Text to place in forum index header. Can be empty string if not set, in which case `forum_name` should be used instead.


## index_meta_description

Forum index Meta Description used as value meta description attribute on forum index.


## index_title

Forum index title. Can be empty string if not set, in which case `forum_name` should be used instead.


## logo

Logo image displayed in forum's navbar.


## logo_small

Small logo image displayed in forum's compact navbar. When set, it's used instead icon for menu item pointing to forum index.


## logo_text

Text displayed in forum's navbar. This text is never included in compact navbar used on small displays.


## post_length_max

Maximal allowed post content length.


## post_length_min

Minimal allowed post content length.


## signature_length_max

Maximal allowed length of users signatures.


## subscribe_reply

Default value for automatic subscription to replied threads prefference for new user accounts. Its value represents one of those settings:

* `no` - don't watch.
* `watch` - put on watched threads list.
* `watch_email` - put on watched threads list and send e-mail when somebody replies.


## subscribe_start

Default value for automatic subscription to started threads prefference for new user accounts. Allows for same values as `subscribe_reply`.


## thread_title_length_max

Maximal allowed thread title length.


## thread_title_length_min

Minimal allowed thread title length.


## username_length_max

Maximal allowed username length.


## username_length_min

Minimal allowed username length.

