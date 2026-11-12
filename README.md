# NTUA-LLaMA-500M Nanotron (Full Repository)

Official Team 12 repository for the NTUA ECE course "Pattern Recognition" (Winter 2025-2026).

## Scope

This repository contains the full project workflow for:

- Stage 1 pretraining
- Stage 2 high-quality code/math continuation
- Stage 3 reasoning continuation
- Stage 4 supervised fine-tuning (SFT)
- Tiered evaluation, analysis, and paper assets

## Repository Layout

- `config/`: canonical training and SFT YAML configurations
- `scripts/`: data preparation, prefill, offline evaluation, submission, and backends
- `slurm/`: Slurm job templates used on Leonardo
- `evaluation/`: stage evaluators, task suites, fetched results, and diagrams
- `docs/`: paper source, report artifact, and maintenance utilities
- `final_model/`: selected model-only exported weights used as final artifacts

## Notes

- This repo is the full project artifact set (code + configs + selected outputs).
- Run-specific scratch logs and temporary caches are intentionally excluded.
- Original contributions: MIT (`LICENSE`); third-party and model assets have separate terms described below.

## Project status and reproduction

Team 12 developed this coursework project in Winter 2025–2026. The report credits Evaggelos Asitzoglou, Panagiotis Avgerinos, Michail-Athanasios Peppas, Andreas Saounatsos, and Giannis Chatzigeorgiadis. This repository presents the shared implementation; individual responsibilities have not been reconstructed from the imported history.

The workflow uses Hugging Face Nanotron for distributed training. Start with [reproduction instructions](docs/REPRODUCING.md), the [model card](docs/MODEL_CARD.md), and [source and asset provenance](docs/PROVENANCE.md). The [compact release](https://github.com/ntua-el21026/NTUA-Llama_500M_Nanotron) contains the same selected model-only exports and report; these are two packages of one project.

A 12 November 2026 audit checked download metadata for all 388 unique LFS objects (1,968,528,560 logical bytes): this Full endpoint reported every object missing, while the compact endpoint returned download actions. This preparation rechecked local pointer metadata only. Neither check proves successful transfer, tensor integrity, inference, or training reproduction. Avoid downloading weights merely to browse the code.

The report retains inconsistent total-token statements and mixed evaluation protocols. The model card explains these bounds, including weak SFT results. No new numerical or performance validation accompanies the local preparation.

## Credentials and license scope

The smoke checkpoint template is now `slurm/run_smoke_checkpoint.sbatch`. It keeps W&B offline and contains no API-key literal. Online use requires an externally supplied `WANDB_API_KEY`; never place credentials in scheduler files. A key previously committed to this repository needs owner-side revocation or rotation. Removing the current assignment does not erase its history.

The root MIT license applies to original contributions for which the authors hold rights. Adapted Nanotron code carries separate Apache-2.0 attribution and license material. Tokenizers, datasets, third-party code, and model exports require their own provenance and terms; the root license does not establish those rights. See [PROVENANCE.md](docs/PROVENANCE.md).

The [artifact verification record](docs/ARTIFACT_VERIFICATION.md) confirms the
Compact checkpoint bytes and matching Full pointers, with explicit tokenizer,
conversion, inference and licensing limits.
