# Changelog


## [2.2.0] - 2023-04-24

This version fixes cookie function usage in PHP 8 and now relies on ProcessWire's
built-in cookie functionality - thanks to [Julian Pollak](https://github.com/poljpocket)
for the contribution.

This version also includes a security fix for the cookie theft detection code,
where it might erroneously allow a single login.

The following code in the `attemptLogin()` method, upon detecting a mismatch
between the cookie's key and the token stored in the database, correctly destroys
the user's cookie sessions in the database. However, due to a missing
`return false;` statement, the rest of the logging-in code following it is also
executed when it shouldn't be.

The severity is lessened when using Fingerprinting, which works correctly to
deny logins when the fingerprint does not match the expected value.

```php
if ( ! $this->validateKeys($cookie->key, $row->token)) {
	$this->destroyLogins($cookie->user);
	return false; // <-- this line was missing
}
/* (rest of code to log the user in based on the User ID in the cookie) */
```

The risk of this issue, for most scenarios, is rated Low for likelihood but
High for impact.


### Changed
- Use ProcessWire's 'Input' built-in cookie methods instead of native PHP cookie methods.

### Fixed
- Fixed issue that might raise PHP errors if the cookie isn't in the right format.

### Security
- Fixed issue where theft detection might still allow a successful login.


## [2.1.0] - 2020-05-25

### Added
- Restrict usage by role as well as allow.
- Clear all persistent logins `destroyLogins()`

### Changed
- `checkRoles($user)` is now hookable.
- Visual layout of module configuration.
- General code maintenance.


## [2.0.0] - 2019-12-19

### Added
- Requirements for PHP and ProcessWire versions.
- New hook method `Session::persist()` as a shortcut when using manual mode.

### Changed
- Login process now uses `Session::forceLogin()` instead of hooking `authenticate`.

### Fixed
- Issue when referring to undefined `$user` variable.
- Issue generating fingerprint; now uses `Session::getFingerprint()`.


## 1.0.0

- Initial release
