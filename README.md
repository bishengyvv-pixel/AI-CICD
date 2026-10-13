# AI-CICD 架构构思工作区

> 状态：v0.1 草案（2026-09-24）
> 本目录用于项目架构的构思与迭代，文档随讨论持续更新。

## 项目一句话定位

一个 **AI 驱动的轻量级 CI/CD 系统**：以「合并请求（MR/PR）的 AI 审核门禁」为核心场景，
形态上参考主流轻量 CI/CD 项目，通过外挂方式接入现有 CI 系统，AI 以**受限执行者**身份工作。

## 核心理解（来自需求方原话的提炼）

1. **轻量化**：形态对标 Drone / Woodpecker / Gitea Actions 这类轻量 CI，而不是 Jenkins 级重型平台。
2. **AI 驱动**：AI 是系统的决策核心，不是锦上添花的辅助功能。
3. **核心场景**：开发者提交代码 → 发起合并至主开发分支的合并请求 → 触发 AI 审核代码 →
   AI 决定**通过 / 驳回**（附理由与逐行评论）。
4. **外挂集成**：不替代现有 CI 系统，与之协作（复用其 runner 与流水线）。
5. **受限执行者**：低风险操作自动执行（评论、审核、驳回），高风险操作（合并、部署）需人工审批。

## 文档导航

| 文档 | 内容 |
|------|------|
| [01-vision.md](01-vision.md) | 愿景、对标产品、差异化、非目标 |
| [02-core-scenario.md](02-core-scenario.md) | 核心场景逐帧走查（主场景 + 次要场景） |
| [03-architecture.md](03-architecture.md) | 总体架构、组件职责、关键设计原则 |
| [04-ai-review-engine.md](04-ai-review-engine.md) | AI 审核引擎详细设计（流水线、决策模型、防注入） |
| [05-autonomy-safety.md](05-autonomy-safety.md) | 自主权分级、策略引擎、审计与安全 |
| [06-tech-stack.md](06-tech-stack.md) | 技术选型与权衡 |
| [07-roadmap.md](07-roadmap.md) | 里程碑规划（M0 PoC → M3） |
| [08-open-questions.md](08-open-questions.md) | 待定问题清单（持续维护） |
| [09-gitea-integration.md](09-gitea-integration.md) | Gitea 集成深化：MR 代码读取路径 + 网页呈现与门禁落地设计 |

## 使用方式

- 讨论中产生的决策更新到对应文档，并在文档头部标注版本与日期。
- 未决问题一律进 [08-open-questions.md](08-open-questions.md)，不要散落在对话里。
- 文档中的图表均为 **Mermaid**（时序图 / 流程图 / 状态机），请在支持渲染的查看器中阅读
  （GitLab、GitHub、Typora、VS Code 等）。
