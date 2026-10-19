# EFSP — Enterprise Financing & Settlement Platform

面向企业应收账款融资的业务编排与资金运营项目，以欧洲 Payables Finance（买方主导的应付账款融资）场景为研究基础。

当前处于设计阶段：G1 业务真实性研究已整理，下一关为 G2 产品边界与责任。尚未生成应用代码。EFSP 负责融资编排与运营记录，实际资金转移由外部金融机构执行。

## 目录

| 路径 | 用途 | 当前状态 |
|---|---|---|
| `backend/` | Java / Spring Boot 后端 | 已建目录，待实现 |
| `frontend/` | React / TypeScript 前端 | 已建目录，待实现 |
| `infrastructure/` | Docker、部署与环境配置 | 已建目录，待实现 |
| `internal/` | 内部研究、证据、假设与关卡决策 | 已有 G1 与内部索引 |
| `docs/` | 面向项目读者的技术说明 | 已建目录，尚无对外文档 |
| `services/` | 未来具有独立职责的服务 | 按需创建，当前未创建 |

内部设计入口：[研究与决策索引](internal/README.md)。

未来对外文档按 `docs/01-business.md`、`docs/02-architecture.md`、`docs/03-engineering.md` 分类；有实质内容时再创建。内部研究不直接等同于对外承诺，已规划技术不代表已经实现。
