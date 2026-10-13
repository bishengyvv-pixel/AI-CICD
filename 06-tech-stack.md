# 06 · 技术选型与权衡

> v0.2 · 2026-09-24（Q1/Q2/Q4/Q6/Q7 已决，见 08）
> 原则：轻量优先、单机可部署、依赖数量最少化。

## 语言 ✅ 已决：Go

单二进制部署、并发模型适合事件驱动、轻量 CI 血统（Drone / Woodpecker / Gitea 全是 Go）、内存占用小。
（曾对比 Python：AI 生态更丰富但部署重；本项目对 LLM 的使用是「受约束的 API 调用编排 + 结构化输出解析」，
不依赖训练/微调生态，Go 足够。）

## 存储（团队 1~5 人 ✅，单实例已确认足够）

| 组件 | v1 选择 | 升级路径（1~5 人规模下暂不需要） |
|------|---------|--------------------------------|
| 业务库 | SQLite（零依赖，单文件） | PostgreSQL |
| 队列 | 进程内队列 + 持久化到业务库（崩溃恢复） | Redis / NATS |
| 审计 | 业务库 append-only 表 | 对象存储归档 |

## LLM 接入 ✅ 已决：在线 API 为主

| Provider | 状态 | 说明 |
|----------|------|------|
| GLM（智谱）/ DeepSeek | ★ 主力 | OpenAI 兼容协议，配置 base_url + api_key 即接入 |
| 其他 OpenAI 兼容服务 | 可选 | 同一协议层天然支持（Qwen、Kimi 等） |
| Anthropic | 可选 | 独立 provider 实现 |
| Ollama（本地） | 可选 | 保留为 provider 选项，不做硬性依赖 |

- 结构化输出统一走 JSON schema 约束（function-calling / json mode）。
- 不引入 LangChain 类重框架：编排逻辑简单明确，自研 200 行内可控（见 04 五阶段流水线）。
- 已知取舍：代码内容会发送至所选在线 API 服务商（已确认接受）。
- 成本控制：分级用模（M2）—— 切片/摘要用便宜模型，裁决用强模型。

## Git 平台接入 ✅ 已决：Gitea P0

| 平台 | 优先级 | 说明 |
|------|--------|------|
| Gitea | P0（M0 实现） | webhook（pull_request 事件）+ review API（APPROVED / REQUEST_CHANGES / COMMENT）+ 逐行评论 API；**分支保护实现真正门禁**（见 05） |
| GitLab | P1 | 自建场景常见，MR API 完备 |
| GitHub / Gitee | P2 | adapter 成本低，按需 |

Gitea 版本要求：≥ 1.18（受保护分支的「拒绝审核阻止合并」等选项），具体最低版本 M0 验证后固化。

## 现有 CI 对接 ✅ 已决：GitHub Actions 优先

| 系统 | 优先级 | 方式 |
|------|--------|------|
| GitHub Actions | P0（M2 实现） | API 查 workflow run 状态 / 拉取失败 job 日志；或 check_run webhook |
| Gitea Actions（act_runner） | P0（与上同源） | 工作流协议与 GH Actions 同源；仓库托管在 Gitea 时，CI 状态可直接经 Gitea API 查询 |
| Jenkins / GitLab CI | P2 | 按需扩展 |

统一抽象：`trigger(build) / status(buildId) / logs(buildId, failedOnly)`。

待确认细节（不阻塞设计）：仓库托管在 Gitea 时，CI 实际跑在 Gitea Actions 还是 GitHub —— 两种情形适配器均覆盖。

## 部署

- 单容器（docker-compose）：服务 + SQLite。
- 配置：全局配置环境变量 + 仓库级 `.ai-cicd.yaml`。
- 可观测：结构化日志 + Prometheus 指标（审核耗时 / 成本 / 误杀率 / 人工兜底率）。

## 明确不引入（保持轻量）

- Kubernetes Operator（单机场景收益为负）
- 微服务拆分（单二进制模块化即可）
- 消息中间件默认依赖（内嵌队列起步）
- 重型 AI 编排框架（自研薄流水线）
