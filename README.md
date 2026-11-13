# NTUA LLaMA-style 500M — Team 12 workflow

Training, fine-tuning and evaluation workflow for the NTUA ECE **Pattern Recognition** course, Winter 2025–2026. The project studies a staged curriculum for a LLaMA-style language model described as 500M parameters, implemented with Hugging Face Nanotron and Leonardo Slurm jobs.

The report credits **Evaggelos Asitzoglou, Panagiotis Avgerinos, Michail-Athanasios Peppas, Andreas Saounatsos and Giannis Chatzigeorgiadis**. This is shared Team 12 work; individual implementation responsibilities have not been reconstructed from the imported history.

## Start here

- [Report PDF](docs/paper/Report.pdf) and [LaTeX source](docs/paper/main.tex): original method, analysis and figures.
- [Reproduction guide](docs/REPRODUCING.md): configuration values, execution sequence and missing prerequisites.
- [Model card](docs/MODEL_CARD.md): selected checkpoints, evaluation scope and scientific limitations.
- [Provenance](docs/PROVENANCE.md) and [historical artifact verification](docs/ARTIFACT_VERIFICATION.md): source/asset origins and the scope of byte-level checks.
- [Companion artifacts repository](https://github.com/ntua-el21026/ntua-llama-500m-artifacts): the same report, four main configurations and selected model-only checkpoints. The two repositories describe one team project.

## Implemented workflow

| Stage | Purpose | Main configuration |
| --- | --- | --- |
| 1 | Base pretraining | [config_stage1.yaml](config/config_stage1.yaml) |
| 2 | Code/math-oriented continuation | [config_stage2.yaml](config/config_stage2.yaml) |
| 3 | Reasoning-oriented continuation | [config_stage3.yaml](config/config_stage3.yaml) |
| 4 | Supervised fine-tuning on smol-smoltalk | [sft_stage3_smoltalk.yaml](config/sft_stage3_smoltalk.yaml) |

The main configurations declare 32 layers, hidden size 1024, intermediate size 2816, 16 attention heads, four key/value heads, tied embeddings and vocabulary size 128256. Training sequences are 1024 tokens; the model configuration allows positions up to 2048. These are configuration facts, not evidence that every configured step completed.

[Data-preparation scripts](scripts/) construct the stage inputs. The adapted [training entry point](scripts/run_train.py) delegates model/distributed training to Nanotron and integrates staged datasets and SFT. [Slurm templates](slurm/) provide the historical cluster launch environment. [Evaluation runners](evaluation/) prepare task inputs, invoke project backends and write per-checkpoint results for perplexity, conditional-likelihood QA and SFT-native behavior checks.

The repository preserves fetched Stage-2, Stage-3 and Stage-4 outputs and diagrams under `evaluation/_runs_stage*/`; Stage-1 outputs are not included. Selected model-only exports are indexed in [final_model/README.md](final_model/README.md). Full training checkpoints, dataset caches and scratch logs are external.

## Browsing and execution prerequisites

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/ntua-el21026/ntua-llama-500m-nanotron.git
cd ntua-llama-500m-nanotron
```

This permits source/report review without downloading weights or installing a numerical stack. Training requires a compatible allocated GPU environment, the actual Nanotron revision and local patches, authorized datasets/tokenizers, recovered input caches and adapted cluster paths. There is no frozen, resolved dependency environment. Read [REPRODUCING.md](docs/REPRODUCING.md) before using historical installers or job templates.

The smoke checkpoint template [run_smoke_checkpoint.sbatch](slurm/run_smoke_checkpoint.sbatch) retains offline W&B use. Online use requires an externally supplied `WANDB_API_KEY`; never embed credentials in scheduler files. A previously committed key requires owner-side revocation/rotation; removing it from the current file does not erase history. This documentation update did not reopen or perform credential operations.

## Evidence and limitations

The [12 November 2026 verification record](docs/ARTIFACT_VERIFICATION.md) reports that all 388 companion-repository LFS objects, totaling 1,968,528,560 bytes, were downloaded and matched their SHA-256/size pointers. Their safetensors structure was checked, and this repository's pointers matched. Its own LFS endpoint returned missing-object errors in that historical check. That record is preserved; the present documentation pass did not repeat downloads or infer current endpoint availability.

Byte identity does not qualify conversion, inference or reported benchmarks. The exact custom SFT tokenizer is absent; upstream/dependency pins remain unresolved; the exporter and retained configurations disagree on RoPE/BOS/EOS settings. The report's 22B/23.13B token totals also require reconciliation. Evaluation protocols differ between stages, and the retained 50-prompt SFT-native result includes 12% all-checks pass and 0% JSON validity. See the model card before comparing scores.

This update performed static documentation/source checks only. It introduces no new training, evaluation or performance claim and preserves original configs, reports, results and checkpoints.

## License and attribution

[MIT](LICENSE) applies to original team contributions within the contributors' rights. Adapted Nanotron code has separate [Apache-2.0 material](LICENSES/Apache-2.0.txt). Tokenizers, datasets, pretrained assets and model weights retain their own access and redistribution requirements; the project license does not establish a blanket grant. [PROVENANCE.md](docs/PROVENANCE.md) records remaining source and asset questions.
