# LoginPersist

LoginPersist is a [ProcessWire](https://processwire.com/) module to support the "Remember Me" functionality for users so that they can remain logged in across browser sessions.

The module follows recommendations documented here:

* [Persistent Login Cookie Best Practice, Charles Miller](http://fishbowl.pastiche.org/2004/01/19/persistent_login_cookie_best_practice/)
* [Improved Persistent Login Cookie Best Practice, Barry Jaspan](https://web.archive.org/web/20160716075951/http://jaspan.com/improved_persistent_login_cookie_best_practice)

## Features

* The module can operate in two ways:
	* Automatically. No code changes, but users do not have a choice.
	* Manually. The module must be called from a site's custom login process code.
* Options can be changed in the module configuration page.
* Enable fingerprinting (IP address and User Agent) as an additional security check.
* Limit the persistent login functionality by role.
* Set the name and age of the cookie.
* Sets an identifier in the session when a user is logged in via a persistent login cookie. This should be used to control access to sensitive information and actions within a site's custom code.
* Clears login tokens when a potential theft has been identified.


## Documentation

This functionality is provided by setting a new cookie for user after successfully logging in.

The next time the user visits the site and their standard PW session had naturally expired, and are therefore treated as a 'guest' user, they will be logged in via the persistent cookie.

When a user logs out (triggered with the hook `Session::logoutSuccess`) the user's persistent cookie will be removed, along with the corresponding entries in the database.


### Automatic mode

When Automatic mode is enabled (by setting the __Automatic__ checkbox on the Settings page) any user successfully logging in will be given a persistent login cookie.

This is handled via a hook to `Session::loginSuccess`.


### Manual mode

In manual mode, you must call a module function yourself to create the persistent login cookie.

For example, this can be in response to a 'Remember Me' checkbox value on a login form being checked by the user, indicating their preference to remain logged in.

You can use any of the below methods once you have authenticated the user attempting to log in:

**(1)** Call the hooked method `persist()` on ProcessWire `Session` class. This means you don't have to load and reference this module yourself.

```php
wire('session')->persist();
```

**(2)** Call the `persist()` method on this module.

```php
$persist = wire('modules')->get('LoginPersist');
$persist->persist();
```


### Roles

The persistent login cookies can optionally be limited to certain user roles. This is applicable to both automatic and manual modes.

Choose the desired roles from the dropdown list in the module's configuration page. When selected, only users who have any of those roles (via `$user->hasRole()`) will be given persistent cookies. If no roles are selected, persistent login cookies will be given to all users on successful login.
