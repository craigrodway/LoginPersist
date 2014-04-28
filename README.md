#LoginPersist#

LoginPersist is a ProcessWire module to support the "Remember Me" functionality for users so that they can remain logged in across browser sessions.

This module follows industry best practise advice as documented here:

* [Persistent Login Cookie Best Practice, Charles Miller](http://fishbowl.pastiche.org/2004/01/19/persistent_login_cookie_best_practice/)
* [Improved Persistent Login Cookie Best Practice, Barry Jaspan](http://jaspan.com/improved_persistent_login_cookie_best_practice)

- - -

##Features##

* The module can operate in two ways:
	* Automatically. No code changes, but users do not have a choice.
	* Manually. The module must be called from a site's custom code.
* Options can be changed in the module configuration page.
* Enable fingerprinting (IP address and User Agent) as an additional security check.
* Limit the persistent login functionality by role.
* Set the name and age of the cookie.
* Sets an identifier in the session when a user is logged in via a persistent login cookie. This should be used to control access to sensitive information and actions within a site's custom code.
* Clears login tokens when a potential theft has been identified.


##Documentation##


###Automatic usage###

To set persistent logins for every user, automatically, ensure the _Automatic_ checkbox is set in the module configuration page.


###Manual usage###

To persist logins from a site's custom code, just call this module's `persist()` method once you have authenticated the user, and optionally checked for a "remember me" preference.

```php
$persist = wire('modules')->get('LoginPersist');
$persist->persist();
```

A persistent cookie will be given to the user. The next time the user visits the site, they will be logged in.


###Roles###

The persistent login cookies can optionally be limited to certain user roles. Choose the roles from the dropdown list in the module's configuration page and only users who have those roles will be given persistent cookies. If no roles are selected, there is no restriction - persistent login cookies will be given to anyone.


##Changelog##

1.0.0
* Initial release