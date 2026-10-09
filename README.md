> **[中文化] OmniMemEval**
>
> 此项目是 [MemTensor/OmniMemEval](https://github.com/MemTensor/OmniMemEval) 的镜像仓库。
> 上游已自带中文 README，本仓库在此基础上整理中文内容，代码与上游保持一致。
> - 原项目 Stars: 68
> - 主语言: Python
> - 许可证: Apache-2.0
> - 整理日期: 2026-10-09
> - 上游中文 README: README_zh.md（上游原文件，未改动）
>
> 著作权归上游作者所有。功能改动请提到上游仓库。

---
## 这是什么

OmniMemEval 是面向记忆系统和记忆增强 agent 的评测框架。它提供两条互补的评测轨道：一条测记忆后端 API，一条测装了记忆插件的 agent runtime。它让研究者能用统一的方式对比不同的记忆产品和自建框架。

## 核心能力

- **User Memory Evaluation**：评测通过 `add()` 和 `search()` 暴露的记忆后端 API，覆盖 LoCoMo、LongMemEval、BEAM、PersonaMem v2、HaluMem。
- **Agent Memory Evaluation**：评测装了 memory plugin 的 agent runtime 的任务表现，当前基于 AgentBench，在五个任务域上评测 OpenClaw：reasoning、information retrieval、knowledge work、code implementation、software engineering。
- **可切换后端**：通过 `--lib` 选择记忆后端，同一套 benchmark 流程可以跑主流记忆产品、自建记忆框架或自定义适配器。
- **完整流程**：User Memory Evaluation 覆盖数据写入、检索、答案生成、LLM-as-Judge 评测、指标聚合和报告生成。
- **memory-plugin 协议**：Agent Memory Evaluation 的协议包括 memory cleanup、training、memory settling、backup、restore 和 test execution。
- **结果落盘**：User Memory 结果写到 `results/<benchmark>/<LIB>-<VERSION>/`，Agent Memory 结果写到 `results/agentbench/`。

## 快速开始

两条评测轨道使用各自独立的 Python 环境。下面是 User Memory Evaluation 的安装与运行。

```bash
conda create -n omnimemeval python=3.12 -y
conda activate omnimemeval
pip install -r requirements_user_memory.txt
```

```bash
cp env_examples/.env.memos .env.memos
python data/locomo/prepare_locomo.py
./scripts/run_locomo_eval.sh --lib memos --env .env.memos
```

Agent Memory Evaluation 建议另建 `agentmem` 环境，因为它需要 OpenClaw 和额外的领域依赖：

```bash
conda create -n agentmem python=3.12 -y
conda activate agentmem
python -m pip install -U pip
pip install -r requirements_agentbench.txt
```

OpenClaw、系统包和各领域的准备步骤见上游的 `docs/agent_memory/README.md`。

## 目录结构

```text
configs/
  agentbench/                 # Agent Memory Evaluation configs
data/
  locomo/ longmemeval/ beam/  # User Memory benchmark data preparation
  personamem_v2/ halumem/
docs/
  README.md / README_zh.md    # Documentation index
  user_memory/                # User Memory guides and results
  agent_memory/               # Agent Memory guides, architecture, and results
env_examples/                 # Environment templates and parameter docs
scripts/
  agentbench/                 # AgentBench runner implementation
  client_factory/             # User Memory backend adapters
  locomo/ longmemeval/ beam/  # User Memory benchmark pipelines
  personamem_v2/ halumem/
  tests/                      # Test suite
  utils/                      # Shared utilities
requirements_user_memory.txt  # User Memory Evaluation dependencies
requirements_agentbench.txt   # Agent Memory Evaluation dependencies
```

## 与上游的差异

本仓库不做功能改动，只补充中文文档。如需提交功能改动，请提到上游仓库 [MemTensor/OmniMemEval](https://github.com/MemTensor/OmniMemEval)。

## 许可证

上游项目使用 Apache License 2.0。第三方 benchmark 数据沿用各自的上游许可证，OmniMemEval 的代码许可证不改变外部数据集的许可证。本仓库的代码部分沿用上游同一许可证。
