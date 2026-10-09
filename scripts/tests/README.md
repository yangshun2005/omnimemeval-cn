# OmniMemEval Tests

The public test suite covers both evaluation tracks, shared utilities, memory
adapter configuration, pipeline contracts, and integration smoke helpers.

## User Memory and Shared Tests

```bash
conda run -n omnimemeval python -m unittest discover -s scripts/tests -p 'test_*.py'
conda run -n omnimemeval python -m unittest discover -s scripts/tests/unit -p 'test_*.py'
conda run -n omnimemeval python -m unittest discover -s scripts/tests/pipeline -p 'test_*.py'
```

## Agent Memory Unit Tests

Agent Memory uses the separate `agentmem` environment. Focused AgentBench unit
tests can be selected without running real tasks:

```bash
conda run -n agentmem python -m unittest discover -s scripts/tests/unit -p 'test_agentbench_*.py'
```

## Integration Smoke Tests

Integration smoke tests require real backend credentials:

```bash
conda run -n omnimemeval python scripts/tests/integration/smoke_clients.py --lib memos --env .env.memos
```
