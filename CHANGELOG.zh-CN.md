# 变更日志

<!-- release-skill:changelog:start version=0.17.0 locale=zh-CN baseline=sha256:526755ba8892da548e7ebb04387c1adebd3ecb581695bf6c9f538ad880f7395c -->
## [0.17.0] - 2026-09-01

Contracts 0.17.0 新增业务中立的 engineering-baseline 合同，并将契约规格升至 1.16.0。

### 新增

- 新增闭合的 engineering-baseline Schema，记录提供方身份、权威规则谱系引用和一个 Foundation 参考骨架身份。
- 新增 validateEngineeringBaseline 与 describeEngineeringBaseline，校验惰性 JSON 输入、规则引用的确定性顺序与唯一性，以及文档自身摘要；不解释提供方拥有的规则语义。

### 变更

- 将消费方契约测试向量更新到 Foundation 0.17.0 锁步坐标。

### 升级说明

三个 Foundation 包须一起精确锁定到 0.17.0。Audit 或其他基线提供方负责规则含义并发布基线文档；Foundation 只校验中立身份与参考绑定，依赖方向保持为提供方依赖 Foundation。
<!-- release-skill:changelog:end version=0.17.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.16.0 locale=zh-CN baseline=sha256:0317e589a23c3d9d16647f4ecec9aace7bda35c2399a2c2d6d1767e3c2bbb4e0 -->
## [0.16.0] - 2026-08-31

Contracts 0.16.0 保持现有合同面，并将消费方向量与 Foundation 0.16.0 锁步版本对齐。

### 变更

- 将消费方契约测试向量的版本坐标更新为 0.16.0，不新增合同对象，也不改变校验语义。

### 升级说明

三个 Foundation 包须一起精确锁定到 0.16.0；消费方接受规则与真实宿主观察仍由调用方负责。
<!-- release-skill:changelog:end version=0.16.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.15.0 locale=zh-CN baseline=sha256:71524174ce3feb8bbe6e19f7ab7b0b712a4e48eebc3467136093931f304aed39 -->
## [0.15.0] - 2026-08-29

Contracts 0.15.0 增加封闭的可执行文件身份与 Kimi 目录验证合同。

### 新增

- 新增 executable-identity-observation 以及 skill-family-directory-verification 请求/结果合同。

### 变更

- 将 Kimi 的 driverVersion 1.0.0 与闭合 CLI 版本 0.39.1 分开。

### 升级说明

三个 Foundation 包须一起精确锁定到 0.15.0；领域接受规则与真实宿主观察仍由消费者负责。
<!-- release-skill:changelog:end version=0.15.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.14.0 locale=zh-CN baseline=sha256:dd29155fc5fa3db2890cb03d81d0630b4c8366736072829648d96d0961c4159c -->
## [0.14.0] - 2026-08-28

Contracts 0.14.0 增加消费者契约测试向量与能力采用字段，继续保持三包锁步。

### 新增

- 新增 consumer-contract-vector Schema、正式向量以及 listConsumerContractVectors/verifyConsumerContractVector 入口。
- 为 migration manifest 增加能力使用与能力决策字段，明确记录采用决策。

### 变更

- Contracts 规格升至 1.14.0；消费者向量仍是候选测试合同，不改变既有宿主验证身份。
- 明确区分候选发现、迁移完成、契约接入完成和真实宿主资格四种结论。

### 升级说明

Contracts、Harness 与 Engineering Kit 须一起精确锁定到 0.14.0。消费者向量只证明契约接线；领域测试与真实宿主资格仍由消费者负责。
<!-- release-skill:changelog:end version=0.14.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.13.0 locale=zh-CN baseline=sha256:4e688ca871352558d1c93ed95ffc496bb25e8341c44089d8cd02c2359ca72264 -->
## [0.13.0] - 2026-08-26

Contracts 0.13.0 源码候选增加完整插件验证与私有文件系统树观察合同。

### 新增

- 以永久 Schema 身份增加 plugin-verification-request、plugin-verification-result 和 filesystem-tree-observation。

### 变更

- Contracts 规格升至 1.13.0，登记 42 类顶层对象。
- 为既有 watchdog 信封增加可选每流输出限额事实；未启用限额的旧调用保持原形状。

### 升级说明

三个包须精确锁定到同一版本。既有单 Skill 宿主验证与 Kernel 1.8.0 保持不变。候选准备不代表宿主资格、独立验收或发布完成。
<!-- release-skill:changelog:end version=0.13.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.12.0 locale=zh-CN baseline=sha256:c69ddf418a927bb24ed0fb8641b71d7ae6bf69b4c0997c5f5dfed419c40d1d00 -->
## [0.12.0] - 2026-08-26

Contracts 0.12.0 将既有宿主验证合同扩展为五组固定的宿主与驱动对应关系。

### 新增

- 在既有两个验证组合之外增加三个固定组合，继续使用 existing-user-state 与 host-managed 凭证语义；精确宿主与驱动名单见宿主能力矩阵。

### 变更

- 宿主输出未通过固定协议检查时，允许退出码为零的调用返回 execution-failed。
- Contracts 规格升级为 1.12.0，不新增顶层对象类别、Schema ID 或错误码。

### 升级说明

三个 Foundation 包须一起升级到 0.12.0。宿主验证仍为 candidate，只报告执行事实，不代表领域审阅通过；Descriptor 中的验证配置不授予自动安装或生命周期能力。
<!-- release-skill:changelog:end version=0.12.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.11.0 locale=zh-CN baseline=sha256:c20d52d377b61fa822dc3616e40ed0f10c157918c4f44d47e448585d48be9823 -->
## [0.11.0] - 2026-08-25

Contracts 0.11.0 增加受约束真实宿主验证所需的候选请求与结果合同。

### 新增

- 新增闭合的宿主验证请求与结果 Schema，明确摘要前像、逐宿主绑定和失败关闭的终态语义。
- 为 Kimi 与 WorkBuddy 登记宿主验证三元组（existing-user-state + host-managed），不增加通用 driver Registry 或认证 SPI。

### 变更

- 把此前准备好的宿主 Profile 闭包并入 0.11.0 三包锁步交付。

### 升级说明

消费者可以验证候选宿主验证合同；0.11.0 实现仍须等待 Kimi 与 WorkBuddy 真实宿主发布门通过后才能发布。
<!-- release-skill:changelog:end version=0.11.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.10.0 locale=zh-CN baseline=sha256:cfa887aada0fdc695e7bd594c104666cbffac7d3b2ad0dc90dcba45a36a583a8 -->
## [0.10.0] - 2026-08-24

Contracts 0.10.0 发布 Contracts 1.10.0 minor 规格，将能力成熟度与消费者规范身份分离，扩展既有宿主合同，并增加业务中立的同级适配器只读验证合同。

### 新增

- 新增 skill-family-contracts/quickstart-profile 规范导出，与历史 candidate 路径使用同一模块。
- 新增机器可读的 candidate 晋升政策与历史 candidate 迁移政策。
- 新增冻结的八项迁移表，把历史 Quickstart 与批量校验 Schema ID 映射到成熟度中立的规范 ID。
- 保持既有宿主 Schema 身份不变，增加手动宿主支持、有限 source alias 和九项独立 probe fact 表达。
- 新增闭集 request/result Schema，验证两个或更多 peer 真实适配器目录的共同闭包、逐字节摘要和完整 logicalMappings，且输入顺序不影响结论。

### 变更

- Quickstart Profile v2 与有序批量校验仍为 candidate；loader 改为返回规范 Schema ID。Registry、rules 与 error-codes 文档携带 1.10.0 坐标，Kernel 文档仍保持 1.8.0 生命周期坐标和字节基线。
- 0.10.0 以后新能力必须从首版起使用不携带成熟度的规范身份。

### 升级说明

现有 Quickstart v2 消费者应把三个包的精确 pin 更新到 0.10.0，并从历史 candidate 子路径和 Schema ID 迁移一次到规范身份；以后晋升 stable 不再改源码或合同身份，投影重建仍按既有绑定输入规则执行。Quickstart v1 消费者继续锁定 0.2.1。
<!-- release-skill:changelog:end version=0.10.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.9.0 locale=zh-CN baseline=sha256:e875e0408deb3335d0a6893c89fb1ad6707094b88aba6a5f3ab16364b1cef48e -->
## [0.9.0] - 2026-08-24

Contracts 1.9.0 新增稳定文件系统绑定与固定集合发布 Schema，并新增有序批量校验的 candidate Schema。

### 新增

- 将 filesystem-root-binding、fixed-set-publication-manifest 与 fixed-set-publication-receipt 登记为稳定顶层契约对象。
- 为现有 Quickstart Bundle 机制桥接增加 schema-validation-batch 请求与结果 candidate Schema。

### 变更

- 包版本进入 Foundation 0.9.0 锁步版本线。

### 升级说明

消费者可在核对对应 Harness API 后采用稳定文件系统 Schema；批量校验仍为 candidate，必须精确锁定 0.9.0 Bundle 面。
<!-- release-skill:changelog:end version=0.9.0 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.8.4 locale=zh-CN baseline=sha256:b53db664b96371ac7e0cd86b16258da3a2beb76d13d7ef015860376cb93e07f0 -->
## [0.8.4] - 2026-08-24

Contracts 1.8.0 新增业务中立的外置 source-authority receipt 及纯校验 API。

### 新增

- 登记闭合的 source-authority-receipt Schema，稳定顶层契约对象增至 32 类。
- 新增 validateSourceAuthorityReceipt 与 parseSourceAuthorityReceipt，用于校验收据规范形态并精确核对调用方实际观测的 subjects。

### 变更

- 包版本与 Harness、Engineering Kit 一同升至 0.8.4。

### 升级说明

需要 source authority 的消费者先用实际包 subjects 校验外置 receipt，再把返回的 sourceRepository 与 sourceBaseCommit 传入既有 source 字段；其他 Contracts 消费者无需迁移。
<!-- release-skill:changelog:end version=0.8.4 locale=zh-CN -->


<!-- release-skill:changelog:start version=0.8.3 locale=zh-CN baseline=sha256:bb7cfbdd852384a2344ccf8c0a28ee87cc13477cb2439ad878c76d921d3ddcb6 -->
## [0.8.3] - 2026-08-23

随 Foundation 0.8.3 锁步升版；Contracts 1.7.0 机器合同保持不变。

### 变更

- 包版本与 Harness、Engineering Kit 一同升至 0.8.3。
- Contracts 1.7.0、31 类顶层对象登记、Schema、错误码和公共导出均保持不变。

### 升级说明

消费者必须把三个 Foundation 包精确锁定到 0.8.3，再重建受管 Bundle；从 0.8.2 升级不需要迁移 Contracts API 或规格。
<!-- release-skill:changelog:end version=0.8.3 locale=zh-CN -->


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
