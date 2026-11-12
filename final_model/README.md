# Final Model Folder

This folder stores selected historical model-only exports.

## Included artifacts

- `stage3_12000_modelonly/`: Stage 3 bridge baseline export
- `sft_stage3_smoltalk_run1_5000_modelonly/`: final SFT export

These directories are intended for reproducible comparison between pre-SFT and post-SFT checkpoints.

Model files are stored in sharded weight format (e.g., `.safetensors`) together with configuration metadata.

## Availability and compatibility

The 12 November 2026 audit found these 388 LFS objects unavailable at the Full
endpoint. The [compact release](https://github.com/ntua-el21026/NTUA-Llama_500M_Nanotron)
returned download metadata for the same OIDs. Neither preparation nor the audit
validated transfers or tensors. These are model-only Nanotron exports; compatible
conversion/runtime and the exact tokenizer are required before inference. See
[MODEL_CARD.md](../docs/MODEL_CARD.md) for the selected checkpoints and limitations.
