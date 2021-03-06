Static settings reference
=======================

This is the reference of all [static settings](./README.md#static-settings) defined by Misago. Reference of settings defined by Django can be found on [Django documentation](https://docs.djangoproject.com/en/{{ book.django_version }}/ref/settings/).


## `MISAGO_ACL_EXTENSIONS`

List of Misago ACL framework extensions.


##### Note

On Misago admin panel home page you will find a message telling you if you have entered the correct value, or what value is correct in case you've didn't.


## `MISAGO_ADMIN_NAMESPACES`

Link namespaces that are administrator-only areas that require additional security from Misago. Users will have to re-authenticate themselves to access those namespaces, even if they are already signed in your frontend. In addition they will be requested to reauthenticated if they were inactive in those namespaces for certain time.

Defautly `misago:admin` and `admin` namespaces are specified, putting both Misago and Django default admin interfaces under extended security mechanics.


## `MISAGO_ADMIN_PATH`

Path prefix for Misago administration backend. Defautly "admincp", but you may set it to empty string if you with to disable your forum administration backend.


## `MISAGO_ADMIN_SESSION_EXPIRATION`

Maximum allowed lenght of inactivity period between two requests to admin namespaces. If its exceeded, user will be asked to sign in again to admin backed before being allowed to continue activities.


## `MISAGO_ATTACHMENT_403_IMAGE_IMAGE`

Default file that should be served if user has no permission to see requested attachment.


## `MISAGO_ATTACHMENT_404_IMAGE_IMAGE`

Default file that should be served if user has requested nonexistant attachment.


## `MISAGO_ATTACHMENT_IMAGE_SIZE_LIMIT`

Max dimensions (width and height) of user-uploaded images embedded in posts. If uploaded image is greater than dimensions specified in this settings, Misago will generate thumbnail for it.


##### Note

Because user-uploaded GIF's may be smaller than dimensions specified, but still be considerably heavy due to animation, Misago always generates thumbnails for user-uploaded GIFS, stripping the animations from them.


## `MISAGO_ATTACHMENT_SECRET_LENGTH`

Length of attachment's secret (filenames and url token). The longer, the harder it is to bruteforce, but too long may conflict with your uploaded files storage limits (eg. filesystem path length limits).


##### Warning

In order for Misago to support clustered deployments or CDN's (like Amazon's S3), its unable to validate user's permission to see the attachment at its source. Instead it has to rely on exessively long and hard to guess urls to attachments and assumption that your users will not "leak" source urls to attachments further.

Generaly, neither you nor your users should use forums to exchange files containing valuable data, but if you do, you should make sure to secure it additionaly via other means like password-protected archives or file encryption solutions.


## `MISAGO_AVATAR_GALLERY`

Path to directory containing avatar galleries. Those galleries can be loaded by running `loadavatargallery` command.

Feel free to remove existing galleries or add your own.

If you create gallery named `__default__` and set avatar gallery as default user avatar, Misago will select new users avatars from it while keeping this gallery hidden from existing users.


## `MISAGO_AVATARS_SIZES`

Misago uses avatar cache that prescales avatars to requested sizes. Enter here sizes to which those should be optimized.


##### Warning

It's impossible to regenerate user avatars store for existing avatars. Misago comes with sane defaults for avatar sizes, with min. height for user avatar being 400 pixels square, and steps of 200, 150, 100, 64, 50 and 30px. However if you need larger avatar or different pregenerated dimensions, changing those will require you to manually remove `avatars` directory from your media storage as well as running `misago.users.avatars.set_default_avatar` function against every user registered.


## `MISAGO_BLANK_AVATAR`

This path to static image file that Misago should use as blank avatar. Defaults to `misago/img/blank-avatar.png`.


## `MISAGO_BLEACH_CALLBACKS`

Custom callbacks for `bleach` to customize automatic links substitution with `bleach.linkify`.


## `MISAGO_DYNAMIC_AVATAR_DRAWER`

Function used to create unique avatar for this user. Allows for customization of algorithm used to generate those.


## `MISAGO_LOGIN_API_URL`

URL to API endpoint used to authenticate sign-in credentials. Musn't contain api prefix or wrapping slashes. Defaults to 'auth/login'.


## `MISAGO_MARKUP_EXTENSIONS`

List of python modules extending Misago markup.


## `MISAGO_MOMENT_JS_LOCALES`

List of Moment.js locals available.


## `MISAGO_NEW_REGISTRATIONS_VALIDATORS`

List of functions to be called when somebody attempts to register on forums using registration form.


## `MISAGO_POST_SEARCH_FILTERS`

List of post search filters that are used to normalize search queries and documents used in forum search engine.


## `MISAGO_POST_VALIDATORS`

List of post validators used to validate posts.


## `MISAGO_POSTING_MIDDLEWARES`

List of middleware classes participating in posting process.


## `MISAGO_PROFILE_FIELDS`

Defines list of groups of profile fields used on users profiles "details" tab.

Should be list of dicts defining `name` key that's string with name of fields group and `fields` that is list of paths to Python types implementing fields: 

    MISAGO_PROFILE_FIELDS = [
        {
            'name': _("Personal"),
            'fields': [
                'misago.users.profilefields.default.FullNameField',
                'misago.users.profilefields.default.GenderField',
            ],
        },
        {
            'name': _("Contact"),
            'fields': [
                'misago.users.profilefields.default.TwitterHandleField',
                'misago.users.profilefields.default.WebsiteField',
            ],
        },
    ]


## `MISAGO_SEARCH_CONFIG`

PostgreSQL text search configuration to use in searches. Defaults to "simple", for list of installed configurations run "\dF" in "psql".

Standard configs as of PostgreSQL 9.5 are: `dutch`, `english`, `finnish`, `french`, `german`, `hungarian`, `italian`, `norwegian`, `portuguese`, `romanian`, `russian`, `simple`, `spanish`, `swedish`, `turkish`.

Example on adding custom language can be found [here](https://github.com/lemonskyjwt/plpstgrssearch).


##### Note

Items in Misago are usually indexed in search engine on save or update. If you change search configuration, you'll need to rebuild search for past posts to get reindexed using new configuration. Misago comes with `rebuildpostssearch` tool for this purpose.


## `MISAGO_SLUGIFY`

Path to function or callable used by Misago to generate slugs. Defaults to `misago.static.slugify.default`. Use this function if you want to customize slugs generation on your community.


## `MISAGO_THREADS_ON_INDEX`

Change this setting to `False` to display categories list instead of threads list on board index.


## `MISAGO_THREAD_TYPES`

List of clasess defining thread types.


## `MISAGO_USER_DATA_DOWNLOADS_WORKING_DIR`

Path to the directory that Misago should use to prepare user data downloads. Should not be accessible from internet.
