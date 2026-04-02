# aipostex-templates

Community vulnerability templates for [aipostex](https://github.com/professor-moody/aipostex) — the open-source AI/ML infrastructure security assessment tool.

## Overview

This repository contains YAML-based vulnerability detection and exploit templates that aipostex uses to scan AI infrastructure. Templates cover 20 service families including LLM gateways, model registries, vector databases, inference servers, and tool-calling frameworks.

## Template Count

| Category | Detection | Exploit | Total |
|----------|-----------|---------|-------|
| ollama | 3 | 3 | 6 |
| mcp | 10 | 10 | 20 |
| jupyter | 3 | 3 | 6 |
| openai | 4 | 5 | 9 |
| ray | 3 | 2 | 5 |
| mlflow | 3 | 2 | 5 |
| vectordb | 4 | 3 | 7 |
| streamlit | 2 | 0 | 2 |
| *...and 12 more* | | | |
| **Total** | **60** | **44** | **104** |

## Directory Structure

Each subdirectory maps to a service family:

```
ollama/              # Ollama LLM server
mcp/                 # Model Context Protocol servers
jupyter/             # Jupyter notebooks/hubs
openai/              # OpenAI-compatible APIs
ray/                 # Ray clusters
mlflow/              # MLflow tracking servers
vectordb/            # ChromaDB, Weaviate, Qdrant, Milvus, pgvector
streamlit/           # Streamlit applications
gradio/              # Gradio interfaces
litellm/             # LiteLLM proxy
bentoml/             # BentoML serving
triton/              # NVIDIA Triton Inference Server
torchserve/          # PyTorch TorchServe
huggingface/         # HuggingFace TGI/TEI
...
```

## Template Format

Templates are YAML files with a standard structure:

```yaml
id: service-type-NNN-description
info:
  name: "Human-readable name"
  severity: critical|high|medium|low|info
  author: your-handle
  description: |
    What the template detects or exploits.
  tags: [service, category, ...]

detect:
  - method: GET
    path: /endpoint
    matchers:
      - type: body_contains
        value: "expected response"

checks:
  - name: "Check description"
    method: GET
    path: /api/endpoint
    matchers:
      - type: status
        value: "200"
    extractors:
      - type: json
        name: variable_name
        path: "json.path"
    severity: high
    finding:
      title: "Finding title with {{variable_name}}"
      description: "What was found and why it matters."
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide. The short version:

1. Fork this repo
2. Create a template in the appropriate service directory
3. Follow the ID convention: `service-type-NNN-description`
4. Test against a real or mocked instance of the service
5. Open a PR using the template

## License

Templates in this repository are available under the same license as the main aipostex project.

## Links

- [aipostex](https://github.com/professor-moody/aipostex) — Main tool repository
- [aipostex-lab](https://github.com/professor-moody/aipostex-lab) — Proxmox lab environment
- [Template authoring guide](https://github.com/professor-moody/aipostex/blob/main/docs/development/adding-templates.md)
