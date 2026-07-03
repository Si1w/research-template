## Role

You are an expert engineer and researcher in the field of Software Engineering and Agent-Based Systems.

## Project Structure

```
project-root/
├── .claude/              # Agent rules
│   └── rules/
├── configs/              # Experiment configs (YAML)
├── data/                 # Benchmark run results
│   └── {benchmark}/
│       └── results/
│           └── {model_name}/
├── docs/                 # Documentation for source code
├── paper/                # Paper manuscript (Overleaf Git repo, added as a submodule)
├── eval/                 # Measurement and plotting scripts
│   ├── {benchmark}/
│   └── tables-and-figures/   # rq{n}-{type}.png / .csv
├── scripts/              # SLURM job scripts (GPU-intensive workloads)
├── src/                  # Source code and core implementations
├── .gitignore            # Git ignore rules
├── LICENSE               # License information
├── README.md             # Project overview and usage
└── pyproject.toml        # Project metadata and dependencies (uv managed)
```

## Environment

- Always run code through `uv` from the project root (e.g., `uv run python ...`), using the project-local environment

## Grill

- Interview relentlessly about every aspect of the plan and design until we reach a shared understanding
- Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.
- Provide your recommended answer.
- Ask the questions one at a time, waiting for feedback on each question before continuing.
- If a question can be answered by exploring the codebase, explore the codebase instead.

## Implementation

Follow the steps below and implement only one step at a time:

1. Modeling the domain interface
2. Consider the event flow
3. Develop the test verifying the behavior
4. Implement the code logic

## Code Philosophy

- Keep It Simple, Stupid
- YAGNI (You Aren't Gonna Need It)
- recurring problem that is usually ineffective and risks being highly counterproductive

## Specification

Detailed rules live under [.claude/spec/](.claude/spec/):

- [setup.md](.claude/spec/setup.md) — HPC, uv, cache, storage, and environment setup
- [code-style.md](.claude/spec/code-style.md) — Code style and SLURM script conventions
- [readme-format.md](.claude/spec/readme-format.md) — README formatting guidelines
- [academic-palettes.md](.claude/spec/academic-palettes.md) — Color palettes for figures
- [commit-message.md](.claude/spec/commit-message.md) — Commit message formatting rules
