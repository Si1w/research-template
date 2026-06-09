# Research Completeness Checklist

Use this checklist when developing a research idea, writing a proposal, designing an experiment, or preparing a paper draft. Maintain it as the study evolves. Every unchecked item must become either a concrete design change, a scoped-down claim, or a TODO in the proposal/paper.

## Reference Standards

Load the active `brainstorming` skill before using this checklist. For empirical rigor, consult the active `ese-review` skill reference set, normally under `~/.claude/skills/ese-review/references/`.

Always consult:

- `general/general-standard.md`
- `general/engineering-research.md` for any study that builds and evaluates an artifact

Load specific standards as needed:

| Study Type | Reference |
|------------|-----------|
| Benchmarking | `quantitative/benchmarking.md` |
| Controlled experiment with human participants | `quantitative/experiment.md` |
| Repository mining | `quantitative/repository-mining.md` |
| Data science / exploratory quantitative study | `quantitative/data-science.md` |
| Optimization study | `quantitative/optimization-study.md` |
| Quantitative simulation | `quantitative/quantitative-simulation.md` |
| Questionnaire survey | `quantitative/questionnaire-survey.md` |
| Case study | `qualitative/case-study.md` |
| Grounded theory | `qualitative/grounded-theory.md` |
| Systematic review | `literature-review/systematic-review.md` |
| Replication | `other/replication.md` |
| Mixed methods | `general/mixed-methods.md` |

## Problem And Motivation

- [ ] The problem is stated in one concrete sentence: who is affected, in what context, what breaks, and what consequence follows.
- [ ] The severity is justified with evidence or a credible argument, not just intuition.
- [ ] Existing practice is described, including why it is insufficient.
- [ ] The proposal explains why now is the right time: new models, tools, datasets, scale, or observed failure modes.
- [ ] The expected before/after change is concrete enough that a reviewer can judge whether the work matters.

## Existing Work And Positioning

- [ ] Zotero, local notes, and external literature have been searched for direct and partial overlap.
- [ ] The proposal explains why the problem has not been solved satisfactorily, not merely that no one used the same technique.
- [ ] State-of-the-art alternatives, baselines, or benchmarks are identified.
- [ ] Any missing direct comparison has an explicit rationale.
- [ ] The novelty claim is scoped to what the study can actually demonstrate.

## Study Classification

- [ ] Planned study type(s) are classified using empirical standards terminology.
- [ ] General standard has been checked.
- [ ] Engineering-research standard has been checked if an artifact is proposed.
- [ ] Method-specific standards have been checked for every applicable study type.
- [ ] Every essential attribute from the selected standards is either satisfied by the design or recorded as a gap.

## Research Questions And Claims

- [ ] Each RQ is answerable by the planned data and analysis.
- [ ] Each experiment or analysis maps back to at least one RQ.
- [ ] Each expected contribution is supported by a planned measurement or argument.
- [ ] The claim scope matches the study breadth; single-benchmark studies do not claim broad generality.
- [ ] Success and failure thresholds are defined before running the full study.

## Study Design

- [ ] Data sources, benchmark tasks, subjects, or repositories are specified.
- [ ] Inclusion/exclusion criteria are explicit.
- [ ] Baselines and ablations are sufficient to isolate the claimed contribution.
- [ ] Metrics are defined with units and interpretation.
- [ ] The minimum viable experiment is defined: data, metric, threshold, and what failure means.
- [ ] Technical risks and feasibility blockers are listed with mitigation plans.

## Measurement And Analysis

- [ ] Random seeds, sampling strategy, and repetitions are planned.
- [ ] Statistical analysis is planned before data collection, including effect sizes and confidence intervals where appropriate.
- [ ] Assumptions of statistical tests will be checked and reported.
- [ ] The analysis keeps raw results and performs offline aggregation.
- [ ] Visualizations and tables are designed to support claims, not just decorate results.

## Benchmarking Specific Checks

- [ ] Benchmark selection or new benchmark design is justified for relevance and timeliness.
- [ ] Workload, task sample, or usage profile is specified enough for replication.
- [ ] Benchmark configurations compete fairly without artificial limitations.
- [ ] Stability is assessed with enough repetitions or justified if not.
- [ ] Construct validity is discussed: the benchmark measures what the paper says it measures.

## Human-Participant Specific Checks

- [ ] Hypotheses, independent variables, dependent variables, and controls are explicit.
- [ ] Participant characteristics and recruitment plan are specified.
- [ ] Sample size rationale is provided, such as power analysis or fixed-population justification.
- [ ] Random assignment or the reason for not using it is described.
- [ ] Ethics, incentives, consent, and data handling are planned.

## Reproducibility

- [ ] Code, prompts, configs, seeds, scripts, and environment assumptions are recorded.
- [ ] Data availability is planned, or non-release constraints are justified.
- [ ] Evaluation can be rerun with small `--num_samples` pilots and full runs.
- [ ] Result files are stored under `data/{benchmark}/results/{model_name}/`.
- [ ] Tables and figures are generated by Python scripts under `eval/{benchmark}/` and written to `eval/tables-and-figures/`.

## Threats And Limitations

- [ ] Construct, internal, conclusion, and external validity are considered where applicable.
- [ ] Limitations are tied to actual design choices, not listed generically.
- [ ] Alternative interpretations of expected results are recorded.
- [ ] Risks, harms, burdens, or unintended consequences are acknowledged.
- [ ] Conclusions will be linked directly to RQs and explicit evidence.

## Gap Log

Record unresolved issues here. Do not delete a gap without replacing it with a decision, design change, or scoped-down claim.

| Date | Gap | Resolution / TODO |
|------|-----|-------------------|
| TODO | TODO | TODO |
