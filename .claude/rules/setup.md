---
description: This file describes the global setup instructions for the project.
---

# Global Setup Instructions

All code runs on a HPC cluster (SLURM). Follow the instructions below to set up your environment.

## HPC

### Hardware

| Type | Model |
|------|-------|
| GPU  | h200, b200, h100 |
| GPU  | a100 (40g/80g), l40s, rtx6000 |
| GPU  | a40, a30, rtx3070, rtx2080 |
| CPU  | sapphire_rapids, icelake |
| CPU  | zen4, zen3, zen2 |
| CPU  | cascadelake, skylake_avx512 |

### Singularity

Containers can be used like any other application via an interactive shell or a batch job.

```bash
# Run the Container
singularity run ./container.sif

# Execute a command
singularity exec ./container.sif python hello.py

# Start a shell
singularity shell ./container.sif
Singularity container.sif:~>

# Run a container so that it has access to the GPU
singularity shell --nv ./gpu_enabled_container.sif
```

The following bind variable must be set to make such directories and their contents available to your containerized application:

```bash
singularity exec --bind /scratch/user/$USER/project:/my-project ./container.sif python /my-project/hello.py
singularity shell --bind /scratch/user/k1234567:/my-scratch,/scratch/prj/project:/my-project container.sif
```

### Cache

The cache should be allocated on `/scratch/$USER/`

```bash
export HF_HOME=/scratch/users/$USER/cache/huggingface
export VLLM_CACHE_ROOT=/scratch/users/$USER/cache/vllm
export TORCH_HOME=/scratch/users/$USER/cache/torch
export SINGULARITY_CACHEDIR=/scratch/users/$USER/cache/singularity
export UV_CACHE_DIR=/scratch/users/$USER/cache/uv
```

### Storage

| Path | Quota | Usage |
|------|-------|-------|
| /users/ | 50GB | Code, configs, small files |
| /scratch/ | 200GB | Job I/O, large datasets |

### Script

```sh
#!/bin/bash
# --- Job ---
#SBATCH -J <job_name>                # job name
#SBATCH -p <gpu|cpu>                 # partition
#SBATCH -t 30:00:00                  # time limit (max 48h)
#SBATCH -o %x_%j.out                # stdout: <job_name>_<job_id>.out

# --- Resources ---
#SBATCH -N <n>                       # nodes
#SBATCH -n <n>                       # total tasks
#SBATCH -c <n>                       # cpus per task
#SBATCH --mem=<size>                 # memory per node
#SBATCH -G <n>                       # total GPUs
#SBATCH --gpus-per-node=<n>          # GPUs per node
#SBATCH -C <constraint>             # e.g., a100, h100

# --- Notifications ---
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=<email>
```

## Environment

- Use `uv` to manage Python environments

### Common Libraries

| Category | Libraries |
|----------|-----------|
| Data & Computation | numpy, pandas, scipy |
| Visualization | matplotlib, seaborn |
| ML/DL | torch, transformers, datasets |
| LLM Inference | vllm |
| LLM API | litellm |
| Sandboxing | e2b, docker |
| Experiment Tracking | wandb |
| Utilities | tqdm, jsonlines |
