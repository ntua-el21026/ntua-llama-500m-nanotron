# Evaluation Folder

This folder contains the multi-stage evaluation implementation and selected historical outputs.

## Core runners

- `run_eval_stage1.py`
- `run_eval_stage2.py`
- `run_eval_stage3.py`
- `run_eval_stage4.py`

Each runner prepares cached inputs, runs scoring through backend scripts, and writes per-checkpoint JSON outputs.

## Task suites

`task_suites/` defines stage-specific evaluation suites:

- Tier 2: perplexity slices
- Tier 3: conditional-likelihood QA tasks
- Tier 4 (Stage 4): SFT-native prompt checks

## Results and diagrams

- `_runs_stage2/`, `_runs_stage3/`, `_runs_stage4/`: fetched results, selected logs and stage diagrams. Stage-1 outputs are not included in this checkout.
- `paper/`: scripts and figures used for report-ready plots

Primary output files per checkpoint are:

- `tier2_ppl.json`
- `tier3_cf.json`
- `tier4_sft_native.json` (Stage 4)

## Interpreting and rerunning evaluation

The runners orchestrate task preparation and scoring backends. Perplexity slices, conditional-likelihood QA and SFT-native checks use stage-specific suites; matching a metric name alone does not establish a comparable evaluation protocol. [DETAILS.md](DETAILS.md) retains the detailed pipeline description, while the [model card](../docs/MODEL_CARD.md) qualifies reported results.

Read the [reproduction guide](../docs/REPRODUCING.md#entry-points-after-environment-qualification) for cache setup and checkpoint/path requirements. Offline wrappers submit Slurm jobs; they do not make a workstation self-contained. No numerical evaluator was run for this documentation update.
