# 变更日志

<!-- release-skill:changelog:start version=0.8.2 locale=zh-CN baseline=sha256:29a5b9a3e8227837c90b1ce74b84ca5036b3f2ba25e2909f17f951f709e208b3 -->
## [0.8.2] - 2026-08-23

随 Foundation 0.8.2 锁步升版；Contracts 1.7.0 机器合同保持不变。

### 变更

- 包版本与 Harness、Engineering Kit 一同升至 0.8.2。
- Contracts 1.7.0、31 类顶层对象登记、Schema、错误码和公共导出均保持不变。

### 升级说明

消费者必须把三个 Foundation 包精确锁定到 0.8.2，再重建受管 Bundle；从 0.8.1 升级不需要迁移 Contracts API 或规格。
<!-- release-skill:changelog:end version=0.8.2 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.8.1 locale=zh-CN baseline=sha256:083c386c488815200bf1974601a773d646dd6b436177c370311b804e99cafd16 -->
## [0.8.1] - 2026-08-22

随 Foundation 0.8 补丁线锁步升版；Contracts 1.7.0 机器合同保持不变。

### 变更

- 包版本与 Harness、Engineering Kit 一同升至 0.8.1。
- Contracts 1.7.0、31 类顶层对象登记、Schema、错误码和公共导出均保持不变。

### 升级说明

消费者可精确锁定 0.8.1，继续使用 Foundation 锁步补丁线；从 0.8.0 升级不需要迁移 Contracts API 或规格。
<!-- release-skill:changelog:end version=0.8.1 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.8.0 locale=zh-CN baseline=sha256:d39705795b0bdd9320beba7d6adba38e546fa11649ab8218f4607dd455a904ae -->
## [0.8.0] - 2026-08-21

Contracts 1.7.0 新增 Project Profile 合同，包版本随 Foundation 0.8.0 线锁步。

### 新增

- 新增 project-profile 合同，顶层对象登记表从 30 类增至 31 类。
- 将 Contracts profile-adoption-declaration 的 $defs 确定为 adoption 与 overrides 的唯一字段形状权威；SPI 文件只保留为兼容转发表。

### 升级说明

需要校验 scaffold 项目根的消费者必须采用 Contracts 1.7.0。foundation_pin 中的包版本仍填写 Foundation npm 包的精确版本 0.8.0，不填写 Contracts 规格版本。
<!-- release-skill:changelog:end version=0.8.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.7.0 locale=zh-CN baseline=sha256:eacdf2f80fce86c5e7ed165a676ba3b37d0216a7b94cbed2b2d03422167880a5 -->
## [0.7.0] - 2026-08-21

随 Foundation 0.7.0 线锁步升版；机器合同不变。

### 变更

- 机器合同无变更——CONTRACTS_VERSION 保持 1.6.0，30 类顶层对象登记表、九条 mandatory rule 与已登记的错误码和协议名与 0.6.0 完全一致；包版本随 Foundation 线锁步，因为三个叶子包共用同一公开版本坐标。

### 升级说明

0.7.0 不携带任何 contracts 表面变更。锁定契约规格 1.6.0 的消费者无需调整校验；审计基线 pin 仍为 contracts-1.6.0.pin.json。
<!-- release-skill:changelog:end version=0.7.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.6.0 locale=zh-CN baseline=sha256:e86b06f1c37ba4849288bea4659150c0a08a4a514a0953a057ef22a2f2bbbcb8 -->
## [0.6.0] - 2026-08-21

本版把稳定 Contracts 登记表从 24 类扩到 30 类顶层对象，契约规格版本升到 1.6.0（审计整改 C5），新增 append-only 审计基线 pin 合同及其消费侧校验，并新增 token 估算记录的最小消费合同。

### 新增

- 登记六个新顶层契约类（24 → 30）：public-boundary-declaration、platform-difference-registry、observation-scope、profile-adoption-declaration、audit-baseline-pin 与 token-estimate-record，契约规格 1.6.0。
- 新增 append-only 审计基线 pin 合同与消费面：BASELINE_PIN_KINDS、describeBaselinePin 与 verifyBaselinePin（AUD-BASE-001 / AUD-LOCK-001）；contracts-1.6.0.pin.json 为现行 pin，更早的 pin 保留为只读存档。
- 新增 token 估算记录的最小消费合同（SG-33）：TOKEN_ESTIMATE_CONSUMPTION、TOKEN_ESTIMATE_CONSUMPTION_REASONS、TOKEN_ESTIMATE_CONSUMPTION_ERROR_KIND、consumeTokenEstimate 与 consumeTokenEstimateStrict，携带 consumptionTarget 整数字段与失败关闭拒绝语义，并补齐 negative-03/04 与 positive-02 fixtures。

### 变更

- CONTRACTS_VERSION 升到 1.6.0；审计表面现投影 30 类登记表与基线 pin 机制，集成审计 verify-lock 指向 1.6.0 pin。
- quickstart-profile 稳定基线按 1.6.0 登记表重录（F2）；方法标识、参数 Schema 与领域结果语义继续归消费者所有。

### 升级说明

0.6.0 是审计整改契约线。校验六个新对象类的消费者必须锁定契约规格 1.6.0，并按 30 类登记表校验；审计消费者按 contracts-1.6.0.pin.json 校验。
<!-- release-skill:changelog:end version=0.6.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.5.0 locale=zh-CN baseline=sha256:864f205c202a7261c33ba8ac26fadd22b30d8f10c9659ec3c5c893284f67eb79 -->
## [0.5.0] - 2026-08-16

本版把稳定 Contracts 登记表从 20 类扩到 22 类顶层对象，契约规格版本升到 1.5.0（FND-ADR-010 与 FND-ADR-011），新增 declared-read-surface-result 与 structured-scan-policy 两个契约文档。

### 新增

- 新增 declared-read-surface-result：harness assertDeclaredReadSurface 机制的结果信封（FND-ADR-010），违规词汇表为闭集三值，guarantees 为闭集五值枚举。
- 新增 structured-scan-policy：harness 结构化表面扫描器的声明式策略（FND-ADR-011），携带 allowedNetworks、approvedRegistries、approvedCoordinates、formatAdapters、symlinkPolicy、binaryPolicy 与可选 hostKeyPattern。
- 在稳定 SFC2004 机制错误下新增 structured-scan-violation 与 structured-scan-invalid 错误码种类，与既有 declared-read-surface 种类并列；规则类别经 details.rule 承载，不逐类别造码。
- 契约规格升到 1.5.0，新增 append-only 审计基线 pin（contracts-1.5.0.pin.json）；1.4.0 pin 保留为只读存档。

### 变更

- surface-scan-policy 与 structured-scan-policy 的 schema description 现在显式说明公开策略文档与工作区私有 leak 策略的关系：工作区私有的 leak-policy.json 实例文档既不是这些 schema 的子集、也不同构、更不是迁移目标——两类文档按设计共享规则词汇与失败关闭语义，但字节级形状相互独立，不得比较兼容性。scanSurface 是执行内核通用化投影：同一机制族的公开、消费者参数化形态，自身不解释任何私有身份、路径或批准清单。
- 方法标识、参数 Schema 与领域结果语义继续归消费者所有。

### 升级说明

0.5.0 是 FND-ADR-010/011 契约线。校验 declared-read-surface-result 或 structured-scan-policy 的消费者必须锁定契约规格 1.5.0，并按 22 类登记表校验。
<!-- release-skill:changelog:end version=0.5.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.4.0 locale=zh-CN baseline=sha256:99c7f1d548dff69c87156f903efe5b05fa214299c01c9db4d79b0a322fe8ee5d -->
## [0.4.0] - 2026-08-16

本版在契约规格版本 1.4.0 不变的前提下把稳定 Contracts 登记表扩到 20 类顶层对象，补齐有限封闭语义（FND-ADR-009）的 fixtures 与 schema，并新增 fixed-set-publication 与 schema 清单候选。

### 新增

- 新增两个 stable 顶层对象：token-estimate-result（确定性 UTF-8 字节计数估算结果，封闭 guarantees 枚举）与 surface-scan-policy（路径/内容模式 + 只携带不解释的 allowedUses），对象登记从 18 类增至 20 类，契约规格版本保持 1.4.0（FND-ADR-009）。
- migration-manifest schema 增加遗留引用支持，并补齐对应 fixtures（negative-06、negative-07、negative-08、positive-03）。
- 新增 fixed-set-publication 候选（manifest/receipt schema、fixtures 与覆盖 native-prebuild 运行时的能力适配声明），不进入稳定登记表。
- Quickstart Profile 候选扩展 consumer-schema-inventory 与 harness-surface-inventory schema 及 inventory fixtures。
- 落地 FND-ADR-001 门 1 放宽（一个现有 + 一个结构同型可预见消费者）与门 8「有限封闭语义」（FND-ADR-009）。

### 变更

- CONTRACTS_VERSION 保持 1.4.0：同一契约规格版本线现在覆盖 20 类对象集，而已发布的 0.3.0 字节携带的是该版本的 18 类对象集。
- 方法标识、参数 Schema 与领域结果语义继续归消费者所有。

### 升级说明

0.4.0 已发布到 npm 与 public 镜像仓。candidate 子路径公开但不稳定；candidate 导入必须精确锁定 0.4.0，稳定对象按 20 类登记表契约校验。
<!-- release-skill:changelog:end version=0.4.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.3.0 locale=zh-CN baseline=sha256:5f1c4cf56cc336279cbc6f11bc6cf8ce0a7524f454ff6589e067e828707a7cc7 -->
## [0.3.0] - 2026-08-12

本源码候选版以 Quickstart Profile v2 替换原 candidate，同时保持稳定 Contracts 登记表和内核协议不变。

### 新增

- 新增 v2 协议定义，以业务中立的 execute-method 作为唯一操作，并按真实 $id 登记 Resource、Task、Result Schema 集合。
- 收紧 Task 与 Result 的 JSON-safe 边界、单一 path-backed observation、终态输出形状及 evidence 精确回指。

### 变更

- 替换与 0.2.1 不兼容的 candidate 面；仍依赖 v1 的消费者必须继续精确锁定 0.2.1。
- 方法标识、参数 Schema 与领域结果语义继续归消费者所有。

### 升级说明

0.3.0 当前只是本地、未发布的源码候选。candidate 导入必须精确锁定包版本，v1 接入完成迁移后才能选用本版本。
<!-- release-skill:changelog:end version=0.3.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.2.1 locale=zh-CN baseline=sha256:1f9b53564843c024415fda87d41abf96bc22db2ec5b90d12d7a8cd7c897a24fc -->
## [0.2.1] - 2026-08-10

本版新增 Quickstart Profile 候选契约面，并为包发布文档提供完整英文版与简体中文版。

### 新增

- 新增候选 Resource、Task、Result Schema 及其严格校验辅助函数。这组候选 Schema 不进入稳定 Contracts 登记表。
- 新增完整的英文与简体中文包文档，并补充智能体快速参考章节。

### 变更

- 使用同一份双语版本化说明源管理当前 README 与 CHANGELOG 的发布区域。
- 项目 NOTICE 与 Apache-2.0 LICENSE 分开分发。

### 升级说明

稳定登记表的消费者无需修改。Quickstart Profile 只能通过 candidate 子路径导入，不得把它当作已冻结的 Contracts 对象。
<!-- release-skill:changelog:end version=0.2.1 locale=zh-CN -->
