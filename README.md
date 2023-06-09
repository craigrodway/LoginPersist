# LoginPersist

By Craig A Rodway.

[![License: GPL v3](https://img.shields.io/static/v1?label=License&message=GPLv3&color=3DA639&style=flat-square)](https://www.gnu.org/licenses/gpl-3.0)
[![ProcessWire 3](https://img.shields.io/static/v1?label=ProcessWire&message=3.x&color=2480e6&style=flat-square&logo=processwire)](https://github.com/processwire/processwire)

LoginPersist is a module for the [ProcessWire CMS/CMF](https://processwire.com/) that provides "Remember Me" functionality for users so that they can remain logged in across browser sessions.

The module follows recommendations documented here:

* [Persistent Login Cookie Best Practice, Charles Miller](http://fishbowl.pastiche.org/2004/01/19/persistent_login_cookie_best_practice/)
* [Improved Persistent Login Cookie Best Practice, Barry Jaspan](https://web.archive.org/web/20160716075951/http://jaspan.com/improved_persistent_login_cookie_best_practice)


## Features

- Automatic or manual mode.
- Options can be changed in the module configuration page.
- Enable fingerprinting (IP address and User Agent) as an additional security check.
- Limit the persistent login functionality by role (enable or disable based on specific roles).
- Set the name and age of the cookie.
- Sets an identifier in the session when a user is logged in via a persistent login cookie. This should be used to control access to sensitive information and actions within your site's custom code.
- Clears login tokens when a potential theft has been identified.

### Automatic mode

This requires no code changes, and uses ProcessWire login hooks to run the persistent code. When logging in, users do not have a choice whether to remain logged in or not.

### Manual mode

You you have full control over when and where you call the module's persistence code. This allows you to provide users with the choice of staying logged in or not.


## Requirements

You will need the following to make use of this module:

- PHP 5.6.30 or newer
- ProcessWire 3.0.159 or newer


## Installing

### From the Modules Directory

1. Visit the Site > Modules page in your website's admin panel.
2. Click the 'New' tab.
3. Enter 'LoginPersist' in the 'Module Class Name' field and click 'Get Module Info'.
4. Click the 'Download Now' button.
5. Cick the 'Install Now' button.

### Manually

1. Place the module files in the `/site/modules/LoginPersist` directory.
2. Visit the Site > Modules page in your website's admin panel.
3. Press the 'Refresh' button.
4. In the 'New' tab, find LoginPersist in the list and click the 'Install' button.


## How it works

The 'stay logged in' functionality is provided by setting a new cookie for the user after they have successfully logged in.

The next time the user visits the site after their standard ProcessWire session has naturally expired, they will be treated as any other 'guest' user. This module checks for the presence of a persistent cookie belonging to a visitor, and will log them in.

When a user logs out (triggered with the hook `Session::logoutSuccess`) their persistent cookie will be removed, along with the corresponding entries in the database.


## Configuring

### General Settings

#### Automatic mode

Enable or disable the automatic mode. When a user logs in successfully, they will be given a persistent login cookie, after passing the roles checks.

#### Use fingerprint

Enables or diables user fingerprint checking - a user's IP address and user agent. This determines if the user's fingerprint should be checked on return visits and when being logged in via a persistent cookie against their fingerprint stored in the database at the time the cookie was created.

See ProcessWire `sessionFingerprint` configuration value help for more details on what level of fingerprinting is available and best for your situation.

#### Session identifier

Set the name of the session variable that is set to `true` when a user is logged in via a persistent cookie.

This is useful if you want to implement additional authentication checks in your site's code when a user wants to access an area or action that requires more security. Examples of these could be changing passwords or authentication details, or deleting data or accounts.


### Cookie

#### Cookie name

The name of the cookie given to users for persistent logins. Cookie names should be kept short.

#### Cookie length

The length of time that cookies will be valid for. Change this to suit your needs and the length of time between typical user visits.

### Roles

You can control which user roles can use the persistent login functionality using the Roles configuration - using Allow and Deny lists. Role checking operates in automatic and manual modes.

Leaving both lists empty effectively disables all role checking, allowing any user to stay logged in. The Deny list takes precedence over the allow list.

Role checking runs at the point of issuing cookies as well as checking for them to log a user in.


#### Allow

When there are one or more roles selected, only users in the selected roles will be permitted to stay logged in.

#### Deny

When there are one or more roles selected, users in these roles will not be allowed to stay logged in. Users in any other roles will be allowed.


## Using

### Automatic mode

Automatic mode works by hooking the `Session::loginSuccess()` method which is triggered when a user logs in successfully. This means that it works for back-end as well as front-end logins.

At this point, and if the user passes the necessary roles checks, a cookie is generated and a corresponding database entry is made.


### Manual mode

In manual mode, you must call a module function yourself to create the persistent login cookie.

For example, this can be in response to a 'Remember Me' checkbox on a login form, indicating their preference to remain logged in.

You can use any of the below methods once you have authenticated the user attempting to log in:

**1 - calling the hooked method `persist()` on the ProcessWire `Session` class.**

This means you don't have to load and reference LoginPersist module.

```php
wire('session')->persist();
```

**2 - calling the `persist()` method on LoginPersist.**

```php
$loginPersist = wire('modules')->get('LoginPersist');
$loginPersist->persist();
```

### Hooking checkRoles

This method is responsible for checking if the user passes the denied and allowed roles checks. You can add an 'after' hook to `LoginPersist::checkRoles()` to modify the return value (`$event->return`) if you need to. The value should be true or false.

You might want to do this to check some extra conditions beyond the allow or deny roles, or have some exceptions to the rules.

```php
$wire->addHookAfter('LoginPersist::checkRoles', function(HookEvent $event) {
	$user = $event->arguments(0);
	// Allow user1, regardless of role
	if ($user->name == 'user1') {
		$event->return = true;
	}
	// Never remember user2
	if ($user->name == 'user2') {
		$event->return = false;
	}
});
```
