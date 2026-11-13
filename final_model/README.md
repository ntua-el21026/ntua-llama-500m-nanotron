# Selected historical model-only exports

- [stage3_12000_modelonly/](stage3_12000_modelonly/): selected pre-SFT checkpoint, Stage-3 step 12,000.
- [sft_stage3_smoltalk_run1_5000_modelonly/](sft_stage3_smoltalk_run1_5000_modelonly/): selected SFT checkpoint, step 5,000.

Each directory contains configuration/checkpoint metadata and 194 Git LFS-tracked safetensors shards. A smudge-disabled clone contains pointers rather than tensor payloads. The exports omit full optimizer/scheduler state and are not a qualified Transformers `from_pretrained` package.

The [12 November 2026 artifact record](../docs/ARTIFACT_VERIFICATION.md) reports verified companion downloads and identical pointers here; this repository's endpoint returned missing objects during that check. Obtain artifacts through the [companion repository](https://github.com/ntua-el21026/ntua-llama-500m-artifacts) after checking current access.

Read the [model card](../docs/MODEL_CARD.md) and [conversion caveats](../docs/REPRODUCING.md#model-only-conversion) before use. The exact SFT tokenizer is missing, and exporter/configuration differences remain unresolved. Preserve the selected files and their provenance; a successful transfer alone does not validate inference or reported scores.
