# NetArena Reproduction Results

**Date**: 2026-04-16
**Server**: hk-Super-Server (Ubuntu 22.04, 40 cores, 62GB RAM, NVIDIA GPU)
**Benchmark**: MALT (Data Center Capacity Planning)
**Agent**: Qwen3.5-Flash via DashScope API

## Environment Setup

- Python 3.12 (conda)
- NetArena installed from source (`pip install -e .`)
- Agent Server: LiteLLM A2A server on port 8000
- LLM API: DashScope (OpenAI-compatible)

### Agent Server Command

```bash
python a2a_llm/litellm_a2a_server.py \
  --model-name "openai/qwen3.5-flash" \
  --api-key "$DASHSCOPE_API_KEY" \
  --api-base-url "https://dashscope.aliyuncs.com/compatible-mode/v1" \
  --host 127.0.0.1 --port 8000
```

### Benchmark Command

```bash
cd app-malt
python run.py --config config.toml
```

## Results

### Benchmark Configuration

- Agent: AzureGPT4Agent (template name, actual backend: Qwen3.5-Flash)
- Prompt Type: base
- Complexity: level1, level2
- Queries/Type: 10

### Verification

The Agent Server successfully received benchmark queries and Qwen3.5-Flash generated valid Python code that operates on the data center topology graph using networkx.

## Screenshots

- `malt_benchmark_start.png`: Benchmark evaluation started successfully
- `malt_agent_response.png`: Agent Server receiving queries and returning Python code

## Setup Notes

### Dependencies (lightweight install)

```bash
conda create -n netarena python=3.12 -y
conda activate netarena
cd NetArena
pip install -e .
pip install litellm loguru "a2a-sdk[http-server]" uvicorn cattrs tomli httpx jsonlines prototxt_parser scipy
```

### Docker Troubleshooting (HK Server)

Docker requires `iptables: false` in `/etc/docker/daemon.json` on this server.

### API Access

DashScope (Qwen) works directly. OpenRouter (Claude/GPT) requires SSH tunnel proxy from local machine due to region restrictions.
