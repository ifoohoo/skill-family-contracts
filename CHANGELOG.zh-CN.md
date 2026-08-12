# 变更日志

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
