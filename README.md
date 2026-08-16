<!-- release-skill:safe-first-command -->
<!-- release-skill:external-write-boundary -->
> 简体中文版：[README.zh-CN.md](./README.zh-CN.md)

# skill-family-contracts

<!-- release-skill:release-version: 0.5.0 -->

The single authoritative package of machine-executable engineering structure and mechanism protocols (Contracts 1.4.0, frozen).

<!-- release-skill:managed:start id=latest-release -->
**0.5.0** (2026-08-16)

This release grows the stable Contracts registry from 20 to 22 top-level object classes under contract-spec version 1.5.0 (FND-ADR-010 and FND-ADR-011), adding the declared-read-surface result and the structured-scan policy documents.

**Added**

- Registers declared-read-surface-result, the result envelope of the harness assertDeclaredReadSurface mechanism (FND-ADR-010), with a closed three-rule violation vocabulary and a closed five-guarantee enumeration.
- Registers structured-scan-policy, the declarative policy of the harness structured surface scanner (FND-ADR-011), carrying allowedNetworks, approvedRegistries, approvedCoordinates, formatAdapters, symlinkPolicy, binaryPolicy and an optional hostKeyPattern.
- Adds error-code kinds structured-scan-violation and structured-scan-invalid alongside the existing declared-read-surface kinds under the stable SFC2004 mechanism error; rule categories travel in details.rule and do not each get a code.
- Bumps contract-spec to 1.5.0 with a new append-only audit baseline pin (contracts-1.5.0.pin.json); the 1.4.0 pin stays as a read-only archive.

**Changed**

- The surface-scan-policy and structured-scan-policy schema descriptions now state the relationship between the public policy documents and workspace-private leak policies explicitly: a workspace-private leak-policy.json instance document is not a subset, not isomorphic and not a migration target of these schemas — the documents share rule vocabulary and fail-closed semantics by design, but their byte-level shapes are independent and must not be compared for compatibility. scanSurface is the execution-core generalization projection: the public, consumer-parameterized form of the same mechanism family, without any private identity, path, or approval-list interpretation of its own.
- Keeps method identifiers, parameter schemas, and domain result semantics under consumer ownership.

**Upgrade Notes**

Version 0.5.0 is the FND-ADR-010/011 contracts line. Consumers validating declared-read-surface-result or structured-scan-policy must pin contract-spec 1.5.0 and validate against the 22-class registry.
<!-- release-skill:managed:end id=latest-release -->

## Problem It Solves

When every skill-family project writes its own set of structural contracts, you get schema drift, inconsistent error codes, and protocol-name collisions. Contracts consolidates the structure, protocols, error codes, and protocol-name registry into one frozen machine-readable authority, so that the Harness and Kit consume it unidirectionally instead of each interpreting it independently.

## Core Mental Model

Contracts is the "definition and registry" layer, not the "execution" layer. It owns the JSON Schemas for the eighteen top-level object classes, the Kernel Protocol, the stable error codes, the protocol-name and `$id` registry, and the nine finite mechanical check types together with a restricted set of mandatory rules. This package does not perform skeleton generation, file writing, auditing, or publishing; mechanism implementation is owned by the Harness, and engineering commands are owned by the Kit.

Schema validation is based entirely on [Ajv](https://ajv.js.org/) (exact version in `package.json`), routing by dialect to the corresponding Ajv class; no hand-written schema-subset interpreter is implemented.

## Installation and Minimal Example

```sh
npm install skill-family-contracts@0.5.0
npm info skill-family-contracts --help
```

The minimal example starts from an empty directory and demonstrates how to validate a registered contract object:

```js
// Run from an empty directory: npm install skill-family-contracts@0.5.0
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

## Candidate Quickstart Profile

Use the candidate Quickstart Profile to evaluate an early Resource → Task → Result exchange before proposing it for the frozen registry:

```js
import {
  QUICKSTART_PROTOCOL,
  quickstartProfileSchemas,
  validateQuickstartProfileDocument,
} from "skill-family-contracts/candidate/quickstart-profile";
```

Version 0.4.0 carries Quickstart Profile v2. Its protocol fixes the business-neutral operation to `execute-method`; Resource, Task, and Result schemas resolve one another through their real v2 `$id` values. Foundation validates the JSON-safe exchange shape, while the consumer owns method identifiers, parameter schemas, and domain results.

The subpath is public but **not stable**. Its schemas remain absent from `src/registry.json`, do not expand the eighteen stable object classes, and may change or be removed in a later minor release. Pin exactly `0.4.0` when evaluating v2. Integrations that still require candidate v1 must stay pinned to exactly `0.2.1`.

## Typical Use Cases

- Need to validate whether a contract document conforms to a registered Schema: use `validateDocument`.
- Need to look up a Schema by object name or `$id`, or look up a Kernel Protocol by protocol name: use `loadRegistry` / `findSchemaByObject` / `findProtocol`.
- Need to run mandatory mechanical rules and collect unresolved references: use `runChecks` / `collectUnresolvedRefs`.
- Need to enumerate and validate the public fixtures: use `verifyAllFixtures`.

## Eighteen Top-Level Object Classes

| Object | `$id` | Schema File |
| --- | --- | --- |
| `project-manifest` | `https://contracts.skill-family.example/v1/project-manifest.json` | `src/schemas/project-manifest.schema.json` |
| `profile-descriptor` | `https://contracts.skill-family.example/v1/profile-descriptor.json` | `src/schemas/profile-descriptor.schema.json` |
| `managed-file-lock` | `https://contracts.skill-family.example/v1/managed-file-lock.json` | `src/schemas/managed-file-lock.schema.json` |
| `operation-request` | `https://contracts.skill-family.example/v1/operation-request.json` | `src/schemas/operation-request.schema.json` |
| `operation-result` | `https://contracts.skill-family.example/v1/operation-result.json` | `src/schemas/operation-result.schema.json` |
| `migration-manifest` | `https://contracts.skill-family.example/v1/migration-manifest.json` | `src/schemas/migration-manifest.schema.json` |
| `report-model` | `https://contracts.skill-family.example/v1/report-model.json` | `src/schemas/report-model.schema.json` |
| `report-binding` | `https://contracts.skill-family.example/v1/report-binding.json` | `src/schemas/report-binding.schema.json` |
| `host-descriptor` | `https://contracts.skill-family.example/v1/host-descriptor.json` | `src/schemas/host-descriptor.schema.json` |
| `host-capability-fact` | `https://contracts.skill-family.example/v1/host-capability-fact.json` | `src/schemas/host-capability-fact.schema.json` |
| `adapter-build-manifest` | `https://contracts.skill-family.example/v1/adapter-build-manifest.json` | `src/schemas/adapter-build-manifest.schema.json` |
| `host-operation-plan` | `https://contracts.skill-family.example/v1/host-operation-plan.json` | `src/schemas/host-operation-plan.schema.json` |
| `host-operation-receipt` | `https://contracts.skill-family.example/v1/host-operation-receipt.json` | `src/schemas/host-operation-receipt.schema.json` |
| `adapter-source` | `https://contracts.skill-family.example/v1/adapter-source.json` | `src/schemas/adapter-source.schema.json` |
| `host-registry` | `https://contracts.skill-family.example/v1/host-registry.json` | `src/schemas/host-registry.schema.json` |
| `host-probe-result` | `https://contracts.skill-family.example/v1/host-probe-result.json` | `src/schemas/host-probe-result.schema.json` |
| `state-event-envelope` | `https://contracts.skill-family.example/v1/state-event-envelope.json` | `src/schemas/state-event-envelope.schema.json` |
| `state-snapshot-metadata` | `https://contracts.skill-family.example/v1/state-snapshot-metadata.json` | `src/schemas/state-snapshot-metadata.json` |

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
  ERROR_CODES, ContractsError, stableError,
} from "skill-family-contracts";
```

The imports above list the stable public surface of this package; `validateDocument` and `runChecks` are the most commonly used entry points. `validateDocument(document, { schemaId | schema, dialect, policy })` returns `{ valid, errorCode, errors, data }`; `runChecks({ rules?, registry?, fixtures?, loadSchema? })` returns `{ ok, mandatoryCount, budget, results }`; `registerSchema` / `registerProtocol` return a new registry copy, throwing `ContractsError` with `SFC1003` / `SFC1004` respectively on duplicates.

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

- `foundation.contracts.object-validation`: Ajv dual-dialect validation of the 20 object classes.
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

- The set of 20 top-level object classes is fixed; additions require an ADR; the error-code freeze does not drift.
- The validator is exclusively Ajv 8.20.0 (exact pin); no other implementation is accepted.

### Route elsewhere when

- Consumer business Schema validation: stays with the caller.
- Remote publish writing: route to release-skill.
- Domain audit semantics: route to a standalone audit consumer.

### Machine-readable sources

- Public capability catalog: [`capability-catalog.json`](https://ifoohoo.github.io/skill-family-engineering-kit/agents/capability-catalog.json) (`foundation.contracts.*` entries).
- Package-local structural contract: `src/registry.json`, `src/schemas/*`.
- Package-local candidate source: `candidate/quickstart-profile/*`; public import: `skill-family-contracts/candidate/quickstart-profile`.
<!-- agent-quick-reference:end -->
