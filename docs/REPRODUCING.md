# Reproducing the staged Nanotron workflow

## Read-only review without a numerical environment

Clone with LFS smudging disabled when weights are unnecessary:

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/ntua-el21026/ntua-llama-500m-nanotron.git
cd ntua-llama-500m-nanotron
bash -n slurm/run_smoke_checkpoint.sbatch
python3 -m compileall -q scripts
```

Compilation checks Python syntax only and does not import PyTorch or Nanotron. The model directories currently contain LFS pointers; reading a pointer is not reading a tensor. The [companion artifacts](https://github.com/ntua-el21026/ntua-llama-500m-artifacts) have a preserved [12 November 2026 byte-verification record](ARTIFACT_VERIFICATION.md). That historical check downloaded all 388 objects and matched their hashes/sizes.

## Historical training environment

The Slurm templates target Leonardo, with four data-parallel workers and site-specific modules/paths. `scripts/prepare_env.sh` is a historical installer: it uses mutable package releases and Nanotron HEAD, and installs PyTorch/CUDA-dependent extensions. It is not a lockfile or a portable workstation setup. Read it before using it. The exact Nanotron source revision and resolved runtime must be recovered from the original environment before claiming replay.

Use an allocated, compatible managed GPU environment. Record Python, PyTorch, CUDA, GPU/driver, compiler, Nanotron commit and package versions before setup changes. Reuse a compatible provider PyTorch stack. Record any conflict instead of assuming the old module names define a working environment. Install additional packages only after an explicit compatible, pinned requirements manifest has been established.

## Declared configurations

All four main configurations use seed 42, data parallelism 4, tensor/pipeline parallelism 1 and sequence length 1024. The values below are declared schedules; selected checkpoints can occur earlier, and configured steps are not actual consumed-token measurements.

| Configuration | Train steps | Microbatch per replica | Accumulation per replica | Initial learning rate |
| --- | ---: | ---: | ---: | ---: |
| [Stage 1](../config/config_stage1.yaml) | 53,500 | 4 | 16 | 3e-4 |
| [Stage 2](../config/config_stage2.yaml) | 22,717 | 4 | 16 | 1.5e-4 |
| [Stage 3](../config/config_stage3.yaml) | 15,260 | 4 | 16 | 5e-5 |
| [Stage 4 SFT](../config/sft_stage3_smoltalk.yaml) | 5,000 | 1 | 4 | 1e-5 |

Stage-3 initialization loads Stage-2 weights without optimizer/scheduler state. SFT similarly initializes from Stage 3 and declares bfloat16. Keep these distinctions when reconstructing the curriculum rather than treating model-only exports as full-state resume checkpoints.

## Reproduction order

1. Read `config/config_stage1.yaml`, `config/config_stage2.yaml`, `config/config_stage3.yaml` and `config/sft_stage3_smoltalk.yaml`. Keep the originals as evidence; create local copies for current paths, allocations and output directories.
2. Obtain the required tokenizer and dataset versions. Pretraining references `meta-llama/Meta-Llama-3-8B` without a frozen tokenizer revision; SFT references a custom local tokenizer with a chat template. Recover and hash that exact tokenizer rather than substituting a different template.
3. Recover input data/cache/checkpoint fingerprints and actual consumed-token logs for each stage. Full training checkpoints, optimizer state, caches and Stage-1 evaluation results are incomplete in Git. Model-only exports do not provide optimizer-state resume.
4. Resolve the Nanotron revision and local patches described in [PROVENANCE.md](PROVENANCE.md). Use a separate smoke configuration to establish numerical compatibility before a training run.
5. For evaluation, inspect `scripts/stage3_prefill_cache.sh`, `scripts/stage3_eval_offline.sh`, and the corresponding stage files before use. Prefill is a separate dataset/network setup step; offline evaluation depends on those external caches and correct paths. Preserve task suite, split, prompt, normalization, selected checkpoint, seed and precision.
6. Export run metadata and new results into separate storage. Include Git/source hashes, full resolved config, package versions, actual hardware, data/tokenizer/model hashes, seeds, and the result status. Keep setup, oracle checks and downloads outside measured execution.

## Entry points after environment qualification

The historical training templates use four local workers. In an allocated compatible environment, from a separate working checkout with resolved data/checkpoint/tokenizer paths, the entry-point form is:

```bash
export CUDA_DEVICE_MAX_CONNECTIONS=1
torchrun --nproc_per_node=4 scripts/run_train.py --config-file config/config_stage3.yaml
```

The configuration is not a self-contained run request: its Leonardo paths, continuation checkpoint and data folders must exist first. The other stages use their corresponding main configurations. Inspect `slurm/run_stage3.sbatch` or `slurm/sft_stage3_smoltalk.sbatch` for the historical launch settings; account/partition/modules and `$HOME/Smol_Project` are site-specific. Do not submit them unchanged as a portable quick start.

Stage-3 evaluation separates network/cache preparation from offline compute-node submission:

```bash
bash scripts/stage3_prefill_cache.sh
STAGE3_STEPS="12000" STAGE3_INCLUDE_STAGE2_BRIDGE=0 bash scripts/stage3_eval_offline.sh
```

These commands perform real preparation/submission when run in the intended environment. The second invokes `sbatch`; it is not a local static check. The launcher reads `config/config_stage3.yaml`, expects checkpoints below `$SCRATCH/ckpts/stage3`, and requires `config.yaml`, `model_config.json` and `model/` in each selected checkpoint. Its default selection is steps 2000–12000 plus a Stage-2 bridge; the example narrows it explicitly.

`SCRATCH` must be defined. The backend also needs `NANOTRON_ROOT`; tokenizer/length and scoring batch-size controls include `TOKENIZER_NAME_OR_PATH`, `SEQ_LEN` and `EVAL_BATCH_SIZE`. Prefill supports `EVAL_DATA_DIR`, `EVAL_RESULTS_DIR` and `EVAL_LOG_DIR`, while the submit script constructs its own stage directories under `$SCRATCH/evaluation_results/stage3`. Keep both steps aligned. Stages 1, 2 and 4 have analogous scripts, but their suites and checkpoint defaults differ; inspect each rather than assuming identical scoring.

## Model-only conversion

`scripts/export_nanotron_ckpt_to_hf.py` accepts `--ckpt`, `--out`, `--tokenizer`, and `--dtype` (`bfloat16`, `float16`, `float32`). It needs a compatible numerical environment and real checkpoint tensors. Inspect its format assumptions first. These Nanotron model-only shards are not documented as a ready-to-load Transformers `from_pretrained` directory. The historical exporter hardcodes `rope_theta=500000.0` and BOS/EOS IDs 128000/128001, while the retained Stage-3 configuration declares `rope_theta=10000.0` and the SFT configuration declares BOS/EOS IDs 1/2. Resolve these semantic differences against the actual checkpoint and exact tokenizer before using the converter; a successful file export alone would not establish equivalence. The original exporter/configuration are preserved as evidence. Conversion and inference remain unqualified until executed against the recovered environment and exact tokenizer.
