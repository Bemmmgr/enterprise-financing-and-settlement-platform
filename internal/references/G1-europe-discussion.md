# G1 欧洲实务补充与审查记录

来源：[供应链金融全栈项目规划](chatgpt-conversation://6ad57c3a-44e8-83ed-b07a-9be89a2e2cc0)，2026-10-19。

以下保留用户补充及此前助手对 G1 的审查原文。本次为历史记录归档，并非重新核验；原文中的 ChatGPT 引用索引无法作为独立来源链接使用。可访问的来源 URL 见用户补充与正式 G1 文档。

## 用户补充原文

一些对G1的补充：

好，原平台的经验不再作为设计依据，EFSP 改为以欧洲实际业务为准。原来那个分流问题和五个回忆问题已经撤掉，G1 也就没有待补的项了。

我按欧洲的规则补了一节"欧洲实务约束"，写在 G1 文档第 9 节。基本假设是：荷兰法为准据法、运营地在荷兰、资金走欧元和 SEPA。决策日志也同步了。

**对设计影响最大的两条：**

- **重复付款几乎无法事后补救。** 按 SEPA 普通转账规则，重复付款只能申请撤回，必须在 10 个营业日内发起，还要等收款方银行最多 15 个营业日答复，并且不保证能追回。所以防止重复付款的幂等机制，是由欧洲支付规则本身决定的，不是为了展示技术硬加的。
- **多了一个真实的失败场景：收款人核验。** 欧盟即时支付条例要求，从 2025 年 10 月 9 日起，银行在付款人授权之前必须提供收款人核验，而且适用于所有转账，不只是即时转账。供应商银行账户被篡改或填错时，核验会提示不匹配。平台需要决定这时是暂停放款，还是通知某一方处理。

**其他几条也会进入 G2：**

- **SEPA 有在途期。** 普通转账最长一个银行营业日到账，所以"已发出、未到账"是真实存在的状态。到账之后款项仍可能被退回，状态机需要有"被拒绝"和"被退回"。
- **营业日历按 TARGET 假日计算。** 放款日和到期日都按银行营业日算，2026 年的 TARGET 假日共 6 天。原来"只处理周末"的简化已经改掉。
- **应收转让要通知买方。** 荷兰 2025 年的新法规定，公开转让应收账款必须书面通知债务人，所以融资生效时要给买方发转让通知。
- **账期上限有法律约束。** 荷兰大企业付给中小企业最长 30 天，一般 B2B 交易最长 60 天。所以演示里的供应商设为非中小企业。
- **买方需要项目级报表。** 按 IAS 7 / IFRS 7，买方必须披露供应商融资安排，所以平台要能给买方出项目级报表。

**还有一个问题公开资料确定不了：** 平台和资金方之间的接口具体是什么形式，没有公开标准。这一项在 G2 里作为假设处理：用 API 加异步回执，语义参照 ISO 20022。

现在可以开始 G2 了，我会先从账本模型入手，要开始吗？

Sources:

- [EPC – SEPA Credit Transfer Rulebook 2025](https://www.europeanpaymentscouncil.eu/sites/default/files/kb/file/2024-11/EPC125-05%202025%20SCT%20Rulebook%20version%201.0.pdf)
- [Osborne Clarke – EU Instant Payments Regulation](https://www.osborneclarke.com/insights/what-are-key-obligations-and-timelines-eu-instant-payments-regulation)
- [Bundesbank – TARGET holidays 2026](https://www.bundesbank.de/resource/blob/764518/56d96128916d44d1e8d3bc245c1e14dc/mL/feiertage-in-deutschland-1-data.pdf)
- [Poelmann van den Broek – Wet opheffing verpandingsverboden](https://www.poelmannvandenbroek.nl/actuele-zaken/ondernemers-opgelet-per-1-juli-treedt-de-wet-opheffing-verpandingsverboden-in-werking)
- [Business.gov.nl – Payment terms](https://business.gov.nl/regulations/payment-term-collection-charges-statutory-interest/)
- [EY – IAS 7 / IFRS 7 supplier finance](https://www.ey.com/en_gl/technical/ifrs-technical-resources/iasb-amendments-to-ias-7-and-ifrs-7-for-supplier-finance-arrangements)

## 此前助手 G1 审查原文

# EFSP — G1 欧洲实务补充审查与 G2 启动建议

**我认可这次补充。EFSP 现在可以结束 G1，正式进入 G2（Product Boundary & Responsibilities）。**

这次新增的欧洲实务约束很有价值，因为它们把之前比较抽象的技术问题，落实到了真实存在的欧洲金融业务中。

尤其是 SEPA 转账、收款人核验（Verification of Payee）、应收账款转让通知和银行营业日历，使 EFSP 的资金处理流程更接近真实企业系统。

不过，我核对了相关官方规则，发现有几处需要进一步限定，否则 G2 可能会引入不准确的业务假设。

## 一、欧洲实务约束的核验结果

| 新增约束 | 核验结果 | 对 EFSP 的实际影响 |
|---|---|---|
| SEPA 重复付款追回 | 确认，但需要限定 | 必须优先防止重复付款指令 |
| Verification of Payee | 确认 | 增加收款人核验及异常处理 |
| SCT 在途与退回 | 确认存在不同支付状态 | 分离指令、执行、结算与退回 |
| TARGET 营业日历 | 确认 | 引入银行营业日计算 |
| 应收账款转让书面通知 | 确认适用于相关债权 | 记录通知及送达证据 |
| 荷兰 B2B 付款期限 | 确认，但存在例外 | 业务案例需要符合适用规则 |
| IFRS 供应商融资披露 | 确认 | 为适用企业提供报表数据 |
| API + 异步回执 | 合理假设 | 作为模拟资金方接口 |

### 1. SEPA Recall：幂等的重要性有了现实依据

EPC 的 SEPA Credit Transfer Rulebook 确实规定：

对于重复发送或技术错误导致的转账，付款人的支付服务提供商（PSP）需要在原交易执行后的 **10 个银行营业日内发起 Recall**。

收款方 PSP 则需要在收到 Recall 后的 **15 个银行营业日内作出回应**。

但需要明确：

- 10 天是特定 Recall 原因下的发起期限，不是所有转账追回的统一期限。
- 欺诈原因另有期限。
- Recall 不保证成功追回资金。
- 规则直接约束的是参与支付体系的 PSP，而不是 EFSP 这类第三方融资编排平台。

此外，EPC 在 2026 年发布了 SCT Rulebook v1.2，自 2026 年 9 月 30 日起生效；相关更新主要涉及地址信息，不改变这里讨论的 Recall 业务规则。:chatgpt-content-reference{index="0"}

**工程结论：**

EFSP 不应依赖事后 Recall 纠正重复付款。

但是必须区分两个层面的幂等：

1. EFSP 内部避免为同一融资生成重复的放款操作。
2. 外部资金方能够根据稳定的业务标识识别重复请求。

只有第一层幂等，不能自动保证外部银行不会重复付款。

这会是 G2/G3 非常重要的系统边界。

### 2. Verification of Payee：需要考虑，但不能简单理解为银行必须拒绝

欧元区 PSP 自 2025 年 10 月 9 日起适用相关收款人核验要求。

该服务同时覆盖普通 SEPA 转账与即时 SEPA 转账。

核验结果可能包括：

- Match
- Close Match
- No Match
- Other / Unable to Verify

不过，欧洲规则并不要求所有名称不匹配的付款一律被阻止。付款人仍可能在收到警告后决定授权交易。

此外，非消费者在批量付款场景下存在选择退出逐笔核验服务的安排。:chatgpt-content-reference{index="1"}

**对 EFSP 的建议：**

V1 可以采用比法规更严格的内部风险控制：

> 收款人核验不匹配时，暂停放款请求，进入人工审核队列。

这应当标记为 **EFSP Product Policy**，而不是描述成欧洲法规强制要求所有平台停止付款。

这个场景非常适合展示异常处理工作流。

### 3. TARGET 营业日历：六个假日属实，但要区分支付方式

ECB 公布的 2026 年 TARGET 关闭日确实包括六个节假日：

1 月 1 日、4 月 3 日、4 月 6 日、5 月 1 日、12 月 25 日和 12 月 26 日。

但 TARGET 并不意味着所有 SEPA 支付都只能在这些营业日处理。

例如，SEPA Instant 采用全天候处理机制，而普通 SCT 具有不同的处理周期。:chatgpt-content-reference{index="2"}

因此建议 V1 明确采用：

**EUR + SEPA Credit Transfer（普通 SCT）**

不立即加入 SEPA Instant。

这样更容易合理地实现银行营业日、付款处理周期和延迟状态。

### 4. 荷兰应收转让规则：值得纳入业务模型

荷兰《Wet opheffing verpandingsverboden》自 2025 年 7 月 1 日生效。

法律在取消符合条件的商业应收账款转让和质押限制的同时，针对其适用范围内的债权转让，引入书面通知要求。

这可以从荷兰官方公布的法律文本中确认。:chatgpt-content-reference{index="3"}

对 EFSP 的影响不仅是多发送一封通知。

更重要的是：

**债权转让状态、债务人通知状态和付款对象的更新，应当是可追踪的不同业务事实。**

例如：

```text
Receivable Purchase Agreed
          ↓
Assignment Recorded
          ↓
Buyer Notification Sent
          ↓
Buyer Notification Acknowledged
          ↓
Payment Instructions Updated
```

这里 `Acknowledged` 可以是 EFSP 选择采用的运营证据，而不应直接等同于所有情况下法律要求的生效条件。

对于跨境交易，即使合同选择荷兰法，也不能直接假定债权转让的所有法律问题都只受荷兰法支配。V1 应将适用法和债权范围写成明确的模拟假设。

### 5. 账期与 IFRS 披露：正确，但不是所有企业都一样

荷兰政府确认，大型企业向符合条件的中小企业付款，一般适用最长 30 天的法定账期。普通 B2B 合同通常以 60 天为上限，但符合条件时可以有例外安排。:chatgpt-content-reference{index="4"}

因此，将 60 天示例中的供应商设置为非 SME 是合理的简化。

但如果未来扩展为跨境融资，需要重新检查适用合同法律与付款期限，而不是仅按供应商国籍判断。

IFRS 方面，IAS 7 和 IFRS 7 的修订确实要求适用相关会计准则的企业披露供应商融资安排，涉及条款、负债金额及流动性风险等信息。:chatgpt-content-reference{index="5"}

EFSP 可以提供支持披露的数据和项目级报表，但不需要承担买方财务报表编制及最终会计合规责任。

---

# 二、G1 正式结论

我建议将 G1 状态改为：

**G1 — PASS**

通过依据：

- 反向保理业务模式有充分的行业依据。
- 欧洲存在真实运行的类似商业产品。
- 参与方、合同安排与资金流可以解释清楚。
- 支付异常、应收转让、银行营业日等具有现实依据。
- V1 的关键简化假设已经能够明确标注。
- 不再需要用你的原工作经历证明欧洲融资业务规则。

同时保留一个原则：

**G1 通过，不代表所有法律、会计和运营细节都已经确定。**

未解决的业务责任问题进入 G2；需要技术验证的问题进入 G3/G4。

这比让 G1 无限延续更加合理。

---



