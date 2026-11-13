# Source and asset provenance

The report credits Evaggelos Asitzoglou, Panagiotis Avgerinos, Michail-Athanasios Peppas, Andreas Saounatsos, and Giannis Chatzigeorgiadis.

## Adapted training source

`scripts/run_train.py` adapts the [Hugging Face Nanotron training script](https://github.com/huggingface/nanotron/blob/main/run_train.py). Nanotron is distributed under [Apache-2.0](https://github.com/huggingface/nanotron/blob/main/LICENSE); a local copy is retained in [LICENSES/Apache-2.0.txt](../LICENSES/Apache-2.0.txt). Local code includes project-specific dataset/SFT integration and compatibility edits.

The exact upstream commit was not recorded by the historical `pip install git+https://github.com/huggingface/nanotron.git` command. The current upstream branch link identifies the project, not the revision used for the experiment. Recover the historical revision and local patches to reproduce the training source.

## Model, tokenizer and data

The two selected artifacts are `stage3_12000_modelonly` and `sft_stage3_smoltalk_run1_5000_modelonly`. Their pointer OIDs identify expected tensor-file content. The earlier pointer-only preparation is recorded in `release-provenance.json`; the later [12 November byte-verification record](ARTIFACT_VERIFICATION.md) documents companion downloads, SHA-256/size and serialization checks. Both records are retained unchanged. Neither establishes numerical execution or complete training provenance. Export/config/report hashes remain in `release-provenance.json`.

Configurations reference the Meta Llama 3 tokenizer and an external custom SFT tokenizer. Exact tokenizer and dataset revisions are required for reproduction.

## Preserved evidence

The report, its LaTeX source, canonical training configs and selected result artifacts remain historical evidence. Token-total inconsistencies and weak evaluations are documented in the [model card](MODEL_CARD.md); the historical files have not been rewritten to imply stronger results. The [compact package](https://github.com/ntua-el21026/ntua-llama-500m-artifacts) and this workflow are one team project.
