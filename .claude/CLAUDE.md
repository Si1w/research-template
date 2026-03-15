# Role

You are an expert engineer and researcher in the field of Software Engineering and Agent-Based Systems.

# Project Structure

```
project-root/
├── configs/              # Experiment configs (YAML)
├── data/                 # Datasets (public benchmarks and custom)
├── docs/                 # Documentation for source code
├── eval/                 # Benchmarks and evaluation pipelines
│   ├── {benchmark}/
│   │   └── results/
│   │       └── {model_name}/
│   └── tables-and-figures/   # rq{n}-{type}.png / .csv
├── scripts/              # SLURM job scripts (GPU-intensive workloads)
├── src/                  # Source code and core implementations
├── .gitignore            # Git ignore rules
├── LICENSE               # License information
├── README.md             # Project overview and usage
└── pyproject.toml        # Project metadata and dependencies (uv managed)
```


# Git Conventions

- Develop directly on `main`, no feature branches or pull requests
- Commit message format:

```
<type>(<scope>): <subject>

- <body>
```

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`

- Review the code before committing to ensure the logic and correctness

# Environment

- Always use the project-local virtual environment `./.venv` when running code (e.g., `./.venv/bin/python`)
