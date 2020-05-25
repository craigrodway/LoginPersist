# Changelog

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
