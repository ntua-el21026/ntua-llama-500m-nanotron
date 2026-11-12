# Reproducing the staged Nanotron workflow

## Read-only review without a numerical environment

Clone with LFS smudging disabled when weights are unnecessary:

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/ntua-el21026/NTUA-Llama_500M_Nanotron_Full.git
cd NTUA-Llama_500M_Nanotron_Full
bash -n slurm/run_smoke_checkpoint.sbatch
python3 -m compileall -q scripts
```

Compilation checks Python syntax only and does not import PyTorch or Nanotron. The model directories currently contain LFS pointers; reading a pointer is not reading a tensor. The [compact release](https://github.com/ntua-el21026/NTUA-Llama_500M_Nanotron) had working object-download metadata in the audit; verify transfer and SHA-256 separately in an authorized model environment before use.

## Historical training environment

The Slurm templates target Leonardo, with four data-parallel workers and site-specific modules/paths. `scripts/prepare_env.sh` is a historical installer: it uses mutable package releases and Nanotron HEAD, and installs PyTorch/CUDA-dependent extensions. It is not a lockfile or a portable workstation setup. Read it before using it; this preparation did not run it or reconstruct the historical package versions. The exact Nanotron source revision and resolved runtime must be recovered from the original environment before claiming replay.

Use an allocated, compatible managed GPU environment. Record Python, PyTorch, CUDA, GPU/driver, compiler, Nanotron commit and package versions before setup changes. Reuse a compatible provider PyTorch stack. Record any conflict instead of assuming the old module names define a working environment. Install additional packages only after an explicit compatible, pinned requirements manifest has been established. No historical package pins are invented here.

## Reproduction order

1. Read `config/config_stage1.yaml`, `config/config_stage2.yaml`, `config/config_stage3.yaml` and `config/sft_stage3_smoltalk.yaml`. Keep the originals as evidence; create local copies for current paths, allocations and output directories.
2. Obtain authorized tokenizer and dataset versions. Pretraining references `meta-llama/Meta-Llama-3-8B` without a frozen tokenizer revision; SFT references a custom local tokenizer with a chat template. Recover and hash that exact tokenizer rather than substituting a different template.
3. Recover input data/cache/checkpoint fingerprints and actual consumed-token logs for each stage. Full training checkpoints, optimizer state, caches and Stage-1 evaluation results are incomplete in Git. Model-only exports do not provide optimizer-state resume.
4. Resolve the Nanotron revision and local patches described in [PROVENANCE.md](PROVENANCE.md). Use a separate smoke configuration to establish numerical compatibility before a training run. No job is launched by these instructions automatically.
5. For evaluation, inspect `scripts/stage3_prefill_cache.sh`, `scripts/stage3_eval_offline.sh`, and the corresponding stage files before use. Prefill is a separate dataset/network setup step; offline evaluation depends on those external caches and correct paths. Preserve task suite, split, prompt, normalization, selected checkpoint, seed and precision.
6. Export run metadata and new results into separate storage. Include Git/source hashes, full resolved config, package versions, actual hardware, data/tokenizer/model hashes, seeds, and the result status. Keep setup, oracle checks and downloads outside measured execution.

## Model-only conversion

`scripts/export_nanotron_ckpt_to_hf.py` accepts `--ckpt`, `--out`, `--tokenizer`, and `--dtype` (`bfloat16`, `float16`, `float32`). It needs a compatible numerical environment and real checkpoint tensors. Inspect its format assumptions first. These Nanotron model-only shards are not documented as a ready-to-load Transformers `from_pretrained` directory. The historical exporter hardcodes `rope_theta=500000.0` and BOS/EOS IDs 128000/128001, while the retained Stage-3 configuration declares `rope_theta=10000.0` and the SFT configuration declares BOS/EOS IDs 1/2. Resolve these semantic differences against the actual checkpoint and exact tokenizer before using the converter; a successful file export alone would not establish equivalence. The original exporter/configuration are preserved as evidence. Conversion and inference remain unqualified until executed against the recovered environment and exact tokenizer.
