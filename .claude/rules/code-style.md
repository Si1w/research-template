---
description: This file describes the code style guidelines for the project.
paths: 
  - "**/*.py"
  - "**/*.sh"
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
  vllm:  # HuggingFace model IDs (vllm)
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
- `scripts/` is for workloads that require dedicated SLURM job submissions
- Split by model type: `{benchmark}_local.sh` (GPU, vllm) and `{benchmark}_api.sh` (CPU, litellm), matching `models.vllm` / `models.api` in config YAML
- Each script defines a `MODELS=()` array and loops through them, submitting or running each model sequentially
- Scripts read experiment parameters from `configs/` YAML via Python argparse; model list is defined in the script itself

### Bash Script Template

Usage: `sbatch scripts/<benchmark>_<task>.sh --model <name> --step <step>`

```bash
#!/bin/bash
set -euo pipefail

# --- Job ---
#SBATCH -J <job_name>                # job name
#SBATCH -p <gpu|cpu>                 # partition
#SBATCH -t 30:00:00                  # time limit (max 48h)
#SBATCH -o %x_%j.out                 # stdout: <job_name>_<job_id>.out

# --- Resources ---
#SBATCH -N <n>                       # nodes
#SBATCH -n <n>                       # total tasks
#SBATCH -c <n>                       # cpus per task
#SBATCH --mem=<size>                 # memory per node
#SBATCH -G <n>                       # total GPUs
#SBATCH --gpus-per-node=<n>          # GPUs per node
#SBATCH -C <constraint>              # e.g., a100, h100

# --- Singularity ---
CONTAINER="./container.sif"
PROJECT="/scratch/user/$USER/<project>"

# --- Main ---
singularity exec --nv \
    --bind "${PROJECT}":/project \
    "$CONTAINER" \
    bash -c "cd /project && uv run python eval/<benchmark>/main.py "$@""
```

### Bash Style
- Always start with `#!/bin/bash` and `set -euo pipefail`
- Use `# ---` comments to separate sections: Job, Resources, Singularity, Main
- UPPER_CASE for script-level constants, lower_case for local variables inside functions
- Quote all variable expansions: `"$VAR"`, `"${ARRAY[@]}"`
- Use `$(command)` for command substitution, never backticks
- Long commands use trailing `\` for line continuation, with arguments indented one level

## Pilot Experiments
- For every project, make sure to generate a small sample of data and run a pilot experiment script to verify the code and environment
- Use `--num_samples` for small-scale dry runs before full evaluation
