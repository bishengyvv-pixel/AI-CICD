# 07 · 里程碑规划

> v0.2 · 2026-09-24（Gitea / Go / GLM·DeepSeek / GH Actions 已决，见 08）
> 节奏：每个里程碑都可独立交付价值；M0 结束即可在真实仓库试用。

## M0 · PoC（约 1~2 周）

**目标：验证核心链路 —— 「MR → AI 审核 → MR 评论」端到端跑通。**

- [ ] Gitea webhook 接入 + 验签（已决：Gitea）
- [ ] 单一审查 prompt（先不分维度）：diff → findings → 总结评论
- [ ] LLM provider 接入：GLM / DeepSeek（OpenAI 兼容协议，已决）
- [ ] SQLite 持久化审核记录
- [ ] Docker Compose 一键部署

**验收**：真实仓库提一个含明显缺陷的 MR，5 分钟内收到 AI 的逐行评论。

## M1 · MVP（M0 后约 3~4 周）

**目标：成为可日常使用的「AI 审核门禁」。**

- [ ] 四维审查并行（正确性/安全/规范/测试充分性）+ findings schema 强校验
- [ ] 决策综合器：规则化 verdict（APPROVE / REQUEST_CHANGES / NEEDS_HUMAN）+ 置信度
- [ ] approve / request-changes 状态回写（严格门禁：配合 Gitea 分支保护真正阻止合并）
- [ ] 接入引导：自动检查仓库分支保护配置（required approvals / block on rejected reviews / dismiss stale）
- [ ] `.ai-cicd.yaml` 仓库级配置（维度开关、阈值）
- [ ] 新 push 取消旧任务 + commit SHA 竞态校验
- [ ] append-only 审计记录
- [ ] 大 diff 分片（map-reduce）
- [ ] 注入防护三件套：结构化校验 / 不可信定界 / 启发式扫描

**验收**：团队 1~2 个真实仓库接入日常使用，误杀率可接受。

## M2 · CI 集成（M1 后约 3 周）

**目标：AI 审核与现有 CI 形成闭环。**

- [ ] 执行层适配器：触发 CI / 查询状态 / 拉取失败日志（首家：GitHub Actions，兼容 Gitea Actions / act_runner）
- [ ] CI 失败诊断：根因分析 + 修复建议评论
- [ ] CI 结果纳入裁决（required 模式）
- [ ] 第二个 Git 平台 adapter（验证抽象层）
- [ ] 申诉流程（`@ai-cicd re-review`，次数上限）
- [ ] 分级用模（切片轻模型 + 裁决强模型）控制成本

## M3 · 受限执行 + 可运营（M2 后约 4 周）

**目标：把「受限执行者」做实，系统可运营。**

- [ ] 策略引擎完整实现（L1/L2/L3 分级 + 审批流）
- [ ] L2 审批动作：auto_merge（高置信白名单）/ AI 修复补丁提交（需审批）
- [ ] 人工审核队列（NEEDS_HUMAN 的呈现与流转）
- [ ] 管理 Dashboard：仓库接入、策略编辑、审核历史、指标（误杀率/兜底率/成本/耗时）
- [ ] 熔断与降级（LLM 不可用降级、误杀率异常降级为仅建议模式）
- [ ] 审计重放工具（同输入复跑对比）

## M3 之后的方向（不承诺）

- 自带轻量执行引擎（对无 CI 的团队开箱即用，向「AI 原生 CI/CD 平台」演进）
- 部署编排与发布策略（灰度/回滚建议）
- 多仓库组织级视图与规范知识库（RAG）
