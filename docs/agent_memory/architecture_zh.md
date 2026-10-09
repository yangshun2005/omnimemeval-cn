# Agent Memory Evaluation 架构

[English](./architecture.md)

[文档索引](../README_zh.md)

本文档说明当前 Agent Memory Evaluation 的架构和公开扩展契约。环境准备和评测命令
仍以 [Agent Memory Evaluation 指南](./README_zh.md)为准。

## 架构概览

Agent Memory Evaluation 评估以下组合：

```text
Agent Runtime + Memory Plugin + Task Domain + Verifier
```

AgentBench 入口负责协议和记忆生命周期的执行顺序，Runner 负责任务、Trial、Retry
和 Feedback 的执行。Runtime 相关行为由 `AgentAdapter` 实现；任务加载、Prompt
构造、环境准备和验证由 `DomainAdapter` 实现。记忆插件生命周期操作通常配置在
`configs/agentbench/memory_plugins/` 下。

## 核心组件

### `AgentAdapter`

[`AgentAdapter`](../../scripts/agentbench/agents/base.py) 定义 Runner 如何驱动一个
Agent Runtime。它负责构造 Session 元数据、调用 Runtime、收集 Session 产物和
Token 用量，以及判断可重试的 Runtime 结果。

当前公开 Agent Registry 注册了
[`OpenClawAgentAdapter`](../../scripts/agentbench/agents/openclaw.py)。

### `DomainAdapter`

[`DomainAdapter`](../../scripts/agentbench/domains/base.py) 定义任务域如何加载任务、
准备单任务资源、构造 Prompt、验证 Agent 输出并清理任务状态。

当前 Domain Registry 覆盖 reasoning、information retrieval、knowledge work、
code implementation 和 software engineering。

### Runner 与记忆生命周期

[`run_agent_eval.py`](../../scripts/agentbench/run_agent_eval.py) 选择评测协议，并负责
Phase 和记忆生命周期的执行顺序。

[`runner.py`](../../scripts/agentbench/runner.py) 执行任务和 Trial、写入单次 Trial
产物、应用重试策略并生成 Phase 汇总。

[`memory_lifecycle.py`](../../scripts/agentbench/memory_lifecycle.py) 执行配置中的插件
生命周期命令。`memory_train_backup_test` 协议负责 validation、模式切换、清理、
训练、等待沉淀、备份、恢复和测试的执行顺序。可选的 Structured Feedback 集成
仍在 Runner 和插件配置中显式声明。

## 评测执行流程

每个任务 Trial 按以下顺序执行：

```text
domain.setup
agent.prepare_task
domain.build_prompt
agent.call
domain.verify
optional train feedback
domain.record_agent_outcome
domain.cleanup
agent.collect_session
agent.cleanup_task
write result.json
```

Domain 级 initialize 和 finalize 包围完整 Phase。全部选定任务和 Trial 完成后生成
Phase 汇总。

## Session 标识契约

[`SessionSpec`](../../scripts/agentbench/session.py) 将不同用途的标识分开：

- `cli_session_id`：经过清理、可被 Agent CLI 接受的标识。
- `semantic_session_id`：保留 Benchmark Phase、Domain、Split、Task 和 Trial
  语义的标识。
- `source_ref`：稳定的 Benchmark 来源引用。
- `metadata`：Benchmark、Phase、Domain、Split、Task、Trial 和 Agent 的结构化
  元数据。
- `agent_session_ref`：可选的 Runtime 专用 Session 引用。
- `openclaw_session_key`：使用 OpenClaw 时对应的 Session Key。
- `openclaw_gateway_session_id`：对应的 OpenClaw Gateway 标识。

OpenClaw Adapter 将 `cli_session_id` 传给 `openclaw agent --session-id`，并默认通过
`OMNIMEMEVAL_AGENT_CONTEXT` 暴露序列化后的完整 `SessionSpec`。这样可以让
CLI 安全标识与插件和结果元数据保持对应。

## 结果与指标契约

每个 Trial 写入一个 `result.json`，其中包含 Task、Phase、Split、Trial、Session、
Agent 结果、Verifier 结果、Feedback 结果、Token 用量、时间戳和异常信息。辅助产物
可以包括 `response.txt`、`session.jsonl` 和 Verifier 输出。

每个 Phase 写入 `summary.json` 和 `report.md`。汇总包括任务数和 Trial 数、
`pass@1`、配置的 `pass@n`、平均通过率、Reward、耗时、Turn、Token、Failure
Class、排除基础设施错误后的统计，以及 Domain 专用指标。

## 扩展点

- 新增 Agent Runtime：实现 `AgentAdapter`，并注册到
  `scripts/agentbench/agents/__init__.py`。
- 新增任务域：实现 `DomainAdapter`，并注册到
  `scripts/agentbench/domains/__init__.py`。
- 新增或调整记忆插件生命周期：通过
  `configs/agentbench/memory_plugins/` 配置；当通用命令生命周期不足时，使用显式
  集成代码。

Agent Memory Evaluation 与 User Memory 的 `scripts/client_factory/` Adapter 层保持
分离，因为两条评测线评估的是不同系统边界。
