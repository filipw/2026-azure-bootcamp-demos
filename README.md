# 2026 Azue Bootcamp Bern demos

## Collaboration Patterns

| Pattern Name | Description | Paper | Key Concept |
|--------------|-------------|-------|-------------|
| 💻 SLM-Default, LLM-Fallback | Route queries to a local SLM first, escalating to cloud only if the local model's output fails verification. | [arXiv:2510.03847](https://arxiv.org/abs/2510.03847) | Cost & Latency Optimization |
| 💻 Predictive Router | Use a local router to classify queries as "weak" or "strong". Route simple tasks to local models and complex ones to the cloud. | [arXiv:2501.01818](https://arxiv.org/abs/2501.01818) | Dynamic Routing |
| 💻 MAKER Protocol | Decompose complex tasks using a cloud-based "Planner" and execute atomic steps using a local "Voting Solver" with convergence checks. | [arXiv:2511.09030](https://arxiv.org/abs/2511.09030) | Task Decomposition |
| 💻 MINIONS Protocol | Decompose extraction tasks into parallel jobs for local "minions" to process on document chunks, synthesizing results in the cloud. | [arXiv:2502.15964](https://arxiv.org/abs/2502.15964) | Local-Remote Map-Reduce |
| 💻 Chain of Agents | Process long contexts by chaining local SLMs to sequentially build context before final synthesis in the cloud. | [arXiv:2406.02818](https://arxiv.org/abs/2406.02818) | Sequential Bucket Brigade |

---

## Python

> The SLM role is played by **Phi-4-mini-instruct** running locally.
> Two interchangeable local inference backends are supported, selected via the `LOCAL_BACKEND` environment variable:

| Backend | `LOCAL_BACKEND` value | Use case |
|---------|-----------------------|----------|
| **MLX** | `mlx` *(default)* | Apple Silicon (macOS) via [`agent-framework-mlx`](https://pypi.org/project/agent-framework-mlx/) |
| **Transformers** | `transformers` | Cross-platform (CUDA, MPS, CPU) via [HuggingFace Transformers](https://huggingface.co/docs/transformers/) |

Demos use short model names (e.g. `Phi-4-mini-instruct-4bit`) that are automatically resolved to the correct backend-specific model path. You can also pass a fully-qualified HuggingFace model ID or override with the `LOCAL_MODEL_PATH` env var.

### Prerequisites

- Python 3.11+
- Azure CLI logged in (`az login`)
- For the MLX backend: macOS with Apple Silicon
- For the Transformers backend: any platform with PyTorch support (CUDA, MPS, or CPU)

### Setup

```bash
cd python
cp .env.example .env # fill in your variables
pip install -r requirements.txt
```

### Running

```bash
# default (MLX backend)
python 01-slm-default-llm-fallback/demo.py

# use the Transformers backend instead
LOCAL_BACKEND=transformers python 01-slm-default-llm-fallback/demo.py
```

All five demos follow the same pattern:

```bash
python 01-slm-default-llm-fallback/demo.py
python 02-router-agent/demo.py
python 03-maker/demo.py
python 04-minions/demo.py
python 05-chain-of-agents/demo.py
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AZURE_AI_PROJECT_ENDPOINT` | Azure AI Foundry project endpoint | |
| `AZURE_AI_MODEL_DEPLOYMENT_NAME` | Deployment name for the LLM role in Azure AI Foundry | |
| `LOCAL_BACKEND` | Local inference backend (`mlx` or `transformers`) | `mlx` (default) |
| `LOCAL_MODEL_PATH` | Override the HuggingFace model ID or local path for the SLM | per-backend default |
