# Changelog

<!-- release-skill:changelog:start version=0.2.1 locale=en baseline=sha256:b759650e4d52969bd907bccea937c64622b00b0ff97851da9c6c4aee8adb4888 -->
## [0.2.1] - 2026-08-10

This release adds a candidate Quickstart Profile contract surface and makes the package release documentation available in English and Simplified Chinese.

### Added

- Adds candidate Resource, Task, and Result schemas with strict validation helpers. The candidate schemas remain outside the stable Contracts registry.
- Adds complete English and Simplified Chinese package documentation, including an agent quick-reference section.

### Changed

- Manages the current README and CHANGELOG release sections from one bilingual, versioned notes source.
- Distributes the project NOTICE separately from the Apache-2.0 LICENSE.

### Upgrade Notes

Stable registry consumers do not need to change. Import the Quickstart Profile only through its candidate subpath and do not treat it as a frozen Contracts object.
<!-- release-skill:changelog:end version=0.2.1 locale=en -->
