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


## attachment_403_image

Custom image served by the attachments proxy when user has no permission to see the attachment.


## attachment_404_image

Custom image served by the attachments proxy when attachment does not exist.


## avatar_upload_limit

Max allowed size of uploaded avatars in kilobytes.


## blank_avatar

Custom blank avatar image.


## daily_post_limit

Daily limit of posts that may be posted from single account. Fail-safe for situations when forum is flooded by spam bots. Change to 0 to lift this restriction.


## default_avatar

Default avatar assigned to new accounts. Can be either `initials` for randomly generated pic with initials, `gravatar` or `gallery` which will make Misago pick random avatar from gallery instead.


## default_timezone

Default timezone used by guests and newly registered users that haven't changed their timezone prefferences.


## events_per_page

Misago reads events to display in separate database query to avoid situation when thread with large number of eg. moderator actions displays pages consisting exclusively of events. Using this setting you may specify upper limit of events displayed on thread's single page. This setting is intended as fail safe, both to save threads from excessively long lists of events your users will have to scroll trough, as well as to keep memory usage within limts.

In case of more events than specified being found, oldest events will be truncated.


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


## og_image

User uploaded Open Graph image that will be used instead of default one.


## og_image_avatar_on_profile

Controls if user avatar should be used as OG image for user profiles.


## og_image_avatar_on_thread

Controls if thread starter's avatar should be used as OG image for threads. If thread starter's account was deleted, default OG image will be used instead.


## post_attachments_limit

Limit of attachments per single post.


## post_length_max

Maximal allowed post content length.


## post_length_min

Minimal allowed post content length.


## posts_per_page

Controls number of posts displayed on thread page. Greater numbers can increase number of objects loaded into memory and thus depending on features enabled greatly increase memory usage.


## posts_per_page_orphans

Defines minimal number of posts for thread's last page. If number of posts on last page is smaller or equal to one specified in this setting, last page will be appended to previous page instead.


## readtracker_cutoff

Controls amount of data used by read tracking system. All content older than number of days specified in this setting is considered old and read, even if the opposite is true for the user. Active forums can try lowering this value while less active ones may wish to increase it instead. 


## threads_per_page

Controls number of threads displayed on page. Greater numbers can increase number of objects loaded into memory and thus depending on features enabled greatly increase memory usage.


## signature_length_max

Maximal allowed length of users signatures.


## stop_forum_spam

This settings allows you to decide whether or not [Stop Forum Spam](http://www.stopforumspam.com/) database should be used to validate IPs and emails during new users registrations.


## stop_forum_spam_confidence

Minimum confidence returned by [Stop Forum Spam](http://www.stopforumspam.com/) for Misago to reject new registration and block the IP address for 1 day.


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


## top_posters_ranking_length

This setting controls maximum age in days of posts that should count to the top posters ranking.


## top_posters_ranking_size

Maximum number of users in top posters ranking.


## unused_attachments_lifetime

Period of time (in hours) after which user-uploaded file that wasn't attached to any post is deleted.


## username_length_max

Maximal allowed username length.


## username_length_min

Minimal allowed username length.


## users_per_page

Controls pagination of users lists.


## users_per_page_orphans

Value of `orphans` argument used in pagination.
