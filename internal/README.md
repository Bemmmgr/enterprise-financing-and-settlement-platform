# EFSP 内部研究与决策

这里保存项目内部论证、研究来源、模拟假设及关卡结论。对外介绍放在根 README 与未来的 docs/ 中。

## 关卡

| 关卡 | 目标 | 状态 |
|---|---|---|
| [G1 — Business Reality](gates/G1-business-reality.md) | 业务真实性：角色、合同、资金流和欧洲约束 | PASS，承接前次对话结论；2026-10-19 整合归档 |
| G2 — Product Boundary & Responsibilities | 产品责任、记录模型与交易生命周期 | 下一关，尚未创建文档 |
| G3 — Engineering Necessity | 从业务要求论证工程机制 | 后续 |
| G4 — Technical Validation | 通过实现与故障场景验证 | 后续 |
| G5 — Portfolio Readiness | 项目展示与可解释性 | 后续 |

## 来源与维护

- [原始 G1 研究稿](references/efsp_01_domain_research.original.md)：从用户下载目录取得，与前次对话上传附件逐字节一致；原文保留，不作为当前决策版本。
- [欧洲实务补充与审查记录](references/G1-europe-discussion.md)：保留前次用户补充及助手对 G1 的审查原文。
- [正式 G1](gates/G1-business-reality.md)：保留原研究来源，整合最新欧洲约束，撤销原平台经验要求，限定双边协议、付款承诺和平台资金责任等表述。第 9.3 节记录此次整合决策。

未找到此前生成的 starter 压缩包，故使用已上传研究稿作为基础。本次未重新开展外部资料核验。原文提及的独立决策日志与可行性初审文件尚未取得，不创建空白替代品。

G2 首先明确平台需要负责的业务记录、支付指令、财务运营记录和外部银行证据，再决定是否需要复式记账。API + 异步回执属于模拟假设；VoP 异常审核权限、额度口径及部分回款处理均需在 G2 明确。

G2–G5 与未来对外文档有实际内容后再创建；services/ 等未来目录按需建立。
