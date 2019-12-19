# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## 2.0.0 - 2019-12-19

### Added
- PHP and ProcessWire module requirements.
- New hook method `Session::persist()` as a shortcut when using manual mode.

### Changed
- Login process now uses `Session::forceLogin()` instead of hooking `authenticate`.
- Documentation.

### Fixed
- Issue when referring to undefined `$user` variable.
- Issue generating fingerprint; now uses `Session::getFingerprint()`.


## 1.0.0

- Initial release
