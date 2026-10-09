# AgentBench Evaluation Results

[Chinese](./results_zh.md)

[Agent Memory Evaluation guide](./README.md) ·
[Architecture](./architecture.md)

These tables record OpenClaw and Hermes evaluation setups. The current public
Agent Memory runner guide and registered runtime adapter cover OpenClaw.

## Metrics

> Acc: the average single-run pass rate across 3 independent runs of the same task, used to measure stable task completion in one run.

> Avg turns: the average number of model response turns triggered by the agent per task, used to measure interaction and reasoning depth.

> Avg chars: the average character length of the agent's response text, used to measure output length.

## Data And Evaluation Setup

Dataset source and split logic:

Reference: [https://huggingface.co/datasets/EverMind-AI/EvoAgentBench](https://huggingface.co/datasets/EverMind-AI/EvoAgentBench)

Evaluated agents: OpenClaw and Hermes with the corresponding product plugin or integration.

Baseline refers to the corresponding evaluated agent running without any plugin.

The OpenClaw version used for OpenClaw evaluation is 2026.5.7.

The Hermes version used for Hermes evaluation is Hermes Agent v0.18.0 (2026.7.1).

Products not marked as cloud services use locally deployed services and their corresponding agent plugins or integrations.

MemOS uses Memos-Local-Plugin version 2.0.8.

OpenClaw and Hermes answer model configuration: qwen3.6-flash in no_thinking mode.

Evaluation judge model: qwen3.6-flash in thinking mode.

## Results

### OpenClaw

| **Method** | BrowseComp-Plus Acc | BrowseComp-Plus Avg turns | OmniMath <br>Acc | OmniMath Cost <br>Avg chars | SWE-Bench <br>Acc | SWE-Bench <br>Avg turns | LiveCodeBench <br>Acc | LiveCodeBench Avg turns | GDPVal <br>Acc | GDPVal <br>Avg turns |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Baseline** | 18.46 | 35.1 | 52 | 5658.6 | 26.92 | 58.8 | 51.28 | 23.9 | 34.48 | 17.2 |
| **Mem0** | 13.33 | 36.4 | 53 | 6090.53 | 26.92 | 55.78 | 60.68 | 6.27 | 41.38 | 17.28 |
| **EverOS** | 18.98 | 30.8 | 54.67 | 6085.87 | 32.05 | 54.77 | 59.83 | 7.2 | 38.51 | 16.97 |
| **Supermemory** | 14.36 | 32.03 | 58 | 5632.1 | 30.7 | 51.37 | 59.83 | 6.83 | 43.10 | 16.70 |
| **Hindsight** | 16.92 | 35.6 | 55 | 6177 | 38.46 | 56.7 | 71.79 | 14.2 | 50.00 | 17.5 |
| **mem9**<br>**(Cloud service)** | 12.31 | 31.6 | 54.33 | 5880.9 | 30.77 | 59.1 | 64.1 | 8.23 | 47.7 | 15.8 |
| **Memori**<br>**(Cloud service)** | 16.92 | 36.3 | 52 | 6189.2 | 34.62 | 43.1 | 51.28 | 9.3 | 37.93 | 15.8 |
| **MemOS** | **23.85*** | 43.3 | **61*** | 5164.2 | **38.46*** | 49.55 | <u>64.96</u> | 9.43 | **62.07*** | 23.93 |

### Hermes

| **Method** | BrowseComp-Plus Acc | BrowseComp-Plus Avg turns | OmniMath <br>Acc | OmniMath Cost <br>Avg chars | SWE-Bench <br>Acc | SWE-Bench <br>Avg turns | LiveCodeBench <br>Acc | LiveCodeBench Avg turns | GDPVal <br>Acc | GDPVal <br>Avg turns |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Baseline** | 18.97 | 43.23 | 62 | 10872.8 | 37.18 | 84.93 | 52.99 | 22.27 | 54.6 | 62.57 |
| **Mem0** | 17.95 | 42.73 | 64.33 | 11740 | 34.62 | 94.37 | 57.26 | 25.63 | 56.9 | 57.73 |
| **Supermemory** | 18.46 | 41.03 | 64.00 | 10675.90 | 35.90 | 92.23 | 66.67 | 19.73 | 57.47 | 58.60 |
| **Hindsight** | 21.54 | 48.33 | 64.00 | 12089.63 | 28.20 | 84.47 | 61.54 | 30.07 | 57.47 | 59.67 |
| **Viking** | 21.54 | 39.33 | 58.67 | 12514.20 | 35.90 | 88.70 | 60.68 | 27.77 | 58.05 | 54.27 |
| **mem9**<br>**(Cloud service)** | 18.46 | 40.13 | 66 | 12419.4 | 39.74 | 98.4 | 58.97 | 18.77 | 55.75 | 53.17 |
| **Memori**<br>**(Cloud service)** | 14.87 | 40.13 | 64.33 | 14811 | 28.21 | 91.5 | 54.7 | 22.37 | 55.75 | 53.17 |
| **MemOS** | 20.75 | 36.20 | **72.67*** | 5915.7 | **52.56*** | 76.37 | <u>64.1</u> | 25.63 | 55.17 | 58.6 |
