---
description: This file describes the code style guidelines for the project.
paths: 
  - "**/*.py"
  - "**/*.rs"
---

# Code Style Guidelines

## Idioms
- "Keep It Simple, Stupid"
- Use existing code as much as possible
- Readability is the first priority when writing code
- Code style should be consistent across the project
- All code should be well-documented with docstrings in English
- All variables should be meaningful, concise, and short
- Keep argument names and variable names consistent across the entire project (e.g., if Python uses `--step`, SLURM scripts must also use `--step`, not `--steps`)

## Evaluation
- Always provide a `--num_samples` argument to allow small-scale dry runs (e.g., ~50) before full evaluation; omitting it runs all instances
- For custom (non-official) eval implementations, separate each step (e.g., data loading, inference, scoring) into independent stages and verify each before proceeding
- After completing individual modules, assemble them in a single entry-point file and use a `--step` argument to invoke each module independently (e.g., `python main.py --step preprocess`)
- Reproducibility is the first priority for evaluation

### Directory Structure
```
eval/
├── {benchmark}/
│   ├── results/
│   │   └── {model_name}/
│   │       └── *.json / *.csv
│   └── ...
├── tables-and-figures/
│   └── rq{n}-{type}.png / .csv
```

### Reproducibility Defaults
- Default random seed: `1104` (set in config YAML)
- Default temperature: `0`
- Results stored as JSON or CSV

## Configs
- All experiment configurations are managed in `configs/` using YAML format
- One YAML file per benchmark, named `{benchmark}.yaml`
- Use `configs/` as the single source of truth for experiment parameters
- Template:

```yaml
# configs/{benchmark}.yaml
dataset: org/dataset_name  # HuggingFace dataset path

models:
  api:  # litellm model names
    - gpt-4o
    - claude-sonnet-4-20250514
  local:  # HuggingFace model IDs (vllm)
    - meta-llama/Llama-3-70B-Instruct

inference:
  temperature: 0
  seed: 1104
  max_tokens: 4096
  # num_samples: 50  # uncomment for pilot run
```

## Data
- Datasets are stored in `data/` (both public benchmarks and custom data)
- Use HuggingFace `datasets` for version management of public benchmarks
- Large data files should be added to `.gitignore`

## Logging
- Use Python `logging` module, not `print`, for all non-tqdm output
- Log level: `INFO` for progress, `WARNING` for recoverable issues, `ERROR` for failures
- Log format: `%(asctime)s - %(name)s - %(levelname)s - %(message)s`
- SLURM stdout (`*.out`) captures all logs; no need for separate log files

## LLM API
- API keys are stored in `~/.zshrc` as environment variables, never in code or config files
- Use `litellm` for all LLM API calls
- Model and inference parameters are read from `configs/` YAML files

## Scripts
- `scripts/` is for GPU-intensive workloads that require dedicated SLURM job submissions
- One script per LLM, each submitted as an independent SLURM job so multiple models can run in parallel across GPUs
- Scripts read from `configs/` YAML; comment out models in config to skip them when running locally
- Script naming: `{benchmark}_{task}.sh`
- Log naming: `{benchmark}_{step}_{model_name}.log`

## Pilot Experiments
- For every project, make sure to generate a small sample of data and run a pilot experiment script to verify the code and environment
- Use `--num_samples` for small-scale dry runs before full evaluation
