# Changelog

<!-- release-skill:changelog:start version=0.3.0 locale=en baseline=sha256:32a8df58662f9dbb64e142cb8a2b329556b4bd59872e48fcbc317cc971cbdd7c -->
## [0.3.0] - 2026-08-12

This source candidate replaces the Quickstart Profile candidate with v2 while preserving the stable Contracts registry and kernel protocol.

### Added

- Adds the v2 protocol definition with the business-neutral execute-method operation and a real $id-indexed Resource, Task, and Result schema collection.
- Enforces JSON-safe Task and Result boundaries, one path-backed observation, terminal output shapes, and exact evidence bindings.

### Changed

- Replaces the incompatible 0.2.1 candidate surface; consumers that still require v1 must remain pinned to exactly 0.2.1.
- Keeps method identifiers, parameter schemas, and domain result semantics under consumer ownership.

### Upgrade Notes

Version 0.3.0 is a local, unpublished source candidate. Pin all candidate imports to an exact package version and migrate v1 integrations before selecting this version.
<!-- release-skill:changelog:end version=0.3.0 locale=en -->


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
