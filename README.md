<!-- release-skill:safe-first-command -->
<!-- release-skill:external-write-boundary -->

# skill-family-contracts

机器可执行工程结构和机制协议的唯一权威包（Contracts v1，冻结）。

本包拥有：五类顶层对象的 JSON Schema、Kernel Protocol（内核协议）、稳定错误码、
协议名/`$id` 登记表，以及九种有限机械检查类型与受限强制规则集。
本包不执行骨架生成、文件写入、审计或发布；机制实现由 Harness 承担，
工程命令由 Kit 承担，二者单向消费本包。

Schema 验证完全基于 [Ajv](https://ajv.js.org/)（精确版本见 `package.json`），
按方言路由到对应 Ajv 类；不实现任何手写 Schema 子集解释器。

## 五类顶层对象

| 对象 | `$id` | Schema 文件 |
| --- | --- | --- |
| `project-manifest` | `https://contracts.skill-family.example/v1/project-manifest.json` | `src/schemas/project-manifest.schema.json` |
| `profile-descriptor` | `https://contracts.skill-family.example/v1/profile-descriptor.json` | `src/schemas/profile-descriptor.schema.json` |
| `managed-file-lock` | `https://contracts.skill-family.example/v1/managed-file-lock.json` | `src/schemas/managed-file-lock.schema.json` |
| `operation-request` | `https://contracts.skill-family.example/v1/operation-request.json` | `src/schemas/operation-request.schema.json` |
| `operation-result` | `https://contracts.skill-family.example/v1/operation-result.json` | `src/schemas/operation-result.schema.json` |

所有 v1 Schema 使用 draft 2020-12 方言；实例信封统一为
`schemaVersion: 1` + 唯一 `kind` 常量 + 各层 `additionalProperties: false`。
`$id` 命名空间 `contracts.skill-family.example` 使用保留示例域，永不解析到真实站点。

## Kernel Protocol（内核协议）

登记表：`src/registry.json`；冻结定义：`src/kernel-protocol.json`。

- 协议名：`skill-family.kernel.operation`，版本 `1`，状态 `stable`。
- 状态集：`accepted`、`running`、`succeeded`、`failed`、`rejected`；
  终态为 `succeeded`、`failed`、`rejected`；`operation-result` 只携带终态。
- 转移：`accepted → running → succeeded|failed`，另允许 `accepted → failed`；
  入口可直接 `rejected`。
- v1 操作词汇表只冻结 `validate`，其 params 合同在
  `kernel-protocol.json` 内定义（`schemaId` + `document` 必填）。
  新增操作名属于合同变更，需新版本登记。

重名协议与重复 `$id` 被机械拒绝：`registerProtocol` 抛出 `SFC1004`，
`registerSchema` 抛出 `SFC1003`；检查类型 `protocol.unique-name` 与
`schema.unique-id` 对登记表做同样判定。

## 稳定错误码

冻结登记表：`src/error-codes.json`。`SFC1xxx` 为合同权威层错误，
`SFC2xxx` 为内核操作错误。码只增不改、不复用。v1 冻结：

| 码 | 名称 | 含义摘要 |
| --- | --- | --- |
| SFC1001 | SCHEMA_VALIDATION_FAILED | 文档未通过目标 Schema 验证 |
| SFC1002 | UNKNOWN_SCHEMA_ID | `$id` 未在登记表注册 |
| SFC1003 | DUPLICATE_SCHEMA_ID | 重复 `$id` 注册被拒绝 |
| SFC1004 | DUPLICATE_PROTOCOL_NAME | 重复协议名/版本注册被拒绝 |
| SFC1005 | UNRESOLVED_REF | `$ref` 目标无法解析 |
| SFC1006 | UNSUPPORTED_DIALECT | 方言不在冻结支持集 |
| SFC1007 | UNKNOWN_CHECK_TYPE | 规则使用九类之外的检查类型 |
| SFC1008 | RULE_BUDGET_EXCEEDED | 强制规则数超出预算/上限 |
| SFC1009 | UNKNOWN_ERROR_CODE | 引用了未登记的错误码 |
| SFC1010 | FIXTURE_EXPECTATION_MISMATCH | fixture 行为与声明期望不符 |
| SFC1011 | UNKNOWN_PROTOCOL | 请求引用未登记的协议名/版本 |
| SFC1012 | SCHEMA_COMPILE_FAILED | Schema 本身无法编译 |
| SFC2002 | UNKNOWN_OPERATION | 操作名不在冻结词汇表 |
| SFC2003 | INVALID_PARAMS | 参数不满足操作的冻结 params 合同 |
| SFC2004 | EXECUTION_FAILED | 机制运行时执行失败（仅运行时可演示） |

## 方言与验证策略（Ajv）

- 支持方言：`draft-07`、`2020-12`。draft 识别通过 `$schema` URI 映射
  （`detectDialect`），验证按方言路由到对应 Ajv 类。
- 验证策略（`VALIDATION_POLICIES`）：
  - `strict`（默认）：不做类型强制、不注入默认值，Ajv 严格模式全开；
  - `tolerant`：开启 Ajv `coerceTypes: "array"` 与 `useDefaults`，用于采纳场景。
- 格式：`date-time`（RFC 3339，含日历合法性检查）经 Ajv `addFormat` 登记。
- `validateDocument` 永不修改调用者输入；规范化后的副本在结果的 `data` 字段返回。

## 九类机械检查与规则预算

登记表：`src/rules.json`。检查类型集合封闭，共九种：
`schema.compile`、`schema.unique-id`、`protocol.unique-name`、
`schema.ref-resolves`、`schema.dialect-declared`、`fixture.positive-passes`、
`fixture.negative-coded`、`error-code.registered`、`rules.budget`。

首版强制规则 **13 条**（CR-001…CR-013），预算上限 20 条、绝对上限 30 条；
`rules.budget` 是机械门禁，超限即 `runChecks` 失败并报 `SFC1008`。

## Fixture

`src/fixtures/<contract>/` 为每类合同提供正例（positive）、反例（negative）
与方言边界（dialect-boundary）样例，共 23 个。每个 fixture 声明目标 Schema、
方言、策略与期望；反例期望携带稳定失败码。`verifyAllFixtures()` 机械重放全部期望，
行为不符报 `SFC1010`。fixture 是完全虚构数据，不是审计 oracle。

## API 概览

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

- `validateDocument(document, { schemaId | schema, dialect, policy })` →
  `{ valid, errorCode, errors, data }`；
- `runChecks({ rules?, registry?, fixtures?, loadSchema? })` →
  `{ ok, mandatoryCount, budget, results }`；
- `registerSchema` / `registerProtocol` 返回新登记表副本，重复项分别以
  `SFC1003` / `SFC1004` 抛出 `ContractsError`。

## 边界与非目标

不拥有生成、语义审计、发布状态与远端写入；不定义清理计划、发布快照、
消费者冒烟结果、领域审计报告或自由文本规则语言；不建通用 DSL。
冻结内容的变更只能作为新的合同版本任务进行。

## 安装

```sh
npm install skill-family-contracts
```

## 最小示例

```js
// release-skill: validate a document against its registered schema
import { validateDocument } from "skill-family-contracts";
const result = validateDocument(doc, { schemaId: "project-manifest" });
if (!result.valid) console.error(result.errorCode);
```

## 故障诊断

验证失败时 `errorCode` 为 `SCHEMA_VALIDATION_FAILED`（SFC1001）；未知 Schema 报 `UNKNOWN_SCHEMA_ID`（SFC1002）。如失败，检查文档是否满足目标 Schema 的必填字段与类型约束。
