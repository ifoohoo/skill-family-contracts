# Changelog

<!-- release-skill:changelog:start version=0.4.0 locale=en baseline=sha256:813375a1145b9de729fa5d128a34b1f9a12b5942ab6f06da92486aa72df7a297 -->
## [0.4.0] - 2026-08-16

This release extends the stable Contracts registry to 20 top-level object classes under the unchanged contract-spec version 1.4.0, adds finite-closed-semantics fixtures and schemas (FND-ADR-009), and ships new fixed-set-publication and schema-inventory candidates.

### Added

- Registers two new stable top-level objects, token-estimate-result (deterministic UTF-8 byte-count estimation result with a closed guarantee enumeration) and surface-scan-policy (path/content patterns plus carried-only allowedUses), raising the object registry from 18 to 20 classes under contract-spec version 1.4.0 (FND-ADR-009).
- Extends the migration-manifest schema with legacy reference support and adds the matching fixtures (negative-06, negative-07, negative-08, positive-03).
- Adds the fixed-set-publication candidate (manifest and receipt schemas, fixtures, and a capability-fit declaration covering native-prebuild runtimes) outside the stable registry.
- Extends the Quickstart Profile candidate with consumer-schema-inventory and harness-surface-inventory schemas plus inventory fixtures.
- Applies the FND-ADR-001 gate-1 relaxation (one existing plus one structurally identical foreseeable consumer) and introduces gate 8 for finite closed semantics (FND-ADR-009).

### Changed

- Keeps CONTRACTS_VERSION at 1.4.0; the same contract-spec line now covers the 20-object set, while the published 0.3.0 bytes carry the 18-object set of that version.
- Keeps method identifiers, parameter schemas, and domain result semantics under consumer ownership.

### Upgrade Notes

Version 0.4.0 is released on npm and the public mirror. Candidate subpaths are public but not stable; pin candidate imports to exactly 0.4.0 and validate stable objects against the 20-class registry contract.
<!-- release-skill:changelog:end version=0.4.0 locale=en -->


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
