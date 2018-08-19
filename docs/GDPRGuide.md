GDPR Compliance Guide
=====================

The General Data Protection Regulation is an EU regulation concerning the processing of EU residents data.

For reference, entire regulation can be read [on the EUR-Lex](https://eur-lex.europa.eu/eli/reg/2016/679/oj).


## Data is a burden

Make sure you keep as little of what can be considered personal data, as possible. 

Data processing starts with user registering on your site. Even if they failed to activate their account, you are now processing their e-mail address, username and IP address. Make sure you are peridically deleting old IP address and inactive user accounts - use the `removeoldips` and `deleteinactiveusers` management commands or run them in your `cron` to prevent amount of data processed growing uncontrollably.

Because user data embed in posts is impossible to delete or anonymize automatically, update your forum rules to advise users against posting photos, addresses or other personal data outside of their profiles.


## Automation vs administrator action

GDPR doesn't require the site to provide options for users to exert their rights. For example, you don't have to provide a feature in user control panel enabling user to delete their account if you don't want to. "To delete your account please contact the administrator" in your privacy is good enough, under the condition that you will delete user's account after reach out to you with such request.


## Write a Privacy policy

Articles 12 to 14 of GDPR require site administrators to inform the users about following:

- Who is data administrator on the site, where are they located and how can they be contacted.
- What kind of personal data is being processed by the site, why, for how long, who has access to it and how is it being protected.
- That they have the right to know what data about you is being held and processed.
- That they have the right to rectify any inaccurate personal data.
- That they have the right to be forgotten.
- That they have the right to restriction of processing.
- That they have the right to object to processing.

You should also inform the user that your site uses cookies to provide the log in feature and secure the submiting of forms.


## Right of access

Paragraph 3 of article 15 grants site users right to receive the copy of their personal data being processed. You can enable users to do this themselves using the `MISAGO_ENABLE_DOWNLOAD_OWN_DATA` setting or direct them to site administrator, who can trigger data downloads for them from admin control panel.


## Right to be forgotten or object

Articles 17, 18 and 21 grant your users a right to delete and anonymize their data.

Admin control panel provides an option for administrator to delete only user account, or user account together with content associated with it such as threads, posts or attachments. If you wish to, you may enable users to delete their accounts using the option on their options page. Users may also delete their accounts when new agreement becomes active. In case when user account is deleted by user, their posted content is left behind.

When user account is deleted, Misago attempts to automatically anonymize any user data left behind, but its unable to delete/anonymize user embed in user posts.
