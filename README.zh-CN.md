<!-- release-skill:safe-first-command -->
<!-- release-skill:external-write-boundary -->

> English version: [README.md](./README.md)

# skill-family-contracts

<!-- release-skill:release-version: 0.16.0 -->

机器可执行工程结构和机制协议的唯一权威包（源码候选：Contracts 1.15.0）。

<!-- release-skill:managed:start id=latest-release -->
**0.16.0** (2026-08-31)

Contracts 0.16.0 保持现有合同面，并将消费方向量与 Foundation 0.16.0 锁步版本对齐。

**变更**

- 将消费方契约测试向量的版本坐标更新为 0.16.0，不新增合同对象，也不改变校验语义。

**升级说明**

三个 Foundation 包须一起精确锁定到 0.16.0；消费方接受规则与真实宿主观察仍由调用方负责。
<!-- release-skill:managed:end id=latest-release -->

## 解决的问题

技能族项目各自写一套结构契约，会出现 Schema 漂移、错误码不一致、协议名冲突。Contracts 把结构、协议、错误码与协议名登记收敛成一份冻结的机器可读权威，让 Harness 与 Kit 单向消费，不再各自解释。

## 核心心智模型

Contracts 是「定义与登记」层，不是「执行」层。它拥有 45 类顶层对象的 JSON Schema，其中包括 Project Profile、共享的 profile-adoption 定义、文件系统绑定、固定集合发布、同级适配器验证、可执行身份与技能族目录验证对象；同时拥有 Kernel Protocol（内核协议）、稳定错误码、协议名与 `$id` 登记表，以及九种有限机械检查类型与受限强制规则集。本包不执行骨架生成、文件写入、审计或发布；机制实现由 Harness 承担，工程命令由 Kit 承担。

Schema 验证完全基于 [Ajv](https://ajv.js.org/)（精确版本见 `package.json`），按方言路由到对应 Ajv 类；不实现任何手写 Schema 子集解释器。

## 安装和最小示例

0.16.0 是本地候选版本。候选验证先把三个包分别打入同一个临时目录，再安装这三个精确 tarball：

```sh
pack_dir="$(mktemp -d)"
(cd packages/skill-family-contracts && pnpm pack --pack-destination "$pack_dir")
(cd packages/skill-family-harness-node && pnpm pack --pack-destination "$pack_dir")
(cd packages/skill-family-engineering-kit && pnpm pack --pack-destination "$pack_dir")
mkdir "$pack_dir/consumer" && (cd "$pack_dir/consumer" && npm init -y)
(cd "$pack_dir/consumer" && npm install "$pack_dir/skill-family-contracts-0.16.0.tgz" "$pack_dir/skill-family-harness-node-0.16.0.tgz" "$pack_dir/skill-family-engineering-kit-0.16.0.tgz")
```

发布后再使用 registry 坐标：

```sh
npm install skill-family-contracts@0.16.0
npm info skill-family-contracts --help
```

最小示例从空目录开始，演示如何校验一份已登记契约对象：

```js
// 发布后从已安装的消费者目录运行。
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

以上代码展示了 `validateDocument` 的基本调用：传入文档与目标 Schema 的 `$id`，返回 `{ valid, errorCode, errors, data }`，其中 `data` 是规范化后的副本，原输入不被修改。

## 外置来源权威

`parseSourceAuthorityReceipt(receipt, actualSubjects)` 校验调用方提供的 receipt，并逐项核对实际 subjects 的包名、版本、文件名与 SHA-256。receipt 内的 subjects 必须按 `packageName` 唯一、确定排序；实际 subjects 的输入顺序不限。校验成功时，`data` 只返回 `{ sourceRepository, sourceBaseCommit }`。Contracts 不发现包、不执行目标，也不创建发布状态。

## 消费方契约测试向量

Foundation 0.14.0 提供两组带精确版本身份的消费方契约测试向量。`listConsumerContractVectors` 返回按 `vectorId` 排序的深度冻结数组；`verifyConsumerContractVector` 在消费方代码运行前核对向量结构、`capabilityId`、`vectorSetId` 和 `FOUNDATION_PACKAGE_VERSION`。

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
// 消费方适配器代码只能在身份和结构校验通过后运行。
const consumerResult = invokeConsumerAdapter(vectors[0]);
```

首组向量覆盖 `foundation.contracts.object-validation`，第二组覆盖 `foundation.harness.atomic-write`。每组都包含正例、反例和不确定类。版本或身份不匹配时以 `SFC1013`（`CONSUMER_CONTRACT_VERSION_MISMATCH`）失败关闭。Contracts 只校验公开向量的结构和身份，不调用消费方、Harness 实现、测试替身（`fake`）或宿主。

`foundation.harness.atomic-write` 的正例只声明闭合的 `valueRelation: "atomic-write-contained-absolute-target"`，不把路径值写入向量。运行时含义由 Harness 和消费方测试验证，Contracts 不执行或解释它。

对于 `foundation.harness.atomic-write`，每个 `request.root` 都是闭合声明 `{ runtimeBinding: "atomic-write-canonical-root" }`，不是路径，Contracts 不求值。反例在 `outcome` 和 `errorCode` 旁增加稳定的 `errorKind: "path-traversal"`；通用 throw 向量仍保持只含两个字段的闭对象。

能力特定 Schema 还会拒绝非 atomic 向量携带这个 atomic binding；除此之外，不扩大对其他 request 字段的限制。

## Candidate Quickstart Profile

需要在进入冻结登记表前评估早期 Resource → Task → Result 交换时，使用 candidate Quickstart Profile：

```js
import {
  QUICKSTART_PROTOCOL,
  quickstartProfileSchemas,
  validateQuickstartProfileDocument,
} from "skill-family-contracts/quickstart-profile";
```

0.4.0 携带 Quickstart Profile v2。协议把业务中立的操作固定为 `execute-method`，Resource、Task、Result Schema 通过真实的 v2 `$id` 互相解析。Foundation 只校验 JSON-safe 交换结构；方法标识、参数 Schema 与领域结果继续归消费者所有。

该能力仍是 **candidate**，Schema 不进入 `src/registry.json`。评估时必须精确锁定三个 Foundation 包。0.10.0 新增上面的规范入口；历史 `/candidate/quickstart-profile` 入口作为同源迁移别名继续可用。消费者迁移一次后，未来晋升 stable 不再切入口。仍依赖 candidate v1 的接入必须继续精确锁定 `0.2.1`。

## 典型使用场景

- 需要校验某份契约文档是否符合已登记 Schema：用 `validateDocument`。
- 需要按对象名或 `$id` 查找 Schema、按协议名查找 Kernel 协议：用 `loadRegistry` / `findSchemaByObject` / `findProtocol`。
- 需要运行强制机械规则、收集未解析引用：用 `runChecks` / `collectUnresolvedRefs`。
- 需要枚举并校验公开 fixture：用 `verifyAllFixtures`。

## 已登记顶层对象类

| 对象 | `$id` | Schema 文件 |
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

plugin-verification 合同保留既有 `install-only` 与 `install-and-invoke` 目标，并增加 `native-lifecycle` 分支。原生命周期结果恰好包含十二个有序语义阶段。Contracts 只检查闭合结构、阶段顺序、停止传播，以及后续 `not-performed` 阶段的 `commands` 与 `trees` 为空；不定义跨宿主统一命令计划，不解释厂商输出，不暴露 `hostState`，也不拥有 Qoder 与 WorkBuddy 的 Oracle。

skill-family-directory request/result 合同描述 Kimi 目录边界，不接收调用方提供的 observation。官方观察映射不可用时，合同允许结果保持 `indeterminate`、原因为 `official-observation-unavailable`，逐技能事实保持 unknown。进程执行、固定 argv 与环境、原始流解析，以及未来可能取得的官方观察映射都归 Engineering Kit，不归 Contracts。


所有 v1 Schema 使用 draft 2020-12 方言；实例信封统一为 `schemaVersion: 1` + 唯一 `kind` 常量 + 各层 `additionalProperties: false`。`$id` 命名空间 `contracts.skill-family.example` 使用保留示例域，永不解析到真实站点。

## Kernel Protocol（内核协议）

登记表：`src/registry.json`；冻结定义：`src/kernel-protocol.json`。

- 协议名：`skill-family.kernel.operation`，版本 `1`，状态 `stable`。
- 状态集：`accepted`、`running`、`succeeded`、`failed`、`rejected`；终态为 `succeeded`、`failed`、`rejected`；`operation-result` 只携带终态。
- 转移：`accepted → running → succeeded|failed`，另允许 `accepted → failed`；入口可直接 `rejected`。
- v1 操作词汇表只冻结 `validate`，其 params 合同在 `kernel-protocol.json` 内定义（`schemaId` + `document` 必填）。新增操作名属于合同变更，需新版本登记。

重名协议与重复 `$id` 被机械拒绝：`registerProtocol` 抛出 `SFC1004`，`registerSchema` 抛出 `SFC1003`；检查类型 `protocol.unique-name` 与 `schema.unique-id` 对登记表做同样判定。

## 稳定错误码

冻结登记表：`src/error-codes.json`。`SFC1xxx` 为合同权威层错误，`SFC2xxx` 为内核操作错误，`SFC3xxx` 为报告绑定错误。码只增不改、不复用。v1 冻结：

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
| SFC1013 | CONSUMER_CONTRACT_VERSION_MISMATCH | 消费方向量身份或 Foundation 精确版本不匹配 |
| SFC2002 | UNKNOWN_OPERATION | 操作名不在冻结词汇表 |
| SFC2003 | INVALID_PARAMS | 参数不满足操作的冻结 params 合同 |
| SFC2004 | EXECUTION_FAILED | 机制运行时执行失败（仅运行时可演示） |
| SFC3001 | REPORT_DIGEST_MISMATCH | 报告或结果摘要与绑定不一致 |
| SFC3002 | REPORT_ELEMENT_MISSING | 报告缺少强制元素 |
| SFC3003 | REPORT_FACT_DRIFT | 报告字节偏离确定性重渲染结果 |

## 方言与验证策略（Ajv）

- 支持方言：`draft-07`、`2020-12`。draft 识别通过 `$schema` URI 映射（`detectDialect`），验证按方言路由到对应 Ajv 类。
- 验证策略（`VALIDATION_POLICIES`）：
  - `strict`（默认）：不做类型强制、不注入默认值，Ajv 严格模式全开；
  - `tolerant`：开启 Ajv `coerceTypes: "array"` 与 `useDefaults`，用于采纳场景。
- 格式：`date-time`（RFC 3339，含日历合法性检查）经 Ajv `addFormat` 登记。
- `validateDocument` 永不修改调用者输入；规范化后的副本在结果的 `data` 字段返回。

## 九类机械检查与规则预算

登记表：`src/rules.json`。检查类型集合封闭，共九种：`schema.compile`、`schema.unique-id`、`protocol.unique-name`、`schema.ref-resolves`、`schema.dialect-declared`、`fixture.positive-passes`、`fixture.negative-coded`、`error-code.registered`、`rules.budget`。

当前强制规则 **9 条**（CR-001、CR-006…CR-013）。其中 CR-001 对登记表内全部 Schema 做统一编译，不再为每个对象重复占用一条规则；预算上限 20 条、绝对上限 30 条；`rules.budget` 是机械门禁，超限即 `runChecks` 失败并报 `SFC1008`。

## Fixture

`src/fixtures/<contract>/` 为每类合同提供正例（positive）、反例（negative）与方言边界（dialect-boundary）样例。每个 fixture 声明目标 Schema、方言、策略与期望；反例期望携带稳定失败码。`verifyAllFixtures()` 机械重放全部期望，行为不符报 `SFC1010`。fixture 是完全虚构数据，不是审计 oracle。

## API 概览

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

以上导入列出了本包稳定公共面；`validateDocument` 与 `runChecks` 是最常用入口。`validateDocument(document, { schemaId | schema, dialect, policy })` 返回 `{ valid, errorCode, errors, data }`；`runChecks({ rules?, registry?, fixtures?, loadSchema? })` 返回 `{ ok, mandatoryCount, budget, results }`；`registerSchema` / `registerProtocol` 返回新登记表副本，重复项分别以 `SFC1003` / `SFC1004` 抛出 `ContractsError`。

`detectDialect(schema)` 在声明缺失或未知时返回 `null`，支持的结果为 `draft-07` 或 `2020-12`。不支持的方言由 `compileSchema` 抛出 `SFC1006`，由 `validateDocument` 以 `errorCode: "SFC1006"` 返回。登记表查询未命中返回 `null`。随包登记表的 `schemaVersion=1`、`contractsVersion=1.15.0`、45 类 Schema 与 1 个 Kernel Protocol 以机器源为准；注册函数返回副本且不修改输入，消费方向量身份或精确版本不匹配时报 `SFC1013`。

## 安全边界与非目标

不拥有生成、语义审计、发布状态与远端写入；不定义清理计划、发布快照、消费者冒烟结果、领域审计报告或自由文本规则语言；不建通用 DSL。冻结内容的变更只能作为新的合同版本任务进行。

## 故障诊断

验证失败时 `errorCode` 为 `SFC1001`（SCHEMA_VALIDATION_FAILED，文档未通过目标 Schema 验证）；`$id` 未注册时报 `SFC1002`（UNKNOWN_SCHEMA_ID）。如失败，检查文档是否满足目标 Schema 的必填字段与类型约束。

## 深入文档入口

- 架构边界与路由：[架构说明](https://ifoohoo.github.io/skill-family-engineering-kit/architecture/)、[智能体架构路由](https://ifoohoo.github.io/skill-family-engineering-kit/agents/architecture-routing/)
- 能力目录：[capability-catalog.json](https://ifoohoo.github.io/skill-family-engineering-kit/agents/capability-catalog.json)
- 当前产品状态：[公开状态](https://ifoohoo.github.io/skill-family-engineering-kit/public/status/)

<!-- agent-quick-reference:start -->
## Agent Quick Reference

### Use when

- 需要校验已登记契约对象、查找 Schema/协议、运行强制机械规则。
- 需要枚举并校验公开 fixture，或确定性序列化契约表面。
- 需要在锁定精确包版本后评估非稳定的 Quickstart Resource/Task/Result Profile。

### Do not use when

- 需要校验消费者自有业务 Schema（消费者应自行持有，Foundation 不取代）。
- 需要把领域语义校验混入通用契约。
- 需要兼容性已冻结的 Quickstart Profile；candidate 子路径尚未登记为 stable。

### Capability selection

- `foundation.contracts.object-validation`：Ajv 双方言校验已登记的全部 45 类顶层对象。
- `foundation.contracts.registry-protocol`：Schema `$id` 与协议名登记查询。
- `foundation.contracts.kernel-protocol`：operation-request/result 协议。
- `foundation.contracts.mandatory-checks`：九类强制规则与未解析引用。
- `foundation.contracts.fixture-verification`：fixture 全量回放。
- `foundation.contracts.error-codes`：稳定错误码体系。
- `foundation.contracts.audit-surface`：canonical JSON + sha256 摘要。
- `foundation.contracts.quickstart-profile-candidate`：通过精确版本子路径使用 candidate-only Resource/Task/Result Schema 与校验。

### Required inputs

- 待校验文档（带已登记 `$id`）或目标契约对象名。
- 校验策略 `strict`（默认）或 `tolerant`。

### Outputs and evidence

- `validateDocument` 返回 `{ valid, errorCode, errors, data }`。
- 证据：`packages/skill-family-contracts/test/validator.test.mjs`、`registry.test.mjs`、`checker.test.mjs`、`fixtures.test.mjs`。

### Side effects

- 纯函数，无文件系统、Git、网络或进程副作用（编译缓存仅驻留内存）。

### Failure semantics

- `SFC1001/1002/1006` 等稳定错误码，错误对象含 `stableError` 与 `details.kind`。
- 冻结错误码只增不改，不漂移。

### Architectural invariants

- 45 类顶层对象集合固定，新增需 ADR；Contracts 1.10.0 增加同级适配器验证合同并扩展已登记宿主合同，不新增错误码，错误码冻结不漂移。
- 校验器仅 Ajv 8.20.0（精确 pin），不接受其他实现。

### Route elsewhere when

- 消费者业务 Schema 校验：留在调用方。
- 发布远端写入：转 release-skill。
- 领域审计语义：转独立审计消费者。

### Machine-readable sources

- 公开能力目录：[`capability-catalog.json`](https://ifoohoo.github.io/skill-family-engineering-kit/agents/capability-catalog.json)（`foundation.contracts.*` 条目）。
- 包内结构合同：`src/registry.json`、`src/schemas/*`。
- 包内 Candidate 源：`candidate/quickstart-profile/*`；规范公共导入：`skill-family-contracts/quickstart-profile`；历史迁移别名：`skill-family-contracts/candidate/quickstart-profile`。
<!-- agent-quick-reference:end -->

## 完整插件候选能力

完整插件请求、结果与完整树观察使用三个新增候选 Schema。安装、发现、调用与载荷比较分别表达；原始树内容属于私有数据。

0.16.0 为本地源码候选，尚未发布。消费本地已验证的三包 tarball；版本标记、单元测试或安装成功都不等于契约接入完成、迁移完成或真实宿主资格。
