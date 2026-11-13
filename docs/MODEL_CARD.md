# Selected model artifacts and limitations

This is a Team 12 NTUA Pattern Recognition coursework model, Winter 2025–2026. It is a LLaMA-style model described as 500M parameters; the versioned configuration uses 32 layers, hidden size 1024, intermediate size 2816, 16 attention heads, four key/value heads and vocabulary size 128256.

The selected pre-SFT checkpoint is `stage3_12000_modelonly`; the selected SFT checkpoint is `sft_stage3_smoltalk_run1_5000_modelonly`. Model-only shards support artifact comparison; they do not contain the full optimizer/scheduler state for training resume. Seed 42 and four data-parallel workers appear in the canonical configs. SFT declares bfloat16; actual training/inference precision and full hardware must be obtained from the run environment rather than inferred from a filename.

## Evaluation evidence

The Full repository preserves Stage-3 step-12000 conditional-likelihood results: ARC-Challenge accuracy 0.2475 on 299 validation examples and MMLU 0.2391 on 1,531 examples. Continuation likelihood is normalized by character count, and stage task suites differ. Direct comparisons require matching task set, split, prompt, normalization and sampling; stage macro scores and external headline baselines are not automatically comparable.

The final SFT-native artifact evaluates 50 prompts, with 12% passing all checks and 0% JSON validity. These negative results remain useful evidence about instruction following. They do not establish general reasoning or instruction-following superiority.

## Unresolved training budget

The report states 22B tokens in one passage and 23.13B before SFT elsewhere; figure-stage budgets also differ. Intended budgets, actual consumed tokens and the selected checkpoint's exposure must be reconciled from training logs. No single verified consumed-token total is asserted here.

## Intended use and artifact access

Use for educational inspection and controlled evaluation after qualifying the environment and obtaining the exact tokenizer. The workflow repository’s LFS endpoint reported 388 missing objects on 12 November 2026. The companion artifact verification is recorded below.

See the linked workflow/reproduction documentation for data/cache dependencies and source revision gaps.

## Later byte verification

The [12 November artifact check](ARTIFACT_VERIFICATION.md) downloaded and verified
all Compact objects and compared the Full pointers. It supersedes the earlier
pointer-only availability observation above; conversion, exact tokenizer recovery
and numerical evaluation remain unqualified.
