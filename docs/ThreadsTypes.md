Threads types
=============


All user-published content in Misago follows basic hierarchy: Categories have threads which have (optionally) polls and posts that have attachments.

Whenever user creates new discussion in category, sends private message to other user, or reports offensive post, mechanics behind UI are largery the same and result in thread of certain kind being posted in destined section of site.

This system was designed to be extensible to enable developers using Misago as foundation for their community sites to add new content types such as blogs, articles or galleries.


## Writing custom thread types

Thread type is basically UI for users to interact with and Python code implementing features behind it, plus strategy object for customizing specific behaviours like link building.

Thread type is decided by value of `special_role` attribute of category model instance that content (thread, post, attachment, etc. ect.) belongs to. Using this value model is able to call `misago.threads.threadtypes.get(thread_type)` in order to obtain its strategoy object.


## Type objects

Paths to type objects definitions are specified in `MISAGO_THREAD_TYPES` settings. Each type class is expected to define `type_name` attribute corresponding to its category's `special_role` attribute.

Once strategy class is defined, it's available as `thread_type` attribute on category, thread and post models.

Depending on features used by thread type, its strategy is expected to define different of the following methods:


##### Note

If `special_role` is not defined, Misago falls back to `role` attribute.


### `get_category_name(category)`

Used to obtain category name. Useful when thread type uses single category with predefined name. Should return string.


### `get_category_absolute_url(category)`

Used to obtain category absolute url.


### `get_new_thread_url(category)`

Used to obtain "post new thread" url for category.


### `get_reply_url(thread)`

Used to obtain "post reply" url for thread.


### `get_edit_post_url(post)`

Used to obtain edit url for post.


### `get_thread_absolute_url(thread)`

Used to obtain thread absolute url.


### `get_thread_post_url(thread, post_id, page)`

Used by "go to post" views to build links pointing user to posts. Should return URL to specified thread page with fragment containing specified post.


### `get_thread_last_reply_url(thread)`

Should return url to view redirecting to last post in thread.


### `get_thread_new_reply_url(thread)`

Should return url to view redirecting to first unread post in thread.


### `get_thread_moderated_url(thread)`

Should return url to view returning list of posts in thread that are pending moderator review.


### `get_thread_reported_url(thread)`

Should return url to view returning list of reported posts in thread.


### `get_post_absolute_url(post)`

Used to obtain post absolute url.


### `get_post_approve_url(post)`

Used to obtain url that moderator should follow to approve post.


### `get_post_unhide_url(post)`

Used to obtain url that will make hidden post visible.


### `get_post_hide_url(post)`

Used to obtain url that will make visible post hidden.


### `get_post_delete_url(post)`

Used to obtain url that will delete post.


### `get_post_report_url(post)`

Used to obtain url for reporting post.


### `get_event_edit_url(event)`

Used to obtain url that will handle API calls for hiding/unhiding and deleting thread events.