# 08 · 待定问题清单

> 持续维护：决策后移入对应文档并在此标记「已决」，新增问题追加到末尾。
> 2026-09-24（第二轮）：Q1 / Q2 / Q3 / Q4 / Q6 / Q7 已决，仅剩 Q5 命名待定。

### Q1 · 首选 Git 平台 —— ✅ 已决：Gitea

M0 打通 Gitea 全链路（webhook + review API + 逐行评论 + 分支保护门禁）。
GitLab 降为 P1，GitHub / Gitee 为 P2。详见 06。

### Q2 · 实现语言 —— ✅ 已决：Go

详见 06。

### Q3 · 门禁强制力 —— ✅ 已决：驳回必须真正阻止合并

不是「建议」而是**平台级阻断**。落地方式：Gitea 受保护分支开启
「Block merge on rejected reviews」+ AI 以 REQUEST_CHANGES 类型 review 落地；
「Dismiss stale approvals」与本系统「新 push 重审」机制闭环。
配置细节见 05「Gitea 门禁落地」；接入引导需自动检查目标仓库的分支保护配置。

### Q4 · LLM 与成本 —— ✅ 已决：外接在线 API（GLM / DeepSeek 等）

主力 provider 为 GLM（智谱）/ DeepSeek 等兼容 OpenAI 协议的在线服务；
Ollama 保留为可选 provider，不做硬性依赖。
已知取舍：代码内容会出域至所选 API 服务商（已确认接受）。

### Q5 · 项目命名 —— ❓ 待定

`AI-CICD` 是工作名。候选：MergeGate / Aegis CI / 门神(GateKeeper) ……
不急，M0 前定即可。

### Q6 · 团队规模与并发 —— ✅ 已决：1~5 人

SQLite + 进程内队列 + 单实例部署确认足够，v1 不考虑分布式。
原假设（≤50 仓库、日均 ≤100 MR）成立。

### Q7 · 现有 CI 系统 —— ✅ 已决：GitHub Actions

M2 首个执行层适配器按 GitHub Actions API 实现。
Gitea Actions（act_runner）与 GH Actions 协议/工作流同源，适配器设计同时覆盖
（仓库托管在 Gitea 时，CI 状态亦可直接经 Gitea API 查询）。
待确认细节（不阻塞设计）：CI 实际运行在 GitHub 还是 Gitea Actions。

---

## 已决记录

| 日期 | 决策 | 结论 |
|------|------|------|
| 2026-09-24 | 产品定位 | 轻量 CI/CD 形态 + AI 驱动；核心场景 = MR 触发 AI 审核决定通过/驳回 |
| 2026-09-24 | 集成方式 | 外挂现有 CI 系统，不替代 |
| 2026-09-24 | AI 自主权 | 受限执行者（低风险自动，高风险人工审批） |
| 2026-09-24 | Q1 Git 平台 | **Gitea**（M0）；GitLab P1，GitHub / Gitee P2 |
| 2026-09-24 | Q2 实现语言 | **Go** |
| 2026-09-24 | Q3 门禁强制力 | **驳回真正阻止合并**：Gitea 分支保护 + REQUEST_CHANGES review |
| 2026-09-24 | Q4 LLM | **在线 API：GLM / DeepSeek 等 OpenAI 兼容服务**；Ollama 可选 |
| 2026-09-24 | Q6 团队规模 | **1~5 人**；单实例 + SQLite 足够 |
| 2026-09-24 | Q7 现有 CI | **GitHub Actions**（兼容 Gitea Actions / act_runner） |
