# Source and asset provenance

The report credits Evaggelos Asitzoglou, Panagiotis Avgerinos, Michail-Athanasios Peppas, Andreas Saounatsos, and Giannis Chatzigeorgiadis. Original team contributions are offered under the root MIT license to the extent the contributors hold the relevant rights. Contribution-level ownership and teammate agreement should be documented before a consolidated release.

## Adapted training source

`scripts/run_train.py` adapts the [Hugging Face Nanotron training script](https://github.com/huggingface/nanotron/blob/main/run_train.py). Nanotron is distributed under [Apache-2.0](https://github.com/huggingface/nanotron/blob/main/LICENSE); a local copy is retained in [LICENSES/Apache-2.0.txt](../LICENSES/Apache-2.0.txt). The adaptation notice was added during portfolio preparation. Local code includes project-specific dataset/SFT integration and compatibility edits; an exhaustive upstream patch comparison is pending.

The exact upstream commit was not recorded by the historical `pip install git+https://github.com/huggingface/nanotron.git` command. The current upstream branch link identifies the project, not the revision used for the experiment. Recover that revision and any original copyright/NOTICE material from the historical environment before asserting complete provenance or a reproducible source pin. The added attribution does not certify that this unresolved review is complete.

## Model, tokenizer and data

The two selected artifacts are `stage3_12000_modelonly` and `sft_stage3_smoltalk_run1_5000_modelonly`. Their pointer OIDs identify expected tensor-file content; actual tensors were not downloaded or validated during preparation. Export/config/report hashes appear in `release-provenance.json`.

Configurations reference the Meta Llama 3 tokenizer and an external custom SFT tokenizer. Apply the tokenizer's original access/license conditions; the project's MIT notice does not replace them. Dataset access, redistribution rights and exact revisions must be recorded separately. Model-weight license scope has not been independently established from these configs alone. Do not infer tokenizer/model/dataset rights from framework licensing.

## Preserved evidence

The report, its LaTeX source, canonical training configs and selected result artifacts remain historical evidence. Token-total inconsistencies and weak evaluations are documented in the [model card](MODEL_CARD.md); the historical files have not been rewritten to imply stronger results. The [compact package](https://github.com/ntua-el21026/NTUA-Llama_500M_Nanotron) and this workflow are one team project.
