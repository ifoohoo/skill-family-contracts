# 变更日志

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
