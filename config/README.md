# Config Folder

This folder contains the canonical YAML configurations for training and SFT.

## Main configs

- `config_stage1.yaml`: Stage 1 pretraining
- `config_stage2.yaml`: Stage 2 pretraining
- `config_stage3.yaml`: Stage 3 pretraining
- `sft_stage3_smoltalk.yaml`: Stage 4 SFT run configuration

## Supporting configs

- `DATASET_STAGE_2.yaml`, `DATASET_STAGE_3.yaml`: dataset definitions for pretraining stages
- `*_smoke*.yaml`, `smoke_*.yaml`: smoke-test and checkpoint-resume/save configs
- `config_tiny_llama.yaml`, `config_stage1_16L.yaml`: alternative experiment variants

These files preserve the declared experiment configurations. They contain historical cluster paths and do not pin the complete runtime, tokenizer or data revisions. Use the [reproduction guide](../docs/REPRODUCING.md) to interpret schedules and recover missing inputs; adapt paths in a separate working copy.
