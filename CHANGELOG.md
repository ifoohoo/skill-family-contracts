# Changelog

<!-- release-skill:changelog:start version=0.14.0 locale=en baseline=sha256:1d5adc83e3472a4c9379f52a90c9f264499e09840eb5d084709bb0020b8313d9 -->
## [0.14.0] - 2026-08-28

Contracts 0.14.0 adds consumer contract-testing vectors and capability-adoption fields while keeping the three-package lockstep.

### Added

- Adds the consumer-contract-vector schema, official vectors, and listConsumerContractVectors/verifyConsumerContractVector entrypoints.
- Adds capability-use and capability-decision fields to the migration manifest for explicit adoption decisions.

### Changed

- Advances the Contracts specification to 1.14.0; the consumer vector remains a candidate testing contract and does not change existing host-verification identities.
- Keeps candidate discovery, migration completion, contract integration, and real-host qualification as separate conclusions.

### Upgrade Notes

Pin Contracts, Harness, and Engineering Kit to 0.14.0 together. Consumer vectors prove contract wiring only; consumers still own domain tests and any real-host qualification.
<!-- release-skill:changelog:end version=0.14.0 locale=en -->


<!-- release-skill:changelog:start version=0.13.0 locale=en baseline=sha256:98b7210e4eafd177ac3fe9294badfc810f1f93626fbdde589e8e3dee1a556d49 -->
## [0.13.0] - 2026-08-26

Contracts 0.13.0 is a source candidate for complete plugin verification and private filesystem tree observations.

### Added

- Adds plugin-verification-request, plugin-verification-result and filesystem-tree-observation under permanent Schema identities.

### Changed

- Advances the Contracts specification to 1.13.0 with 42 registered top-level object classes.
- Extends the existing watchdog envelope with optional per-stream output limit facts; legacy uncapped calls keep their shape.

### Upgrade Notes

Pin all three packages together. Existing single-Skill host verification and Kernel 1.8.0 remain unchanged. Candidate preparation does not establish host qualification, independent acceptance or publication.
<!-- release-skill:changelog:end version=0.13.0 locale=en -->


<!-- release-skill:changelog:start version=0.12.0 locale=en baseline=sha256:f7558a99e241184b6e67230d9a61aee9ca770b299f00d01e2bca16a1fc2aed89 -->
## [0.12.0] - 2026-08-26

Contracts 0.12.0 extends the existing host-verification contracts to five fixed host and driver pairings.

### Added

- Adds three fixed verification pairings to the existing two, with existing-user-state and host-managed credential semantics; the host capability matrix lists the exact hosts and drivers.

### Changed

- Allows an execution-failed result with exit status zero when the host output fails the fixed protocol check.
- Advances the Contracts specification to 1.12.0 without adding top-level object classes, Schema IDs, or error codes.

### Upgrade Notes

Upgrade the three Foundation packages together to 0.12.0. Host verification remains candidate and reports execution facts, not domain approval. Descriptor verification does not grant automatic installation or lifecycle support.
<!-- release-skill:changelog:end version=0.12.0 locale=en -->


<!-- release-skill:changelog:start version=0.11.0 locale=en baseline=sha256:3a3bae24cc03a238579c4c3ec3be60b144908f766dbf4367fe20ade6fc86f1a6 -->
## [0.11.0] - 2026-08-25

Contracts 0.11.0 adds candidate request and result contracts for bounded real-host verification.

### Added

- Adds closed host-verification request and result schemas with explicit digest preimages, per-host bindings, and fail-closed terminal semantics.
- Adds the Kimi and WorkBuddy descriptor verification tuples (existing-user-state + host-managed) without adding a general driver registry or authentication SPI.

### Changed

- Carries the previously prepared host Profile closure into the lockstep 0.11.0 family release.

### Upgrade Notes

Consumers may validate the candidate host-verification contracts, but the 0.11.0 implementation remains unpublished until the Kimi and WorkBuddy real-host publication gates pass.
<!-- release-skill:changelog:end version=0.11.0 locale=en -->


<!-- release-skill:changelog:start version=0.10.0 locale=en baseline=sha256:8c1c408b972d96d308a3517252b89671dd024ba0ae56d3e502a0071360577287 -->
## [0.10.0] - 2026-08-24

Contracts 0.10.0 ships the Contracts 1.10.0 minor specification, separating capability maturity from canonical consumer identity, extending existing host contracts, and adding business-neutral peer adapter verification contracts.

### Added

- Adds the canonical skill-family-contracts/quickstart-profile export backed by the same module as the historical candidate path.
- Adds machine-readable candidate-promotion and historical-candidate migration policies.
- Adds a frozen eight-entry migration map from historical Quickstart and batch Schema IDs to maturity-neutral canonical IDs.
- Keeps the existing host Schema identities while extending their registered 1.10.0 semantics for manual host support, finite source aliases, independent nine-fact probe results, and digest-bound lifecycle plans.
- Registers closed request/result Schemas for read-only verification of two or more peer adapter directories, including order-independent common closure and complete logical mappings.

### Changed

- Keeps Quickstart Profile v2 and ordered batch validation candidate while loaders expose canonical Schema IDs. Registry, rules, and error-code documents carry the 1.10.0 coordinate; the Kernel document remains byte-pinned at its 1.8.0 lifecycle coordinate.
- Requires new capabilities to use maturity-neutral identities from their first release.

### Upgrade Notes

Existing Quickstart v2 consumers should update all three exact pins to 0.10.0 and migrate once from the historical candidate subpath and Schema IDs to canonical identities. A later stable promotion will not require another source or contract-identity change; projection rebuilds still follow existing bound-input rules. Quickstart v1 consumers remain pinned to 0.2.1.
<!-- release-skill:changelog:end version=0.10.0 locale=en -->


<!-- release-skill:changelog:start version=0.9.0 locale=en baseline=sha256:a56a3643cbb3bd948bad18d478965f67e6a1b63ac99c2fcf3879a92a48e3f994 -->
## [0.9.0] - 2026-08-24

Contracts 1.9.0 adds stable filesystem binding and fixed-set publication schemas plus candidate ordered batch-validation schemas.

### Added

- Registers filesystem-root-binding, fixed-set-publication-manifest, and fixed-set-publication-receipt as stable top-level contract objects.
- Adds candidate schema-validation-batch request and result schemas for the existing Quickstart Bundle mechanism bridge.

### Changed

- Moves the package to the Foundation 0.9.0 lockstep line.

### Upgrade Notes

Consumers may adopt the stable filesystem schemas after reviewing the corresponding Harness APIs. Batch validation remains candidate and requires the exact 0.9.0 Bundle surface.
<!-- release-skill:changelog:end version=0.9.0 locale=en -->


<!-- release-skill:changelog:start version=0.8.4 locale=en baseline=sha256:0fa2a928cd163594584b41398db0546b816f30028c6d4f565ad07187d40589da -->
## [0.8.4] - 2026-08-24

Contracts 1.8.0 adds a business-neutral external source-authority receipt and pure validation APIs.

### Added

- Registers the closed source-authority-receipt Schema as the thirty-second stable top-level contract object.
- Adds validateSourceAuthorityReceipt and parseSourceAuthorityReceipt for canonical receipt validation and exact caller-observed subject matching.

### Changed

- Moves the package version to 0.8.4 together with Harness and Engineering Kit.

### Upgrade Notes

Consumers that need source authority must validate the external receipt against actual package subjects, then pass the returned sourceRepository and sourceBaseCommit through their existing source fields. Existing Contracts consumers need no migration.
<!-- release-skill:changelog:end version=0.8.4 locale=en -->


<!-- release-skill:changelog:start version=0.8.3 locale=en baseline=sha256:6bd6b0b96981be77e1cdc0e46e9ff6e68b29a57ccb5182ae5ace1c2d9814cbd4 -->
## [0.8.3] - 2026-08-23

Lockstep patch release for Foundation 0.8.3; the Contracts 1.7.0 machine contract is unchanged.

### Changed

- Moves the package version to 0.8.3 together with Harness and Engineering Kit.
- Keeps Contracts 1.7.0, all 31 registered top-level object classes, schemas, error codes, and public exports unchanged.

### Upgrade Notes

Consumers must pin all three Foundation packages to exactly 0.8.3 before rebuilding a managed Bundle. No Contracts API or specification migration is required from 0.8.2.
<!-- release-skill:changelog:end version=0.8.3 locale=en -->


<!-- release-skill:changelog:start version=0.8.2 locale=en baseline=sha256:03f1845f7e02e7dc89a5f963df7d0ad105e284bb8b87bce69bc2fed265e99fbc -->
## [0.8.2] - 2026-08-23

Lockstep patch release for Foundation 0.8.2; the Contracts 1.7.0 machine contract is unchanged.

### Changed

- Moves the package version to 0.8.2 together with Harness and Engineering Kit.
- Keeps Contracts 1.7.0, all 31 registered top-level object classes, schemas, error codes, and public exports unchanged.

### Upgrade Notes

Consumers must pin all three Foundation packages to exactly 0.8.2 before rebuilding a managed Bundle. No Contracts API or specification migration is required from 0.8.1.
<!-- release-skill:changelog:end version=0.8.2 locale=en -->


<!-- release-skill:changelog:start version=0.8.1 locale=en baseline=sha256:85bbc54b318942072db6809e89b5b90c2a9cdb8c78773ef7d39d0cdc10d3fd10 -->
## [0.8.1] - 2026-08-22

Lockstep patch release for the Foundation 0.8 line; the Contracts 1.7.0 machine contract is unchanged.

### Changed

- Moves the package version to 0.8.1 together with Harness and Engineering Kit.
- Keeps Contracts 1.7.0, all 31 registered top-level object classes, schemas, error codes, and public exports unchanged.

### Upgrade Notes

Consumers may pin 0.8.1 to stay on the lockstep Foundation patch line. No Contracts API or specification migration is required from 0.8.0.
<!-- release-skill:changelog:end version=0.8.1 locale=en -->


<!-- release-skill:changelog:start version=0.8.0 locale=en baseline=sha256:ae12fc054a9dc2dd4a2e949ddf9edafa86dead8fbde7de24a69bd7a15c33cc22 -->
## [0.8.0] - 2026-08-21

Contracts 1.7.0 adds the Project Profile contract while the package follows the Foundation 0.8.0 lockstep line.

### Added

- Adds the project-profile contract and grows the registry from 30 to 31 top-level objects.
- Makes the Contracts profile-adoption-declaration $defs the only field-shape authority for adoption and overrides; the SPI file remains a compatibility forwarding path.

### Upgrade Notes

Consumers validating scaffolded project roots must adopt Contracts 1.7.0. The foundation_pin package versions remain the exact Foundation npm package version 0.8.0, not the Contracts specification version.
<!-- release-skill:changelog:end version=0.8.0 locale=en -->


<!-- release-skill:changelog:start version=0.7.0 locale=en baseline=sha256:6146b7916b02e8e46de9d791567498d095ed2843f64cf906bcd6119dc6ae289f -->
## [0.7.0] - 2026-08-21

Lockstep version bump with the Foundation 0.7.0 line; the machine contract is unchanged.

### Changed

- No machine contract change - CONTRACTS_VERSION stays 1.6.0 with the 30-class top-level registry, nine mandatory rules, and the registered error codes and protocol names exactly as published in 0.6.0; the package version moves in lockstep with the Foundation line because the three leaf packages share one public version coordinate.

### Upgrade Notes

Version 0.7.0 carries no contracts surface change. Consumers pinned to contract-spec 1.6.0 keep their existing validation; the audit baseline pin remains contracts-1.6.0.pin.json.
<!-- release-skill:changelog:end version=0.7.0 locale=en -->


<!-- release-skill:changelog:start version=0.6.0 locale=en baseline=sha256:3c16158b9f7ded79eb1b3d4917da94a3c2ccdde20a3f20cf41f72ca5e0caa444 -->
## [0.6.0] - 2026-08-21

This release grows the stable Contracts registry from 24 to 30 top-level object classes under contract-spec version 1.6.0 (audit remediation C5), adds the append-only audit baseline pin contract with its consumption-side verification, and adds the minimal consumption contract of the token estimate record.

### Added

- Registers six new top-level contract classes (24 to 30) - public-boundary-declaration, platform-difference-registry, observation-scope, profile-adoption-declaration, audit-baseline-pin and token-estimate-record - under contract-spec 1.6.0.
- Adds the append-only audit baseline pin contract and its consumption side - BASELINE_PIN_KINDS, describeBaselinePin and verifyBaselinePin (AUD-BASE-001 / AUD-LOCK-001); contracts-1.6.0.pin.json is the current pin and earlier pins stay as read-only archives.
- Adds the minimal consumption contract of the token estimate record (SG-33) - TOKEN_ESTIMATE_CONSUMPTION, TOKEN_ESTIMATE_CONSUMPTION_REASONS, TOKEN_ESTIMATE_CONSUMPTION_ERROR_KIND, consumeTokenEstimate and consumeTokenEstimateStrict, with the consumptionTarget integer field and fail-closed refusal semantics, plus negative-03/04 and positive-02 fixtures.

### Changed

- Bumps CONTRACTS_VERSION to 1.6.0; the audit surface now projects the 30-class registry and the baseline pin machinery, and the integration audit verify-lock points at the 1.6.0 pin.
- Re-records the quickstart-profile stable baseline (F2) against the 1.6.0 registry; method identifiers, parameter schemas, and domain result semantics stay under consumer ownership.

### Upgrade Notes

Version 0.6.0 is the audit remediation contracts line. Consumers validating the six new object classes must pin contract-spec 1.6.0 and validate against the 30-class registry; audit consumers verify against contracts-1.6.0.pin.json.
<!-- release-skill:changelog:end version=0.6.0 locale=en -->


<!-- release-skill:changelog:start version=0.5.0 locale=en baseline=sha256:5315d7c427d017dcc274a89ccba86e7406affb309ad4d30989c770e6e2973a0c -->
## [0.5.0] - 2026-08-16

This release grows the stable Contracts registry from 20 to 22 top-level object classes under contract-spec version 1.5.0 (FND-ADR-010 and FND-ADR-011), adding the declared-read-surface result and the structured-scan policy documents.

### Added

- Registers declared-read-surface-result, the result envelope of the harness assertDeclaredReadSurface mechanism (FND-ADR-010), with a closed three-rule violation vocabulary and a closed five-guarantee enumeration.
- Registers structured-scan-policy, the declarative policy of the harness structured surface scanner (FND-ADR-011), carrying allowedNetworks, approvedRegistries, approvedCoordinates, formatAdapters, symlinkPolicy, binaryPolicy and an optional hostKeyPattern.
- Adds error-code kinds structured-scan-violation and structured-scan-invalid alongside the existing declared-read-surface kinds under the stable SFC2004 mechanism error; rule categories travel in details.rule and do not each get a code.
- Bumps contract-spec to 1.5.0 with a new append-only audit baseline pin (contracts-1.5.0.pin.json); the 1.4.0 pin stays as a read-only archive.

### Changed

- The surface-scan-policy and structured-scan-policy schema descriptions now state the relationship between the public policy documents and workspace-private leak policies explicitly: a workspace-private leak-policy.json instance document is not a subset, not isomorphic and not a migration target of these schemas — the documents share rule vocabulary and fail-closed semantics by design, but their byte-level shapes are independent and must not be compared for compatibility. scanSurface is the execution-core generalization projection: the public, consumer-parameterized form of the same mechanism family, without any private identity, path, or approval-list interpretation of its own.
- Keeps method identifiers, parameter schemas, and domain result semantics under consumer ownership.

### Upgrade Notes

Version 0.5.0 is the FND-ADR-010/011 contracts line. Consumers validating declared-read-surface-result or structured-scan-policy must pin contract-spec 1.5.0 and validate against the 22-class registry.
<!-- release-skill:changelog:end version=0.5.0 locale=en -->


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
