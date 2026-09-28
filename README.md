<!-- release-skill:safe-first-command -->
<!-- release-skill:external-write-boundary -->
> 简体中文版：[README.zh-CN.md](./README.zh-CN.md)

# skill-family-contracts

<!-- release-skill:release-version: 0.23.0 -->

The single authoritative package of machine-executable engineering structure and mechanism protocols. The 0.22.0 package carries the Contracts 1.20.0 specification with 53 top-level objects. The earlier published 0.21.0 package keeps Contracts 1.18.0 and 46 objects; do not attribute the 1.20.0 surface to that artifact.

<!-- release-skill:managed:start id=latest-release -->
**0.23.0** (2026-09-26)

Contracts 0.23.0 moves the package coordinate in lockstep with Foundation 0.23.0 and keeps the published 1.20.0 specification. This note adds no contract object, Schema, or protocol, and it does not claim remote publication.

**Changed**

- Aligns the package identity with Foundation 0.23.0 while the specification remains Contracts 1.20.0, with the same 53 top-level object classes, nine mandatory rules, error-code ranges, and Kernel protocol registration.
- Adds no request, result, Schema, fixture family, or mechanism. File-set application and recovery, and the professional-conclusion object, stay as published in 0.22.0.

**Upgrade Notes**

Pin all three Foundation packages to exactly 0.23.0. Consumers keep using Contracts 1.20.0. Remote publication of 0.23.0 is outside this note.
<!-- release-skill:managed:end id=latest-release -->

## Published 1.20.0 Specification

Contracts 1.20.0 already ships in the published 0.22.0 package, so this specification has a published package coordinate. That release registers the
`professional-conclusion` Schema and extends `project-manifest` with optional declarative engineering
metadata for release-unit versions and physical entries; it then registers the multi-path ordinary-file
apply/recovery family — `file-set-apply-request`, `file-set-recovery-request`, `file-set-prune-request`,
`file-set-result`, `file-set-event-payload` and `state-store-recovery-observation`. Contracts validates
only structure: the provider still owns domain codes and details, the caller owns explicit-path transfer,
contained reading, the acceptance decision, and the domain judgment inside `validate`; the Harness owns
the file-set mechanism. The current working tree is the 0.23.0 lockstep candidate and is not yet formally published. It keeps Contracts 1.20.0 and adds no new specification coordinate.

## Problem It Solves

When every skill-family project writes its own set of structural contracts, you get schema drift, inconsistent error codes, and protocol-name collisions. Contracts consolidates the structure, protocols, error codes, and protocol-name registry into one frozen machine-readable authority, so that the Harness and Kit consume it unidirectionally instead of each interpreting it independently.

## Core Mental Model

Contracts is the "definition and registry" layer, not the "execution" layer. The current local source owns the JSON Schemas for 53 top-level object classes, including Project Profile, shared profile-adoption definitions, filesystem binding, fixed-set publication, multi-path file-set apply/recovery, peer adapter verification, candidate real-host verification, executable identity, skill-family directory verification, engineering baseline, and professional conclusion. It also owns the Kernel Protocol, stable error codes, protocol-name and `$id` registry, and the nine finite mechanical check types together with a restricted set of mandatory rules. This package does not perform skeleton generation, file writing, auditing, or publishing; mechanism implementation is owned by the Harness, and engineering commands are owned by the Kit.

Schema validation is based entirely on [Ajv](https://ajv.js.org/) (exact version in `package.json`), routing by dialect to the corresponding Ajv class; no hand-written schema-subset interpreter is implemented.

## Installation and Minimal Example

Version 0.23.0 is the local source candidate and is not yet formally published. Version 0.22.0 remains the published package and already carries Contracts 1.20.0. Build all three working-tree tarballs into one temporary directory and install those exact files for a candidate check:

```sh
pack_dir="$(mktemp -d)"
(cd packages/skill-family-contracts && pnpm pack --pack-destination "$pack_dir")
(cd packages/skill-family-harness-node && pnpm pack --pack-destination "$pack_dir")
(cd packages/skill-family-engineering-kit && pnpm pack --pack-destination "$pack_dir")
mkdir "$pack_dir/consumer" && (cd "$pack_dir/consumer" && npm init -y)
(cd "$pack_dir/consumer" && npm install "$pack_dir/skill-family-contracts-0.23.0.tgz" "$pack_dir/skill-family-harness-node-0.23.0.tgz" "$pack_dir/skill-family-engineering-kit-0.23.0.tgz")
```

After 0.23.0 is formally published, use the registry coordinate:

```sh
npm install skill-family-contracts@0.23.0
npm info skill-family-contracts --help
```

The minimal example starts from an empty directory and demonstrates how to validate a registered contract object:

```js
// Run from an installed consumer directory after publication.
import { validateDocument } from "skill-family-contracts";

const document = {
  schemaVersion: 1,
  kind: "skill-family.project-manifest",
  project: { id: "my-project", name: "My Project", description: "Example" },
  contracts: { version: "1.0.0", profile: "generic" },
  managedFiles: ["package.json"],
  updatedAt: "2026-01-01T00:00:00Z",
};

const result = validateDocument(document, {
  schemaId: "https://contracts.skill-family.example/v1/project-manifest.json",
  dialect: "2020-12",
});
if (!result.valid) console.error(result.errorCode);
```

The code above shows the basic `validateDocument` call: pass the document and the target Schema's `$id`, and it returns `{ valid, errorCode, errors, data }`, where `data` is a normalized copy and the original input is not modified.

## External Source Authority

`parseSourceAuthorityReceipt(receipt, actualSubjects)` validates a caller-supplied receipt and compares every package name, version, filename, and SHA-256 against the caller's actual subjects. Receipt subjects must be unique and sorted by `packageName`; actual subjects may arrive in any order. A successful result exposes only `{ sourceRepository, sourceBaseCommit }` through `data`. Contracts does not discover packages, execute a target, or create release state.

## Consumer Contract Vectors

Foundation 0.14.0 exposes two public, version-bound vector sets for consumer contract tests. `listConsumerContractVectors` returns a deeply frozen array sorted by `vectorId`; `verifyConsumerContractVector` checks the vector shape and its `capabilityId`, `vectorSetId`, and exact `FOUNDATION_PACKAGE_VERSION` before consumer code runs.

```js
import {
  FOUNDATION_PACKAGE_VERSION,
  listConsumerContractVectors,
  verifyConsumerContractVector,
} from "skill-family-contracts";

const vectors = listConsumerContractVectors({
  capabilityId: "foundation.contracts.object-validation",
  foundationVersion: FOUNDATION_PACKAGE_VERSION,
});
const result = verifyConsumerContractVector(vectors[0], {
  capabilityId: vectors[0].capabilityId,
  foundationVersion: FOUNDATION_PACKAGE_VERSION,
  vectorSetId: vectors[0].vectorSetId,
});
if (!result.ok) throw new Error(`consumer vector rejected: ${result.mismatchCode}`);
// Consumer adapter code starts only after the identity and structure check.
const consumerResult = invokeConsumerAdapter(vectors[0]);
```

The first set covers `foundation.contracts.object-validation`; the second covers `foundation.harness.atomic-write`. Both contain positive, negative, and indeterminate cases. A version or identity mismatch fails closed with `SFC1013` (`CONSUMER_CONTRACT_VERSION_MISMATCH`). Contracts validates only public vector structure and identity; it does not invoke a consumer, Harness implementation, fake, or host.

The `foundation.harness.atomic-write` positive vector declares the closed relation `valueRelation: "atomic-write-contained-absolute-target"` instead of embedding a path value. Its runtime meaning is checked by the Harness and consumer tests, not by Contracts.

For `foundation.harness.atomic-write`, each `request.root` is the closed declaration `{ runtimeBinding: "atomic-write-canonical-root" }`; it is not a path, and Contracts does not evaluate it. The negative vector adds the stable `errorKind: "path-traversal"` beside `outcome` and `errorCode`; generic throw vectors retain their two-field closed shape.

The capability-specific Schema rejects that atomic binding when it appears on a non-atomic vector, while leaving other request fields outside this rule unchanged.

## Candidate Quickstart Profile

Use the candidate Quickstart Profile to evaluate an early Resource → Task → Result exchange before proposing it for the frozen registry:

```js
import {
  QUICKSTART_PROTOCOL,
  quickstartProfileSchemas,
  validateQuickstartProfileDocument,
} from "skill-family-contracts/quickstart-profile";
```

Version 0.4.0 carries Quickstart Profile v2. Its protocol fixes the business-neutral operation to `execute-method`; Resource, Task, and Result schemas resolve one another through their real v2 `$id` values. Foundation validates the JSON-safe exchange shape, while the consumer owns method identifiers, parameter schemas, and domain results.

The capability remains **candidate** and its schemas stay outside `src/registry.json`. Pin all three Foundation packages exactly while evaluating it. Version 0.10.0 adds the canonical path above; the historical `/candidate/quickstart-profile` path remains a same-source migration alias. Migrate once to the canonical path. A later stable promotion will not require another import change. Integrations that still require candidate v1 must stay pinned to exactly `0.2.1`.

## Typical Use Cases

- Need to validate whether a contract document conforms to a registered Schema: use `validateDocument`.
- Need to look up a Schema by object name or `$id`, or look up a Kernel Protocol by protocol name: use `loadRegistry` / `findSchemaByObject` / `findProtocol`.
- Need to run mandatory mechanical rules and collect unresolved references: use `runChecks` / `collectUnresolvedRefs`.
- Need to enumerate and validate the public fixtures: use `verifyAllFixtures`.

## Registered Top-Level Object Classes

| Object | `$id` | Schema File |
| --- | --- | --- |
| `project-manifest` | `https://contracts.skill-family.example/v1/project-manifest.json` | `src/schemas/project-manifest.schema.json` |
| `profile-descriptor` | `https://contracts.skill-family.example/v1/profile-descriptor.json` | `src/schemas/profile-descriptor.schema.json` |
| `project-profile` | `https://contracts.skill-family.example/v1/project-profile.json` | `src/schemas/project-profile.schema.json` |
| `managed-file-lock` | `https://contracts.skill-family.example/v1/managed-file-lock.json` | `src/schemas/managed-file-lock.schema.json` |
| `operation-request` | `https://contracts.skill-family.example/v1/operation-request.json` | `src/schemas/operation-request.schema.json` |
| `operation-result` | `https://contracts.skill-family.example/v1/operation-result.json` | `src/schemas/operation-result.schema.json` |
| `migration-manifest` | `https://contracts.skill-family.example/v1/migration-manifest.json` | `src/schemas/migration-manifest.schema.json` |
| `report-model` | `https://contracts.skill-family.example/v1/report-model.json` | `src/schemas/report-model.schema.json` |
| `report-binding` | `https://contracts.skill-family.example/v1/report-binding.json` | `src/schemas/report-binding.schema.json` |
| `host-descriptor` | `https://contracts.skill-family.example/v1/host-descriptor.json` | `src/schemas/host-descriptor.schema.json` |
| `host-registry` | `https://contracts.skill-family.example/v1/host-registry.json` | `src/schemas/host-registry.schema.json` |
| `adapter-source` | `https://contracts.skill-family.example/v1/adapter-source.json` | `src/schemas/adapter-source.schema.json` |
| `host-capability-fact` | `https://contracts.skill-family.example/v1/host-capability-fact.json` | `src/schemas/host-capability-fact.schema.json` |
| `host-probe-result` | `https://contracts.skill-family.example/v1/host-probe-result.json` | `src/schemas/host-probe-result.schema.json` |
| `adapter-build-manifest` | `https://contracts.skill-family.example/v1/adapter-build-manifest.json` | `src/schemas/adapter-build-manifest.schema.json` |
| `host-operation-plan` | `https://contracts.skill-family.example/v1/host-operation-plan.json` | `src/schemas/host-operation-plan.schema.json` |
| `host-operation-receipt` | `https://contracts.skill-family.example/v1/host-operation-receipt.json` | `src/schemas/host-operation-receipt.schema.json` |
| `state-event-envelope` | `https://contracts.skill-family.example/v1/state-event-envelope.json` | `src/schemas/state-event-envelope.schema.json` |
| `state-snapshot-metadata` | `https://contracts.skill-family.example/v1/state-snapshot-metadata.json` | `src/schemas/state-snapshot-metadata.schema.json` |
| `token-estimate-result` | `https://contracts.skill-family.example/v1/token-estimate-result.json` | `src/schemas/token-estimate-result.schema.json` |
| `surface-scan-policy` | `https://contracts.skill-family.example/v1/surface-scan-policy.json` | `src/schemas/surface-scan-policy.schema.json` |
| `declared-read-surface-result` | `https://contracts.skill-family.example/v1/declared-read-surface-result.json` | `src/schemas/declared-read-surface-result.schema.json` |
| `structured-scan-policy` | `https://contracts.skill-family.example/v1/structured-scan-policy.json` | `src/schemas/structured-scan-policy.schema.json` |
| `timeout-policy` | `https://contracts.skill-family.example/v1/timeout-policy.json` | `src/schemas/timeout-policy.schema.json` |
| `watchdog-termination-envelope` | `https://contracts.skill-family.example/v1/watchdog-termination-envelope.json` | `src/schemas/watchdog-termination-envelope.schema.json` |
| `public-boundary-declaration` | `https://contracts.skill-family.example/v1/public-boundary-declaration.json` | `src/schemas/public-boundary-declaration.schema.json` |
| `platform-difference-registry` | `https://contracts.skill-family.example/v1/platform-difference-registry.json` | `src/schemas/platform-difference-registry.schema.json` |
| `observation-scope` | `https://contracts.skill-family.example/v1/observation-scope.json` | `src/schemas/observation-scope.schema.json` |
| `profile-adoption-declaration` | `https://contracts.skill-family.example/v1/profile-adoption-declaration.json` | `src/schemas/profile-adoption-declaration.schema.json` |
| `audit-baseline-pin` | `https://contracts.skill-family.example/v1/audit-baseline-pin.json` | `src/schemas/audit-baseline-pin.schema.json` |
| `token-estimate-record` | `https://contracts.skill-family.example/v1/token-estimate-record.json` | `src/schemas/token-estimate-record.schema.json` |
| `source-authority-receipt` | `https://contracts.skill-family.example/v1/source-authority-receipt.json` | `src/schemas/source-authority-receipt.schema.json` |
| `filesystem-root-binding` | `https://contracts.skill-family.example/v1/filesystem-root-binding.json` | `src/schemas/filesystem-root-binding.schema.json` |
| `fixed-set-publication-manifest` | `https://contracts.skill-family.example/v1/fixed-set-publication-manifest.json` | `src/schemas/fixed-set-publication-manifest.schema.json` |
| `fixed-set-publication-receipt` | `https://contracts.skill-family.example/v1/fixed-set-publication-receipt.json` | `src/schemas/fixed-set-publication-receipt.schema.json` |
| `adapter-peer-verification-request` | `https://contracts.skill-family.example/v1/adapter-peer-verification-request.json` | `src/schemas/adapter-peer-verification-request.schema.json` |
| `adapter-peer-verification-result` | `https://contracts.skill-family.example/v1/adapter-peer-verification-result.json` | `src/schemas/adapter-peer-verification-result.schema.json` |
| `host-verification-request` | `https://contracts.skill-family.example/v1/host-verification-request.json` | `src/schemas/host-verification-request.schema.json` |
| `host-verification-result` | `https://contracts.skill-family.example/v1/host-verification-result.json` | `src/schemas/host-verification-result.schema.json` |
| `plugin-verification-request` | `https://contracts.skill-family.example/v1/plugin-verification-request.json` | `src/schemas/plugin-verification-request.schema.json` |
| `plugin-verification-result` | `https://contracts.skill-family.example/v1/plugin-verification-result.json` | `src/schemas/plugin-verification-result.schema.json` |
| `filesystem-tree-observation` | `https://contracts.skill-family.example/v1/filesystem-tree-observation.json` | `src/schemas/filesystem-tree-observation.schema.json` |
| `executable-identity-observation` | `https://contracts.skill-family.example/v1/executable-identity-observation.json` | `src/schemas/executable-identity-observation.schema.json` |
| `skill-family-directory-verification-request` | `https://contracts.skill-family.example/v1/skill-family-directory-verification-request.json` | `src/schemas/skill-family-directory-verification-request.schema.json` |
| `skill-family-directory-verification-result` | `https://contracts.skill-family.example/v1/skill-family-directory-verification-result.json` | `src/schemas/skill-family-directory-verification-result.schema.json` |
| `engineering-baseline` | `https://contracts.skill-family.example/v1/engineering-baseline.json` | `src/schemas/engineering-baseline.schema.json` |
| `professional-conclusion` | `https://contracts.skill-family.example/v1/professional-conclusion.json` | `src/schemas/professional-conclusion.schema.json` |
| `file-set-apply-request` | `https://contracts.skill-family.example/v1/file-set-apply-request.json` | `src/schemas/file-set-apply-request.schema.json` |
| `file-set-recovery-request` | `https://contracts.skill-family.example/v1/file-set-recovery-request.json` | `src/schemas/file-set-recovery-request.schema.json` |
| `file-set-prune-request` | `https://contracts.skill-family.example/v1/file-set-prune-request.json` | `src/schemas/file-set-prune-request.schema.json` |
| `file-set-result` | `https://contracts.skill-family.example/v1/file-set-result.json` | `src/schemas/file-set-result.schema.json` |
| `file-set-event-payload` | `https://contracts.skill-family.example/v1/file-set-event-payload.json` | `src/schemas/file-set-event-payload.schema.json` |
| `state-store-recovery-observation` | `https://contracts.skill-family.example/v1/state-store-recovery-observation.json` | `src/schemas/state-store-recovery-observation.schema.json` |

Host descriptor/probe/plan contracts remain the same registered object identities, while Contracts 1.10.0 extends their stable semantics. Version 0.10.0 adds optional maturity and finite source-alias fields, permits `manual` host support, and binds update/uninstall plans to prior member digests. The nine probe facts are always represented independently; an unavailable or unknown fact is not inferred from the CLI or version fact.

The peer adapter verification request/result contracts are business-neutral and read-only. They require at least two explicit peers, closed path and mapping inputs, and a result that can be recomputed from real directories. They do not define canonical migration, directory writes, receipts, retries, lifecycle state, or consumer smoke.

The host-verification request/result contracts are business-neutral candidate contracts. They bind a common candidate and per-host facts without exposing credentials, paths, prompts, raw streams, durable receipts, domain PASS/FAIL, or release state. Their `$id` and `operation=host-verification` are permanent from 1.11.0. Version 1.12.0 retains the fixed `existing-user-state + host-managed` auth pair and extends the descriptor-bound drivers from two to five; see the [host capability matrix](../../docs/reference/host-capability-matrix.md) for their exact identities. An exit status of zero may still produce `execution-failed` when the output fails its fixed protocol check.

The plugin-verification contracts retain the existing `install-only` and `install-and-invoke` goals and add a `native-lifecycle` branch. Native results carry exactly twelve ordered semantic stages. Contracts checks the closed structure, stage order, stop propagation, and empty `commands`/`trees` on later `not-performed` stages. It does not define one cross-host command plan, interpret vendor output, expose `hostState`, or own the Qoder and WorkBuddy Oracle.

The skill-family directory request/result contracts describe the Kimi directory boundary without accepting a caller-supplied observation. They allow `official-observation-unavailable` to remain an `indeterminate` result with unknown per-skill facts. Process execution, fixed argv and environment, raw-stream parsing, and any future official observation mapping belong to Engineering Kit rather than Contracts.


All v1 Schemas use the draft 2020-12 dialect; instance envelopes are uniformly `schemaVersion: 1` + a unique `kind` constant + `additionalProperties: false` at each layer. The `$id` namespace `contracts.skill-family.example` uses a reserved example domain and never resolves to a real site.

## Kernel Protocol

Registry: `src/registry.json`; frozen definition: `src/kernel-protocol.json`.

- Protocol name: `skill-family.kernel.operation`, version `1`, status `stable`.
- State set: `accepted`, `running`, `succeeded`, `failed`, `rejected`; terminal states are `succeeded`, `failed`, `rejected`; `operation-result` carries only terminal states.
- Transitions: `accepted → running → succeeded|failed`, with `accepted → failed` also allowed; the entry may directly `rejected`.
- The v1 operation vocabulary freezes only `validate`, whose params contract is defined inside `kernel-protocol.json` (`schemaId` + `document` required). Adding a new operation name is a contract change and requires a new version registration.

Duplicate-name protocols and duplicate `$id`s are machine-rejected: `registerProtocol` throws `SFC1004`, `registerSchema` throws `SFC1003`; the check types `protocol.unique-name` and `schema.unique-id` make the same determination against the registry.

## Stable Error Codes

Frozen registry: `src/error-codes.json`. `SFC1xxx` are contract-authority-layer errors, `SFC2xxx` are kernel-operation errors, `SFC3xxx` are report-binding errors. Codes are only added, never modified or reused. Frozen in v1:

| Code | Name | Summary |
| --- | --- | --- |
| SFC1001 | SCHEMA_VALIDATION_FAILED | Document failed target Schema validation |
| SFC1002 | UNKNOWN_SCHEMA_ID | `$id` not registered in the registry |
| SFC1003 | DUPLICATE_SCHEMA_ID | Duplicate `$id` registration rejected |
| SFC1004 | DUPLICATE_PROTOCOL_NAME | Duplicate protocol name/version registration rejected |
| SFC1005 | UNRESOLVED_REF | `$ref` target cannot be resolved |
| SFC1006 | UNSUPPORTED_DIALECT | Dialect not in the frozen support set |
| SFC1007 | UNKNOWN_CHECK_TYPE | Rule uses a check type outside the nine categories |
| SFC1008 | RULE_BUDGET_EXCEEDED | Mandatory rule count exceeds budget/limit |
| SFC1009 | UNKNOWN_ERROR_CODE | References an unregistered error code |
| SFC1010 | FIXTURE_EXPECTATION_MISMATCH | Fixture behavior does not match declared expectation |
| SFC1011 | UNKNOWN_PROTOCOL | Request references an unregistered protocol name/version |
| SFC1012 | SCHEMA_COMPILE_FAILED | Schema itself cannot be compiled |
| SFC1013 | CONSUMER_CONTRACT_VERSION_MISMATCH | Consumer vector identity or exact Foundation version mismatch |
| SFC2002 | UNKNOWN_OPERATION | Operation name not in the frozen vocabulary |
| SFC2003 | INVALID_PARAMS | Parameters do not satisfy the operation's frozen params contract |
| SFC2004 | EXECUTION_FAILED | Mechanism runtime execution failed (only demonstrable at runtime) |
| SFC3001 | REPORT_DIGEST_MISMATCH | Report or result digest inconsistent with binding |
| SFC3002 | REPORT_ELEMENT_MISSING | Report missing a mandatory element |
| SFC3003 | REPORT_FACT_DRIFT | Report bytes deviate from deterministic re-render result |

## Dialects and Validation Strategy (Ajv)

- Supported dialects: `draft-07`, `2020-12`. Draft detection uses `$schema` URI mapping (`detectDialect`), and validation routes by dialect to the corresponding Ajv class.
- Validation strategy (`VALIDATION_POLICIES`):
  - `strict` (default): no type coercion, no injected defaults; Ajv strict mode fully enabled;
  - `tolerant`: enables Ajv `coerceTypes: "array"` and `useDefaults`, for adoption scenarios.
- Format: `date-time` (RFC 3339, including calendar-validity checks) registered via Ajv `addFormat`.
- `validateDocument` never modifies the caller's input; the normalized copy is returned in the result's `data` field.

## Nine Mechanical Check Types and the Rule Budget

Registry: `src/rules.json`. The check-type set is closed, with nine types total: `schema.compile`, `schema.unique-id`, `protocol.unique-name`, `schema.ref-resolves`, `schema.dialect-declared`, `fixture.positive-passes`, `fixture.negative-coded`, `error-code.registered`, `rules.budget`.

There are currently **9** mandatory rules (CR-001, CR-006…CR-013). Among them, CR-001 performs uniform compilation of all Schemas in the registry, instead of occupying one rule per object; the budget cap is 20 rules, absolute cap 30 rules; `rules.budget` is a mechanical gate, and exceeding it fails `runChecks` with `SFC1008`.

## Fixtures

`src/fixtures/<contract>/` provides positive, negative, and dialect-boundary samples for each contract class. Each fixture declares the target Schema, dialect, policy, and expectation; negative expectations carry a stable failure code. `verifyAllFixtures()` mechanically replays all expectations, and reports `SFC1010` on mismatch. Fixtures are entirely fictional data, not an audit oracle.

## API Overview

```js
import {
  CONTRACT_OBJECTS, CONTRACTS_VERSION,
  validateDocument, SUPPORTED_DIALECTS, VALIDATION_POLICIES, detectDialect,
  loadRegistry, registerSchema, registerProtocol,
  loadKernelProtocol, checkOperation,
  runChecks, CHECK_TYPES, MANDATORY_RULES, RULE_BUDGET,
  listFixtures, verifyAllFixtures,
  FOUNDATION_PACKAGE_VERSION, listConsumerContractVectors,
  verifyConsumerContractVector, SFC1013,
  ERROR_CODES, ContractsError, stableError,
} from "skill-family-contracts";
```

The imports above list the stable public surface of this package; `validateDocument` and `runChecks` are the most commonly used entry points. `validateDocument(document, { schemaId | schema, dialect, policy })` returns `{ valid, errorCode, errors, data }`; `runChecks({ rules?, registry?, fixtures?, loadSchema? })` returns `{ ok, mandatoryCount, budget, results }`; `registerSchema` / `registerProtocol` return a new registry copy, throwing `ContractsError` with `SFC1003` / `SFC1004` respectively on duplicates.

`detectDialect(schema)` returns `draft-07`, `2020-12`, or `null` when the declaration is absent or unknown. `compileSchema` throws `SFC1006` for an unsupported dialect; `validateDocument` reports the same condition as `errorCode: "SFC1006"`. Registry lookup functions return `null` on a miss. The current local registry is `schemaVersion=1`, `contractsVersion=1.20.0`, with 53 registered Schemas and one Kernel Protocol. Registration returns a copied registry and leaves the input unchanged; consumer vector identity or exact-version mismatches fail with `SFC1013`.

## Security Boundaries and Non-Goals

It does not own generation, semantic auditing, publishing state, or remote writing; it does not define cleanup plans, publish snapshots, consumer smoke results, domain audit reports, or a free-text rule language; it does not build a general DSL. Changes to frozen content can only be carried out as a new contract-version task.

## Troubleshooting

On validation failure `errorCode` is `SFC1001` (SCHEMA_VALIDATION_FAILED, document failed target Schema validation); when `$id` is unregistered it reports `SFC1002` (UNKNOWN_SCHEMA_ID). If it fails, check whether the document satisfies the target Schema's required fields and type constraints.

## Further Documentation

- Architecture boundaries and routing: [Architecture](https://ifoohoo.github.io/skill-family-engineering-kit/architecture/), [Agent architecture routing](https://ifoohoo.github.io/skill-family-engineering-kit/agents/architecture-routing/)
- Capability catalog: [capability-catalog.json](https://ifoohoo.github.io/skill-family-engineering-kit/agents/capability-catalog.json)
- Current product status: [Public status](https://ifoohoo.github.io/skill-family-engineering-kit/public/status/)

<!-- agent-quick-reference:start -->
## Agent Quick Reference

### Use when

- You need to validate a registered contract object, look up a Schema/protocol, or run mandatory mechanical rules.
- You need to enumerate and validate public fixtures, or deterministically serialize the contract surface.
- You need to evaluate the non-stable Quickstart Resource/Task/Result profile with an exact package-version pin.

### Do not use when

- You need to validate a consumer's own business Schema (the consumer should own it; Foundation does not replace it).
- You need to mix domain semantic validation into the general contract.
- You need a compatibility-frozen Quickstart profile; the candidate subpath is not registered as stable.

### Capability selection

- `foundation.contracts.object-validation`: Ajv dual-dialect validation of all 53 registered top-level object classes in the current local contract source.
- `foundation.contracts.registry-protocol`: Schema `$id` and protocol-name registry query.
- `foundation.contracts.kernel-protocol`: operation-request/result protocol.
- `foundation.contracts.mandatory-checks`: nine mandatory rules and unresolved references.
- `foundation.contracts.fixture-verification`: full fixture replay.
- `foundation.contracts.error-codes`: stable error-code system.
- `foundation.contracts.audit-surface`: canonical JSON + sha256 digest.
- `foundation.contracts.quickstart-profile-candidate`: candidate-only Resource/Task/Result schemas and validation through the exact-version subpath.

### Required inputs

- The document to validate (carrying a registered `$id`), or the target contract object name.
- The validation policy `strict` (default) or `tolerant`.

### Outputs and evidence

- `validateDocument` returns `{ valid, errorCode, errors, data }`.
- Evidence: `packages/skill-family-contracts/test/validator.test.mjs`, `registry.test.mjs`, `checker.test.mjs`, `fixtures.test.mjs`.

### Side effects

- Pure functions; no filesystem, Git, network, or process side effects (the compile cache lives only in memory).

### Failure semantics

- Stable error codes such as `SFC1001/1002/1006`; the error object carries `stableError` and `details.kind`.
- Frozen error codes are only added, never modified; no drift.

### Architectural invariants

- The set of 53 top-level object classes in the current local source is fixed; additions require an ADR, and the error-code freeze does not drift. The published 0.21.0 package remains on the 46-object Contracts 1.18.0 surface.
- The validator is exclusively Ajv 8.20.0 (exact pin); no other implementation is accepted.

### Route elsewhere when

- Consumer business Schema validation: stays with the caller.
- Remote publish writing: route to release-skill.
- Domain audit semantics: route to a standalone audit consumer.

### Machine-readable sources

- Public capability catalog: [`capability-catalog.json`](https://ifoohoo.github.io/skill-family-engineering-kit/agents/capability-catalog.json) (`foundation.contracts.*` entries).
- Package-local structural contract: `src/registry.json`, `src/schemas/*`.
- Package-local candidate source: `candidate/quickstart-profile/*`; canonical public import: `skill-family-contracts/quickstart-profile`; historical migration alias: `skill-family-contracts/candidate/quickstart-profile`.
<!-- agent-quick-reference:end -->

## Complete Plugin Candidate

Three new candidate schemas describe plugin requests, plugin results and complete tree observations. Installation, discovery, invocation and payload comparison remain separate; raw tree content is private.

Contracts 1.20.0 ships with the 0.22.0 package. Remote availability must be established by the corresponding release-skill post-release evidence, and the published 0.21.0 package keeps the 46-object Contracts 1.18.0 surface. Local tests or working-tree tarballs do not prove contract integration, migration completion, or real-host qualification.
